>[!Tip] Ajuste Taxonomía de bloom
> - Para evaluación formativa básica: más % en Recordar
>- Para exámenes de certificación experta: más % en Aplicar/Analizar
> - Para investigación/tesis: incluso Evaluar/Crear


 >[!WARNING] Atención
 >- Permite 2 Documentos 
 >  - Más personalizable para el usuario, basado en modelos anteriores
 >   - Tiene menos dificultad
 >   - Pude dar respuestas menos optimas 
 >   - Puede no resultar ideal para Documentos muy largos 
 >   - Fidelidad al 100%
 >   



````
TAREA
Generar un banco de preguntas tipo test (nivel MIR/PIR) a partir de 1 o 2 documentos suministrados. Idioma: Español (formal). Fidelidad absoluta al/los documento(s): terminantemente prohibido inventar información. Si falta información para una pregunta válida, marcar INCOMPLETO.

PARÁMETROS PRINCIPALES (obligatorio)
- num_options = 3.
- Objetivo base: Cubrir todo el documento (rango 30–70; mínimo absoluto 30; si fuera necesario se puede aumentar el máximo). Ajuste automático según longitud/densidad (algoritmo incluido).
- MaxPorConcepto configurable; criterios aplicados (ver sección MaxPreguntasPorConcepto).
- Distribución Bloom objetivo: Recordar 50% ±5 ; Comprender 30% ±5 ; Aplicar 20% ±5.
- Determinismo: temperature = 0 (o 0.0–0.2). No devolver razonamiento interno (do_not_return_chain_of_thought = true).
- Salida final: **Tabla** lista para copiar y pegar en el chat/Excel.

CAPABILITY CHECK (EJECUTAR AL INICIO)
Emitir inmediatamente un bloque JSON breve (máx 3 líneas) con:
{"embeddings_available":"yes/no","preferred_embedding_model":"<modelo>","code_execution":"yes/no","ocr_available":"yes/no"}
- Si `no` en algún recurso, activar fallbacks y documentarlo en el Informe Técnico.

EMBEDDINGS (recomendación para español)
- Preferir modelos multilingües optimizados para español: `"paraphrase-multilingual-MiniLM-L12-v2"` o `"sentence-transformers/distiluse-base-multilingual-cased-v2"`.  
- Si el entorno exige otro modelo, usar parámetro `embedding_model` y documentarlo.

FALLBACKS OBLIGATORIOS
- No embeddings → usar TF-IDF completo (implementación incluida).
- No OCR → marcar elementos `NO_EXTRAÍBLES` y no generar preguntas dependientes.
- No ejecución de código → aplicar heurísticas (frecuencia, co-ocurrencia) y documentar limitaciones.

PRIORIDAD Y MULTI-DOCUMENTO
- Si hay 2 documentos: indicar `prioridad_documentos = [doc1, doc2]`. Si no, se prioriza doc1.
- En contradicciones: registrar ambas citas y elegir según prioridad_documentos; documentar contradicción en Informe.

FLUJOS OBLIGATORIOS (CHECKPOINTS — NO AVANZAR SI FALLA)
> Cada FASE termina con un CHECKPOINT: la IA **debe** imprimir exactamente el bloque JSON del checkpoint (formato abajo). No continuar hasta que `status:"OK"`. Si `status:"FAIL"`, aplicar correcciones automáticas (hasta 2 reintentos). Si persiste FAIL → marcar `INCOMPLETO`, emitir Informe y detener.

**FORMATO DE CHECKPOINT (obligatorio)**  
Cada checkpoint debe imprimirse exactamente así:
{
 "phase":"FASE X - Nombre",
 "status":"OK" | "FAIL",
 "metrics": { ... métricas obligatorias por fase ... },
 "actions":"texto breve con correcciones aplicadas (si aplica)"
}

### Métricas obligatorias por checkpoint
- **CHECKPOINT 1 (FASE 1 — Triple lectura)**: `concepts_extracted` (int), `citations_count` (int), `20pct_count` (int), `doc_count` (int), `errors` (list).
- **CHECKPOINT 2 (FASE 2 — Estratificación / Calibración)**: `objective_calculated` (int), `concept_counts` (dict por estrato), `densidad_conceptual` (float), `longitud_palabras` (int).
- **CHECKPOINT 3 (FASE 3 — Generación primera pasada)**: `generated_count` (int), `per_concept_counts` (dict), `options_uniformity_violations` (int), `issues_sample` (list).
- **CHECKPOINT 4 (FASE 4 — Control de calidad)**: `duplicates_found` (int), `items_rewritten` (int), `avg_confidence` (float), `items_incomplete` (list).

