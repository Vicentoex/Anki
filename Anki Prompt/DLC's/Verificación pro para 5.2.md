
**Instrucción resumen (añadir al prompt principal)**  
`Ejecuta el PROCEDIMIENTO FINAL para el banco de preguntas presente en la conversación siguiendo estrictamente las fases A→E y los checkpoints. Si el usuario incluye la frase EXACTA "AUTORIZO EJECUCIÓN COMPLETA", puedes aplicar automáticamente las correcciones propuestas en Fase C sin pedir confirmación adicional.`

---

## PARÁMETROS OBLIGATORIOS (reiterados)

- `Hard` = versión más seca y mínima (obligatoria para cada ítem modificado).
    
- Mantener fidelidad absoluta al documento fuente (cero información no textual).
    
- **Salida final** en el chat: **tabla Markdown** con columnas: `Nº | Pregunta | Opción 1 | Opción 2 | Opción 3 | Answer | Notas`.
    
    - **Condición**: `Answer` debe ser exactamente (byte-a-byte) el texto de la opción correcta (por ejemplo `A: Texto...`).
        

---

## CHECKPOINTS Y FORMATO JSON (obligatorio)

Cada checkpoint debe imprimirse exactamente como JSON con este esquema mínimo:

```json
{
  "phase": "FASE X - Nombre",
  "status": "OK" | "FAIL",
  "metrics": { /* métricas específicas de la fase */ },
  "actions": "Descripción breve de correcciones aplicadas o por aplicar"
}
```

No continuar hasta que `status` sea `"OK"`. En caso de `"FAIL"`, aplicar correcciones automáticas (hasta 2 intentos); si persiste, marcar `INCOMPLETO` y detener.

---

## FASE A — Segundo pase (revisión en cascada) — CHECKPOINT A

**Objetivo**: detectar duplicados, solapamientos de entidades y violaciones de uniformidad; evaluar Bloom y balance de posiciones.

### 1) Hash / similitud semántica por pregunta

- **Método preferido**: embeddings + cosine similarity.
    
- **Fallback**: TF-IDF pairwise similitudes (funciones incluidas abajo).
    
- **Umbral duplicado**: coseno > 0.70 ⇒ marcar como `DUPLICADO`.
    
    - Si coseno > 0.85 ⇒ recomendar eliminación automática (o reescritura urgente).
        

**Código (embeddings / TF-IDF)**

```python
# embeddings preferred (sentence-transformers)
from sentence_transformers import SentenceTransformer
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np

def load_embedding_model(name="paraphrase-multilingual-MiniLM-L12-v2"):
    return SentenceTransformer(name)

def semantic_sim_matrix(texts, model=None):
    if model:
        embs = model.encode(texts, convert_to_numpy=True)
        return cosine_similarity(embs)
    else:
        # fallback TF-IDF
        from sklearn.feature_extraction.text import TfidfVectorizer
        vect = TfidfVectorizer(ngram_range=(1,2), stop_words='spanish', max_features=50000)
        X = vect.fit_transform(texts)
        return cosine_similarity(X)
```

### 2) Solapamiento de entidades clave entre pares

- **Método preferido**: spaCy NER (es_core_news_sm / lg).
    
- **Fallback**: extracción de sustantivos o tokens capitalizados y normalización.
    
- **Métrica**: `overlap_ratio = |entities_interseccion| / min(|entities_i|, |entities_j|)`.
    
    - Si `overlap_ratio > 0.5` ⇒ marcar `SOLAPAMIENTO_CRÍTICO`.
        

**Código (NER + fallback)**

```python
# NER preferred with spaCy
import spacy
try:
    nlp = spacy.load("es_core_news_sm")
except:
    nlp = None

def extract_entities(text):
    if nlp:
        doc = nlp(text)
        ents = set([ent.text.strip().lower() for ent in doc.ents if len(ent.text.strip())>1])
        return ents
    else:
        # fallback: capitalized token sequences and nouns (very simple heuristic)
        tokens = [t for t in text.replace('\n',' ').split() if t[0].isupper()]
        return set([t.strip('.,;:()[]') .lower() for t in tokens])
```

### 3) Uniformidad de longitud de opciones

- **Tolerancia**: ±10% en número de caracteres entre las 3 opciones de cada pregunta.
    
- Si violación → marcar `LENGTH_VIOLATION`.
    

### 4) Validación distribución Bloom target

- Targets:
    
    - Recordar = 50% ±5
        
    - Comprender = 30% ±5
        
    - Aplicar = 20% ±2
        
- Calcular distribución actual y marcar desviaciones.
    