FASE 1 — TRIPLE LECTURA (Estructural, Analítica, Crítica)
- Extraer: definiciones, datos numéricos, metodología (objetivo/muestra/técnicas/conclusiones), autores/teorías, limitaciones; guardar citas EXACTAS con `doc_id` y `ref_paragraph`.
- Identificar `20pct_essential` (lista de conceptos).
- Emitir CHECKPOINT 1 con métricas obligatorias.

**Código recomendado (embeddings configurable)**

```python
# Embeddings (usar si available). Modelos sugeridos para español:
from sentence_transformers import SentenceTransformer
from sklearn.metrics.pairwise import cosine_similarity
def load_embedding_model(name="paraphrase-multilingual-MiniLM-L12-v2"):
    return SentenceTransformer(name)

def get_embeddings(model, texts):
    return model.encode(texts, convert_to_numpy=True)
````

**Fallback TF-IDF (implementación completa)**

```python
# TF-IDF fallback completo
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np

tfidf_vect = None
tfidf_matrix_cache = None

def fit_tfidf(corpus):
    global tfidf_vect, tfidf_matrix_cache
    tfidf_vect = TfidfVectorizer(ngram_range=(1,2), stop_words='spanish', max_features=50000)
    tfidf_matrix_cache = tfidf_vect.fit_transform(corpus)
    return tfidf_matrix_cache

def tfidf_similarity_between(corpus, query):
    # corpus: list[str] (pre-fitted or to fit now)
    # query: str
    global tfidf_vect, tfidf_matrix_cache
    if tfidf_vect is None or tfidf_matrix_cache is None:
        tfidf_matrix_cache = fit_tfidf(corpus)
    q_vec = tfidf_vect.transform([query])
    sims = cosine_similarity(q_vec, tfidf_matrix_cache)[0]
    return sims  # array of similarities length len(corpus)

def tfidf_pairwise_similarity(list_texts):
    # devuelve matriz de similitudes pairwise
    global tfidf_vect
    vect = TfidfVectorizer(ngram_range=(1,2), stop_words='spanish', max_features=50000)
    X = vect.fit_transform(list_texts)
    sim = cosine_similarity(X)
    return sim
```

FASE 2 — ESTRATIFICACIÓN Y OBJETIVO ADAPTATIVO

- Estratos: `20%_esencial`, `ideas_troncales`, `subideas_criticas`, `autores_relevantes`, `menos_relevante`, `prescindible`.
    
- **Algoritmo objetivo adaptativo (ejemplo reproducible)**:
    

```python
def calcular_objetivo(longitud_palabras, densidad):
    # longitud_palabras: int
    # densidad: float en [0,1]
    if longitud_palabras < 2000:
        base = 30
    elif longitud_palabras < 5000:
        base = 50
    elif longitud_palabras < 10000:
        base = 70
    else:
        base = min(100, int(densidad * 1.5 * 100))
    # ajustar por densidad (±20%)
    if densidad > 0.7:
        base = int(base * 1.2)
    elif densidad < 0.3:
        base = int(base * 0.8)
    objetivo = max(30, min(100, base))
    return objetivo
```

- Emitir CHECKPOINT 2 con métricas obligatorias.
    

### MaxPreguntasPorConcepto — Criterio explícito

- Priorizar así:
    
    - `20%_esencial`: 3–4 preguntas por concepto.
        
    - `ideas_troncales`: 2–3 preguntas por concepto.
        
    - `subideas_criticas`: 1–2 preguntas por concepto.
        
    - `autores_relevantes`: 1–2 preguntas si aportan diferencias conceptuales.
        
    - `contenido_menor`: 0–1 (solo si aún faltan preguntas para objetivo).
        
- Implementar una función que asigna prioridad y decrementa si objetivo global ya alcanzado.
    

FASE 3 — GENERACIÓN DE PREGUNTAS (reglas estrictas)

- Para cada concepto generar hasta MaxPorConcepto según criterio anterior.
    
- Enunciado: breve, claro, auto-contenido; evitar dobles negaciones y absolutos.
    
- Opciones: exactamente 3, comenzar con `A:` , `B:` , `C:` .
    
- **Respuesta**: campo `Respuesta` debe ser **idéntico** (byte-a-byte) al texto de la opción correcta (p. ej. `A: Texto...`).
    
- Cita textual de soporte ≥3 palabras e indicar `doc_id` y `ref_paragraph`.
    
- Longitud de opciones: ±10% respecto a la media de las 3 opciones.
    
- Validar distractores y uniformidad.
    
- Emitir CHECKPOINT 3 con métricas obligatorias.
    

**Plausibilidad de distractores (rango por defecto y configurable)**

- **Por defecto (según tu sugerencia para preguntas difíciles):** `plausibility_min = 0.48`, `plausibility_max = 0.52`.  
    **Nota:** este rango es muy estrecho (alto nivel de dificultad). Puede ampliarse para tests menos exigentes, p. ej. 0.45–0.60. El sistema debe permitir parámetro `plausibility_range`.
    

```python
def plausibility_check(distractor, respuesta, emb_model=None, tfidf_corpus=None, min_sim=0.48, max_sim=0.52):
    if emb_model is not None:
        d_emb = get_embeddings(emb_model, [distractor])[0]
        r_emb = get_embeddings(emb_model, [respuesta])[0]
        sim = cosine_similarity([d_emb], [r_emb])[0,0]
    else:
        sims = tfidf_similarity_between(tfidf_corpus, distractor)
        # aproximar comparativa con respuesta: usar media o max de similitud con oraciones que contienen la respuesta
        sim = sims.mean() if len(sims)>0 else 0.0
    ok = (min_sim <= sim <= max_sim)
    return sim, ok