### 5) Balance de posición de respuestas correctas

- Target: 33% por columna ±5% (i.e., A/B/C ≈ 33% each).
    
- Calcular y marcar desviaciones.
    

**CHECKPOINT A (ejemplo de métricas)**

```json
{
 "phase":"FASE A - Revisión en cascada",
 "status":"OK" | "FAIL",
 "metrics": {
    "pairs_checked": 120,
    "duplicates_found": 3,
    "duplicate_pairs":[[1,7,0.83],[4,12,0.75]],
    "solapamientos_criticos": [[2,5,0.67]],
    "length_violations": 5,
    "bloom_distribution": {"Recordar":0.66, "Comprender":0.29, "Aplicar":0.05},
    "answer_position_balance": {"A":0.40,"B":0.30,"C":0.30}
 },
 "actions":"Descripción breve"
}
```

---

## Si detectas `DUPLICADOS` o `SOLAPAMIENTO_CRÍTICO` — propuestas obligatorias

- Para cada **DUPLICADO** (cos > 0.7):
    
    - Proponer explícitamente una de dos acciones (señalar la preferencia y motivo):
        
        - `ELIMINAR` (explicar qué hueco provoca y justificante)
            
        - `REESCRIBIR` (entregar propuesta de reescritura HARD que mantenga fidelidad)
            
- Para cada **SOLAPAMIENTO_CRÍTICO** (>50%):
    
    - Proponer `MATIZAR` (modificar enunciado o opciones para centrarse en matiz distinto) o `CONVERTIR` (convertir en otro tipo de ítem: p. ej. de definición → ejemplo).
        

Las propuestas deben presentarse en formato claro: par (i,j), tipo_propuesta, texto_propuesto (si REESCRIBIR o MATIZAR), justificación breve.

---

## FASE B — Informe técnico automático — CHECKPOINT B

**Generar informe con:**

1. Lista de duplicados (pares) con similitud exacta (float con 2 decimales).
    
2. Lista de solapamientos críticos con entidades compartidas (mostrar entidades).
    
3. Estadísticas:
    
    - `%Recordar / %Comprender / %Aplicar` actuales.
        
    - `Balance posiciones respuestas` (A/B/C %).
        
    - `% de opciones fuera de ±10% longitud`.
        
4. Recomendaciones concretas por caso (para cada par duplicado/solapamiento indicar `ELIMINAR` o `REESCRIBIR` con la reescritura sugerida, o `MATIZAR/CONVERTIR` con texto ejemplo).
    

**CHECKPOINT B (formato JSON resumido)**

```json
{
 "phase":"FASE B - Informe técnico",
 "status":"OK" | "FAIL",
 "metrics": {
   "duplicates_list":[[i,j,sim],...],
   "solapamientos_list":[[i,j,overlap,entities],...],
   "bloom_pct":{"Recordar":xx,"Comprender":yy,"Aplicar":zz},
   "answer_balance":{"A":xx,"B":yy,"C":zz},
   "length_violations_pct": x.x
 },
 "actions":"Lista de recomendaciones y propuestas de reescritura"
}
```

---

## FASE C — Tercer pase de refinamiento (APLICAR CAMBIOS AUTORIZADOS) — CHECKPOINT C

**Si el solicitante ha indicado EXACTAMENTE**:  
`"AUTORIZO EJECUCIÓN COMPLETA"` → aplicar automáticamente las correcciones seleccionadas (reescrituras y/o eliminaciones) según las propuestas del Fase B.  
Si no hay autorización, **detener** y solicitar confirmación.

**Operaciones a realizar (si autorizado):**

1. **Reescribir** preguntas marcadas (mantener terminología y citas textuales del documento; cada reescritura debe incluir la `Cita` que sustenta la respuesta correcta). Generar versión `Hard` obligatoria para cada ítem modificado.
    
2. **Eliminar** preguntas si la propuesta fue `ELIMINAR`. Si elimina, añadir pregunta(s) de reemplazo que cubran huecos conceptuales del documento (usar estratificación del prompt principal).
    
3. **Homogeneizar** longitudes de opciones (±10%) conservando significado y fidelidad.
    
4. **Generar variantes** solo si se solicita explícitamente: `Hard` obligatorio; `Med` y `Easy` opcionales y solo si se piden.
    

**Reglas de reescritura (fidelidad):**

- No añadir información que no esté presente textualmente en los documentos.
    
- Las parafrasis deben tener `parafrasis_rate < 0.15` (si se mide), y la `Cita` debe aparecer en la nota.
    

**CHECKPOINT C (formato JSON)**

```json
{
 "phase":"FASE C - Aplicación correcciones",
 "status":"OK" | "FAIL",
 "metrics": {
   "items_rewritten": n,
   "items_deleted": m,
   "items_added": k,
   "lengths_homogenized": p
 },
 "actions":"Resumen de cambios aplicados"
}
```

---

## FASE D — Generar diff de reescrituras — CHECKPOINT D

Por cada pregunta modificada incluir:

- `Nº original → Texto original → Texto reescrito → Motivo`
    
- Mostrar diff conciso (máx 2 líneas por cambio). Usar formato legible tipo:
    

```
Nº 4:
- ORIGINAL: ¿Texto original...?
- REESCRITO HARD: ¿Texto reescrito...?
- MOTIVO: DUPLICADO con ítem 12 (sim=0.82)
```

**CHECKPOINT D (formato JSON)**

```json
{
 "phase":"FASE D - Diffs",
 "status":"OK" | "FAIL",
 "metrics": {
   "diff_count": n,
   "examples_shown": min(10,n)
 },
 "actions":"Diffs generados"
}
```

---

## FASE E — Verificación semántica final — CHECKPOINT E (REQUISITO DE APROBACIÓN)

Recalcular:

1. Similitud semántica entre todas las preguntas → **no debe quedar** par con coseno > 0.70.
    
2. Solapamiento de entidades → **no debe quedar** par con overlap_ratio > 0.50 (salvo casos justificados y documentados).
    
3. Bloom dentro de tolerancias indicadas.
    
4. Opciones uniformes ±10%.
    
5. Balance de respuestas A/B/C = 33% ±5%.
    

**Si se cumplen todas → status:"OK".** Si no, enumerar incumplimientos y, si está autorizado, volver a Fase C para ajustes adicionales (hasta 2 ciclos).

**CHECKPOINT E (formato JSON)**

```json
{
 "phase":"FASE E - Verificación final",
 "status":"OK" | "FAIL",
 "metrics": {
   "max_pair_similarity": 0.69,
   "max_entity_overlap": 0.48,
   "bloom_distribution": {"Recordar":0.65,"Comprender":0.30,"Aplicar":0.05},
   "answer_balance": {"A":0.34,"B":0.33,"C":0.33},
   "length_violations": 0
 },
 "actions":"Resumen final"
}
```

---

## ENTREGABLES (formato final dentro del chat)

Al finalizar correctamente (CHECKPOINT E status OK) — devolver **únicamente**:

1. **Tabla final** con columnas: `Nº | Pregunta | Opción 1 | Opción 2 | Opción 3 | Answer | Notas` (todas las filas finales).
    
    - `Answer` debe coincidir exactamente con el texto de una de las opciones.
        
2. **Informe técnico resumido** (máx 300 palabras) con: duplicados/solapamientos principales, distribución Bloom final, balance respuestas, número de reescrituras/eliminaciones/altas, y si se usaron fallbacks (TF-IDF/NER fallback).
    
3. **Diff de reescrituras** (solo preguntas cambiadas), conciso (máx 2 líneas por cambio).
    

---

## EJECUCIÓN AUTOMÁTICA (AUTORIZACIÓN)

- **Para autorizar**: el solicitante debe enviar exactamente el texto:  
    `"AUTORIZO EJECUCIÓN COMPLETA"`
    
- Si la IA recibe esa cadena **antes de ejecutar Fase C**, puede aplicar automáticamente las correcciones propuestas sin pedir confirmación adicional.
    
- Si NO se recibe, la IA debe detenerse en Fase B y pedir confirmación humana.
    

---

## NOTAS TÉCNICAS Y FALLOBACKS

- Si embeddings no están disponibles, usar TF-IDF pairwise similitudes (código incluido). Umbrales deben mantenerse pero documentar la degradación de fiabilidad en el Informe.
    
- Si spaCy no está disponible para NER, usar heurística de tokens capitalizados y co-ocurrencias; documentar limitación.
    
- Todas las reescrituras **deben** incluir la `Cita` textual que las soporta (para mantener la condición de fidelidad absoluta).
    

---

## EJEMPLO DE RESPUESTA (estructura esperada tras ejecución)

1. CHECKPOINT A → JSON
    
2. CHECKPOINT B → JSON + Recomendaciones
    
3. (si autorizado) CHECKPOINT C → JSON de cambios aplicados
    
4. CHECKPOINT D → JSON + diffs
    
5. CHECKPOINT E → JSON final
    
6. Tabla final + Informe técnico + Diff
    

---

`"AUTORIZO EJECUCIÓN COMPLETA"`