```

FASE 4 — CONTROL DE CALIDAD Y REESCRITURA

- Checks obligatorios:
    
    - Duplicados: sim >0.70 = marcar; sim >0.85 = eliminar/reescribir.
        
    - Distractors plausibles: dentro de `plausibility_range`.
        
    - Cita presente y ≥3 palabras.
        
    - Confidence mínima por ítem: 0.65 (fórmula abajo).
        
- Reescritura automática hasta 2 intentos por ítem; si persiste fallo → marcar `INCOMPLETO`.
    
- Emitir CHECKPOINT 4 con métricas.
    

**Cálculo de confidence (num_options=3)**

```python
def calcular_confidence(p_registro):
    score = 0.0
    # Cita clara (30%)
    if len(p_registro['cita'].split()) >= 3 and p_registro.get('cita_clara', True):
        score += 0.30
    # Distractores válidos (40%)
    valid = sum(1 for d in p_registro['distractores'] if d.get('is_valid', False))
    score += (valid / 2) * 0.40
    # Fidelidad/parafrasis_rate (30%)
    if p_registro.get('parafrasis_rate', 1.0) < 0.15:
        score += 0.30
    return round(score,2)
```

SALIDA REQUERIDA (ESTRUCTA — **TABLA MARKDOWN**)

- **Formato**: tabla Markdown con cabecera EXACTA (columnas y orden obligatorio). No CSV.
    
- Cabecera (usar exactamente estos nombres):  
    | Nº | Pregunta | Opción 1 | Opción 2 | Opción 3 | Answer | Nota | Bloom | Confianza |
    
- Estructura obligatoria para las Notas
	- Citación exacta del fragmento relevante
	- Diferenciación crítica y BREVE de conceptos similares/relacionados
	- No es necesario que pongas el número de la página
	- Análisis de Distractores : Por qué son incorrectos o plausibles pero incorrectos
	- Formato de salida obligatorio: Notas: ‘Cita’; No son las otras opciones porque (explicación). Solo válido poner texto y ‘:’ prohibido cualquier otro carácter
    
- `Answer` DEBE SER TEXTUALMENTE IGUAL a la opción correcta (byte-a-byte).
    
- `meta`: JSON compacto con al menos `{"parafrasis_rate":0.XX,"duplicate_sim":0.XX}`.
    

INFORME TÉCNICO COMPACTO (máx 300 palabras)

- Debe incluir: `capability_check` (embeddings/modelo), longitud documento (palabras), densidad conceptual, objetivo final (# preguntas), generadas, % cobertura 20% esencial, duplicados eliminados, confidence promedio, distribución Bloom final, ítems marcados INCOMPLETO (si los hay) y razones, fallbacks usados. Incluir si se usó el fallback TF-IDF o ajuste de plausibility_range.
    

ENTREGA Y PROTOCOLO FINAL

- Emitir CHECKPOINTS 1→4 en orden. No continuar sin `status:"OK"`. En cada FAIL aplicar correcciones automáticas (hasta 2 reintentos). Si sigue FAIL → marcar `INCOMPLETO`, emitir Informe y detener.
    
- Al terminar OK: devolver **solo** 1) la **Tabla** y 2) el **Informe Técnico Compacto**.
    
- No devolver chain-of-thought ni razonamiento interno.
    

