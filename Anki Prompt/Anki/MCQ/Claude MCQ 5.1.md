>[!DANGER] En proceso de destilación 
Basado en el [[Datos/Anki/MCQ/MCQ 5.1]] pero resulta sumamente largo, hay que introducirlo a un proceso de destilación en Claude Opus. Se le podría pedir que hiciera 2 partes y que una fuera basada en la [[Datos/Anki/DLC's/Verificación pro (5 y 5.1)]]


%%Promt
El promt está pensado para inteligencias artificiales de razonamiento y uso de código avanzado como Chat gpt 5 con pensamiento extendido, gemini pro y Claude Opus 4.1 con pensamiento extendido. he probado el prompt en chat gpt y me ha hecho cálculos y me ha conseguido resultados muy buenos. Quiero que hagas sugerencias basadas en que las ias a las que les voy a poner el promt son de esa potencia
Crear una **versión condensada (70% del tamaño actual)** eliminando:

- Ejemplos de código largos → referencias breves
- Explicaciones redundantes
- Secciones menos críticas (JSON opcional, análisis contrafactual)
  
%%
# SISTEMA DE GENERACIÓN DE PREGUNTAS CON ANÁLISIS PROFUNDO

## Para modelos con capacidades de razonamiento extendido y ejecución de código

---

## CONFIGURACIÓN INICIAL

### Parámetros Obligatorios

- **Idioma**: Español (registro formal)
- **Principio fundamental**: Fidelidad absoluta al texto fuente (no introducir información externa ni inventada)
- **MaxPorConcepto**: 4 preguntas máximo por concepto
- **Objetivo base**: Adaptativo (calculado automáticamente según documento)
- **Mínimo absoluto**: 30 preguntas
- **Entrada**: Documento principal (archivo o texto)
- **Salida obligatoria**: Tabla + Informe técnico + JSON de metadatos

### Herramientas Autorizadas (OBLIGATORIO USAR)

Este prompt requiere ejecución de código para cálculos críticos:

```python
# Librerías requeridas:
# - sentence-transformers o sklearn: similitud semántica
# - numpy/pandas: análisis estadístico
# - spacy o regex: extracción de entidades
# - hashlib: versionado
# - difflib: generación de diffs
```

**Formato de código**: Antes de cada cálculo crítico, mostrar:

```python
# [Descripción del cálculo]
[código ejecutable]
```

→ Resultado: [output]

### Modo de Operación

**INSTRUCCIÓN ACTUAL**: AUTORIZO EJECUCIÓN COMPLETA + OMITIR_RESUMEN

Interpretación:

- Aplicar correcciones automáticamente sin pedir confirmación
- Saltar resumen previo (ir directo a generación)
- Si en futuro el usuario dice "QUIERO REVISIÓN MANUAL" → entregar propuestas sin aplicar

### Instrucciones para Modelo de Razonamiento

- Mostrar razonamiento intermedio en checkpoints
- Ejecutar código para cálculos cuantitativos
- Verificar cada decisión contra criterios explícitos
- Si detectas ambigüedad → PARAR y solicitar aclaración
- Priorizar CALIDAD sobre velocidad
- Tiempo estimado: 3-8 minutos para documentos de 3000-8000 palabras

---

## FASE 0: CALIBRACIÓN INTELIGENTE

### 0.1 Análisis Preliminar del Documento

Ejecutar código para análisis estructural:

```python
# Calcular métricas base
longitud_palabras = contar_palabras(documento)
num_secciones = contar_secciones_principales(documento)
num_parrafos = contar_parrafos(documento)

# Análisis de densidad conceptual por párrafo
for párrafo in documento:
    densidad[párrafo] = (
        0.4 * conceptos_únicos(párrafo) / longitud(párrafo) +
        0.3 * presencia_datos_numéricos(párrafo) +
        0.2 * presencia_definiciones(párrafo) +
        0.1 * presencia_relaciones_causales(párrafo)
    )

densidad_promedio = mean(densidad.values())
parrafos_prioritarios = top_percentile(densidad, 0.80)
```

**Salida esperada**:

- Longitud: [X] palabras
- Secciones principales: [Y]
- Densidad conceptual promedio: [Z] (escala 0-1)
- Párrafos de alta densidad (top 20%): [lista con índices]

### 0.2 Cálculo de Objetivo Adaptativo

```python
# Fórmula de objetivo adaptativo
if longitud_palabras < 2000:
    objetivo_base = 30
elif longitud_palabras < 5000:
    objetivo_base = 50
elif longitud_palabras < 10000:
    objetivo_base = 70
else:
    objetivo_base = min(100, num_conceptos_estimado * 1.5)

# Ajuste por densidad
if densidad_promedio > 0.7:  # Documento muy denso
    objetivo_ajustado = objetivo_base * 1.2
elif densidad_promedio < 0.3:  # Documento disperso
    objetivo_ajustado = objetivo_base * 0.8
else:
    objetivo_ajustado = objetivo_base

# Restricción final
objetivo_final = clamp(round(objetivo_ajustado), 30, 100)
```

**Reportar**:

> "Análisis completado: documento de [X] palabras, densidad conceptual [Z].  
> Objetivo adaptado: **[objetivo_final] preguntas** (tolerancia ±10%)"

---

## FASE 1: EXTRACCIÓN PROFUNDA

### 1.1 Primera Lectura: Estructura

- Identificar títulos, secciones, subsecciones y jerarquía
- **Salida**: Índice sintético con estructura jerárquica

### 1.2 Segunda Lectura: Extracción Analítica

Extraer y registrar con **citas textuales exactas**:

1. Conceptos fundacionales (definiciones, principios)
2. Datos cuantitativos y estadísticas
3. Entidades nombradas (personas, lugares, organizaciones)
4. Afirmaciones contundentes o conclusiones principales
5. Relaciones causa → efecto explícitas
6. Elementos experimentales (si aplica):
    - Objetivo del estudio
    - Muestra/metodología
    - Técnicas empleadas
    - Conclusiones principales

**Formato de registro**:

```
Concepto: [nombre_breve]
Cita textual: "[texto exacto del documento]"
Ubicación: [sección X, párrafo Y]
Densidad local: [valor del párrafo]
```

### 1.3 Tercera Lectura: Análisis Crítico

Identificar:

- Matices y precisiones importantes
- Limitaciones metodológicas mencionadas
- Contradicciones o tensiones internas
- Huecos informativos

### 1.4 Síntesis de Extracción

**Matriz de conceptos**:

|Concepto|Cita Textual|Sección|Densidad|Prioridad|
|---|---|---|---|---|
|...|...|...|...|...|

**Identificación del "20% esencial"**: Seleccionar 3-6 conceptos que explican ~80% del contenido, basándose en:

- Frecuencia de aparición
- Conexiones con otros conceptos
- Presencia en párrafos de alta densidad
- Relevancia en estructura del documento

**Salida**:

```
CONCEPTOS EXTRAÍDOS: [N] conceptos únicos
20% ESENCIAL: [lista de 3-6 conceptos con justificación breve]
PÁRRAFOS PRIORITARIOS: [top 20% por densidad]
```

---

## CHECKPOINT FASE 1

**Verificar antes de continuar**:

- [ ] ¿He identificado TODOS los conceptos principales mencionados en el texto?
- [ ] ¿Cada concepto tiene al menos una cita textual asociada?
- [ ] ¿Las citas son 100% fieles (no parafraseadas)?
- [ ] ¿He analizado la densidad conceptual del documento?

**Si algún ítem falla** → Re-ejecutar subfase correspondiente.

**Estado actual**:

- Conceptos extraídos: [X]
- Citas registradas: [Y]
- Densidad promedio: [Z]

---

## FASE 2: GENERACIÓN ITERATIVA (3 PASES)

### Preparación

```python
ListaConceptos = conceptos_extraidos_fase1
N_conceptos = len(ListaConceptos)

if N_conceptos == 0:
    MARCAR("INCOMPLETO - No se extrajeron conceptos")
    TERMINAR()
```

### PASE 1: Cobertura Mínima (1 pregunta por concepto)

**Objetivo**: Asegurar que cada concepto tenga al menos una pregunta.

Para cada concepto C en ListaConceptos:

1. Generar 1 pregunta Hard (nivel Bloom: Recordar preferente)
2. Basar en la cita textual asociada a C
3. Generar 3 distractores usando estrategias algorítmicas (ver sección 2.4)
4. Calcular confidence_score (ver sección 2.5)
5. Registrar metadatos:
    
    ```
    concepto: Cpase: 1nivel_bloom: Recordarconfidence: [score]
    ```
    

**Salida esperada**: [N_conceptos] preguntas generadas

### PASE 2: Expansión Enfocada

**Objetivo**: Alcanzar el objetivo_final añadiendo preguntas a conceptos prioritarios.

```python
Q_generadas_P1 = len(preguntas_pase1)
Q_restantes = objetivo_final - Q_generadas_P1
```

**Orden de prioridad para expansión**:

1. Conceptos del 20% esencial
2. Conceptos de párrafos de alta densidad (>0.6)
3. Conceptos con mayor número de citas/evidencia
4. Conceptos asociados a experimentos o datos numéricos

**Proceso**:

```python
for concepto in orden_prioridad:
    if preguntas_del_concepto < MaxPorConcepto and Q_restantes > 0:
        # Generar pregunta de distinto nivel Bloom
        if len(preguntas_Recordar[concepto]) >= 2:
            nivel = elegir_entre(["Comprender", "Aplicar"])
        else:
            nivel = elegir_entre(["Recordar", "Comprender"])
        
        generar_pregunta(concepto, nivel)
        Q_restantes -= 1
```

**Diversificación Bloom**:

- Pase 1 prioriza: Recordar (~70%)
- Pase 2 añade: Comprender (~25%) y Aplicar (~5%)

**Si Q_restantes > 0 tras agotar candidatos**: Generar preguntas adicionales sobre:

- Comparaciones entre conceptos relacionados
- Matices o implicaciones mencionadas
- Interpretaciones de resultados citados
- Limitaciones metodológicas explícitas

**RESTRICCIÓN**: Cada nueva pregunta requiere cita textual; respetar MaxPorConcepto.

### PASE 3: Afinado y Control de Calidad

**Objetivo**: Eliminar duplicados, balancear distribución Bloom, verificar calidad.

#### 3.1 Detección de Duplicados (ejecutar con código)

```python
from sentence_transformers import SentenceTransformer
from sklearn.metrics.pairwise import cosine_similarity

model = SentenceTransformer('paraphrase-multilingual-MiniLM-L12-v2')

# Generar embeddings de todas las preguntas
preguntas_texto = [p['pregunta'] for p in todas_preguntas]
embeddings = model.encode(preguntas_texto)

# Calcular similitud por pares
similitud_matriz = cosine_similarity(embeddings)

# Detectar duplicados
duplicados = []
for i in range(len(similitud_matriz)):
    for j in range(i+1, len(similitud_matriz)):
        if similitud_matriz[i][j] > 0.70:
            duplicados.append({
                'par': (i, j),
                'similitud': similitud_matriz[i][j],
                'pregunta_i': preguntas_texto[i],
                'pregunta_j': preguntas_texto[j]
            })
```

**Acción sobre duplicados**:

- Si similitud > 0.85: ELIMINAR la de menor confidence_score
- Si similitud 0.70-0.85: REESCRIBIR la de menor confidence enfocando distinto aspecto
- Registrar en log de cambios

#### 3.2 Detección de Solapamiento de Entidades

```python
import spacy
nlp = spacy.load('es_core_news_sm')

for par in pares_similitud_alta:
    entidades_i = extraer_entidades(pregunta_i)
    entidades_j = extraer_entidades(pregunta_j)
    
    interseccion = entidades_i & entidades_j
    menor_conjunto = min(len(entidades_i), len(entidades_j))
    
    if menor_conjunto > 0:
        solapamiento = len(interseccion) / menor_conjunto
        
        if solapamiento > 0.50:
            marcar_SOLAPAMIENTO_CRÍTICO(par)
            proponer_matización(par)  # Ej: definición vs ejemplo
```

#### 3.3 Balance de Distribución Bloom

```python
distribucion_actual = contar_por_bloom(todas_preguntas)
distribucion_objetivo = {
    'Recordar': 0.65,
    'Comprender': 0.30,
    'Aplicar': 0.05
}

for nivel, proporcion_objetivo in distribucion_objetivo.items():
    proporcion_actual = distribucion_actual[nivel] / total_preguntas
    diferencia = abs(proporcion_actual - proporcion_objetivo)
    
    if diferencia > 0.05:  # Tolerancia ±5%
        ajustar_preguntas(nivel, diferencia)
```

**Estrategia de ajuste**:

- Si hay exceso de Recordar: reescribir algunas como Comprender
- Si hay déficit de Aplicar: buscar conceptos con ejemplos prácticos

#### 3.4 Homogeneización de Longitud de Opciones

```python
for pregunta in todas_preguntas:
    longitudes = [len(opcion) for opcion in pregunta['opciones']]
    longitud_media = mean(longitudes)
    
    for opcion, long in zip(pregunta['opciones'], longitudes):
        desviacion = abs(long - longitud_media) / longitud_media
        
        if desviacion > 0.10:  # Tolerancia ±10%
            marcar_REVISAR_LONGITUD(pregunta)
            ajustar_opcion(opcion)  # Expandir o contraer manteniendo fidelidad
```

### 2.4 Estrategias de Generación de Distractores

**Implementación algorítmica obligatoria**:

```python
def generar_distractores(respuesta_correcta, concepto, documento):
    candidatos = []
    
    # Estrategia 1: Negación/Inversión
    afirmaciones_contrarias = buscar_contradicciones(respuesta_correcta, documento)
    for af in afirmaciones_contrarias:
        if af != respuesta_correcta:
            candidatos.append({
                'texto': af,
                'estrategia': 'negación',
                'ubicacion': ubicacion_en_doc(af)
            })
    
    # Estrategia 2: Generalización/Especificación
    if es_especifico(respuesta_correcta):
        gen = buscar_generalizacion(respuesta_correcta, documento)
        if gen: candidatos.append({'texto': gen, 'estrategia': 'generalización'})
    else:
        esp = buscar_especificacion(respuesta_correcta, documento)
        if esp: candidatos.append({'texto': esp, 'estrategia': 'especificación'})
    
    # Estrategia 3: Confusión de entidades similares
    entidades_similares = buscar_entidades_mismo_tipo(respuesta_correcta, documento)
    for ent in entidades_similares[:3]:
        candidatos.append({'texto': ent, 'estrategia': 'confusión_entidades'})
    
    # Calcular similitud y filtrar
    candidatos_validos = []
    for c in candidatos:
        sim = similitud_semantica(c['texto'], respuesta_correcta)
        if 0.45 <= sim <= 0.70:  # Sweet spot de plausibilidad
            c['similitud'] = sim
            candidatos_validos.append(c)
    
    # Seleccionar 3 con máxima diversidad de estrategia
    return seleccionar_diversos(candidatos_validos, n=3)
```

**Requisito**: Todos los distractores deben aparecer textual o conceptualmente en el documento.

### 2.5 Sistema de Confianza por Pregunta

```python
def calcular_confidence_score(pregunta):
    score = 0.0
    
    # Factor 1: Claridad de la cita (0-0.3)
    if len(pregunta['cita']) >= 15 and es_inequivoca(pregunta['cita']):
        score += 0.3
    elif len(pregunta['cita']) >= 8:
        score += 0.15
    
    # Factor 2: Frecuencia del concepto (0-0.2)
    frecuencia = contar_apariciones(pregunta['concepto'], documento)
    if frecuencia >= 3:
        score += 0.2
    elif frecuencia == 2:
        score += 0.1
    
    # Factor 3: Calidad de distractores (0-0.3)
    distractores_validos = sum(1 for d in pregunta['distractores'] 
                                if aparece_en_documento(d['texto']))
    score += (distractores_validos / 3) * 0.3
    
    # Factor 4: Fidelidad textual (0-0.2)
    if pregunta['paráfrasis_porcentaje'] < 0.15:
        score += 0.2
    elif pregunta['paráfrasis_porcentaje'] < 0.25:
        score += 0.1
    
    return round(score, 2)

# Umbral de inclusión
UMBRAL_MINIMO = 0.65
preguntas_finales = [p for p in todas_preguntas if p['confidence'] >= UMBRAL_MINIMO]
```

### Salida del Pase 3

**Matriz de Cobertura**:

```
Concepto | Preguntas Pase1 | Preguntas Pase2 | Total | Bloom Mix
---------|-----------------|-----------------|-------|----------
C1       | 1               | 2               | 3     | R,C,C
C2       | 1               | 0               | 1     | R
...
```

**Conceptos NO cubiertos** (si existen):

```
- Concepto X: Cita ambigua, requiere interpretación
- Concepto Y: FIGURA_NO_EXTRAÍBLE sin leyenda
```

**Estadísticas**:

- Total generadas: [N]
- Eliminadas por duplicación: [M]
- Reescritas: [K]
- Confidence promedio: [X]

---

## CHECKPOINT FASE 2

**Verificar**:

- [ ] ¿He generado al menos 1 pregunta por concepto (salvo justificados)?
- [ ] ¿Estoy dentro del rango objetivo_final ± 10%?
- [ ] ¿No hay pares con similitud > 0.70 sin justificar?
- [ ] ¿Distribución Bloom dentro de tolerancias?
- [ ] ¿Confidence promedio ≥ 0.70?

**Estado**:

- Preguntas actuales: [X]
- Objetivo: [Y]
- Conceptos sin cubrir: [Z]

---

## FASE 3: VERIFICACIÓN CRUZADA PROFUNDA

### 3.1 Verificación de Coherencia Lógica

Para cada pregunta:

```python
def verificar_coherencia(pregunta):
    # Test 1: ¿La cita respalda realmente la respuesta?
    if not cita_respalda_respuesta(pregunta['cita'], pregunta['answer']):
        marcar('REQUIERE_REVISIÓN_HUMANA', 
               'Cita no respalda claramente la respuesta')
    
    # Test 2: ¿Algún distractor podría ser correcto bajo otra interpretación?
    for distractor in pregunta['distractores']:
        if podria_ser_correcto(distractor, pregunta['cita']):
            marcar('AMBIGUO', 
                   f'Distractor "{distractor}" ambiguo según contexto')
    
    # Test 3: Análisis contrafactual
    doc_sin_cita = eliminar_fragmento(documento, pregunta['cita'])
    if pregunta_tiene_sentido_sin_cita(pregunta, doc_sin_cita):
        marcar('ADVERTENCIA', 
               'Pregunta podría ser genérica, revisar fidelidad')
```

### 3.2 Test de Robustez de Distractores

```python
for pregunta in preguntas_finales:
    for distractor in pregunta['distractores']:
        # Verificar presencia en documento
        ubicacion = buscar_en_documento(distractor['texto'])
        
        if not ubicacion:
            marcar('DISTRACTOR_DÉBIL', 
                   f'Distractor no aparece en documento: {distractor}')
        
        # Analizar plausibilidad
        contexto = extraer_contexto(ubicacion)
        plausibilidad = evaluar_plausibilidad(distractor, contexto)
        
        if plausibilidad < 0.3:
            marcar('DISTRACTOR_IMPLAUSIBLE', 
                   f'Contexto no justifica confusión: {distractor}')
```

### 3.3 Validación de Nivel Bloom

```python
def validar_bloom(pregunta):
    # Leer pregunta sin opciones
    proceso_cognitivo = identificar_proceso(pregunta['texto'])
    
    bloom_esperado = {
        'recordar': ['qué es', 'definir', 'enumerar', 'identificar'],
        'comprender': ['explicar', 'interpretar', 'por qué', 'describir'],
        'aplicar': ['cómo usaría', 'aplicar', 'resolver', 'demostrar']
    }
    
    nivel_detectado = None
    for nivel, indicadores in bloom_esperado.items():
        if any(ind in pregunta['texto'].lower() for ind in indicadores):
            nivel_detectado = nivel
            break
    
    if nivel_detectado != pregunta['bloom_asignado']:
        if diferencia_niveles(nivel_detectado, pregunta['bloom_asignado']) > 1:
            marcar('RECLASIFICAR_BLOOM', 
                   f'Detectado {nivel_detectado}, asignado {pregunta["bloom_asignado"]}')
```

### 3.4 Análisis de Cobertura Semántica Global

```python
from sklearn.cluster import KMeans
from sklearn.decomposition import PCA

# Paso 1: Embeddings del documento completo por secciones
secciones_embeddings = [embed(seccion) for seccion in secciones_documento]

# Paso 2: Clustering temático
n_clusters = min(10, len(secciones_embeddings) // 2)
kmeans = KMeans(n_clusters=n_clusters)
clusters_doc = kmeans.fit(secciones_embeddings)

# Paso 3: Asignar cada pregunta a un cluster
preguntas_embeddings = [embed(p['pregunta']) for p in preguntas_finales]
clusters_preguntas = [kmeans.predict([emb])[0] for emb in preguntas_embeddings]

# Paso 4: Verificar distribución
distribucion_clusters = Counter(clusters_preguntas)

# Criterio de éxito
clusters_vacios = [c for c in range(n_clusters) if distribucion_clusters[c] == 0]
desviacion_std = std(list(distribucion_clusters.values()))

if len(clusters_vacios) > 0:
    marcar('COBERTURA_INCOMPLETA', 
           f'Clusters sin preguntas: {clusters_vacios}')
    # Generar 1-2 preguntas para clusters vacíos

if desviacion_std / mean(distribucion_clusters.values()) > 0.40:
    marcar('DISTRIBUCIÓN_DESBALANCEADA',
           'Algunas áreas sobre-representadas')
```

**Salida de cobertura**:

```
ANÁLISIS SEMÁNTICO:
Cluster 0 (Introducción): 5 preguntas
Cluster 1 (Metodología): 3 preguntas
Cluster 2 (Resultados): 8 preguntas
...
Clusters desatendidos: [si existen]
```

---

## CHECKPOINT FASE 3

**Verificación final**:

- [ ] ¿Todas las preguntas pasaron verificación de coherencia?
- [ ] ¿Todos los distractores son robustos y plausibles?
- [ ] ¿Niveles Bloom correctamente asignados?
- [ ] ¿Cobertura semántica uniforme (desv. std < 40%)?

**Preguntas marcadas para revisión humana**: [lista]

---

## FASE 4: REFINAMIENTO Y APLICACIÓN DE CAMBIOS

### 4.1 Versionado

```python
import hashlib

# Generar hash de versión actual
version_pre_cambios = hashlib.sha256(
    str(preguntas_finales).encode()
).hexdigest()[:8]

print(f"Versión pre-refinamiento: {version_pre_cambios}")
```

### 4.2 Aplicación de Correcciones

**MODO ACTUAL**: AUTORIZO EJECUCIÓN COMPLETA → aplicar automáticamente

```python
cambios_aplicados = []

for pregunta in preguntas_marcadas:
    if pregunta['marca'] == 'DUPLICADO':
        eliminar_pregunta(pregunta['id'])
        cambios_aplicados.append({
            'accion': 'ELIMINAR',
            'id': pregunta['id'],
            'motivo': 'Duplicado de pregunta X (sim=0.88)',
            'prioridad': 'ALTA'
        })
    
    elif pregunta['marca'] == 'REESCRIBIR':
        nueva_version = reescribir_pregunta(pregunta, enfoque_distinto)
        reemplazar_pregunta(pregunta['id'], nueva_version)
        cambios_aplicados.append({
            'accion': 'REESCRIBIR',
            'id': pregunta['id'],
            'original': pregunta['texto'],
            'nuevo': nueva_version['texto'],
            'motivo': pregunta['razon_marca'],
            'prioridad': 'ALTA'
        })
    
    elif pregunta['marca'] == 'AJUSTAR_OPCIONES':
        opciones_homogeneizadas = homogeneizar_longitudes(pregunta['opciones'])
        actualizar_opciones(pregunta['id'], opciones_homogeneizadas)
        cambios_aplicados.append({
            'accion': 'AJUSTAR_OPCIONES',
            'id': pregunta['id'],
            'motivo': 'Longitudes fuera de tolerancia ±10%',
            'prioridad': 'MEDIA'
        })
```

### 4.3 Recálculo de Métricas

```python
# Re-calcular similitud tras cambios
nueva_matriz_similitud = calcular_similitud(preguntas_finales_v2)
pares_problematicos = detectar_pares(nueva_matriz_similitud, umbral=0.70)

# Re-calcular distribución Bloom
nueva_distribucion_bloom = contar_por_bloom(preguntas_finales_v2)

# Re-calcular confidence promedio
confidence_promedio_v2 = mean([p['confidence'] for p in preguntas_finales_v2])
```

**Condiciones de éxito**:

- ✅ No pares con similitud > 0.70 (salvo justificados)
- ✅ Distribución Bloom dentro de tolerancias (±5%)
- ✅ Confidence promedio ≥ 0.70
- ✅ Longitudes opciones dentro de ±10%

---

## FASE 5: ENTREGA FINAL

### 5.1 Tabla Principal (formato en chat)

```
| Nº | Pregunta (Hard) | Opción 1 | Opción 2 | Opción 3 | Answer | Bloom | Conf | Notas |
|----|-----------------|----------|----------|----------|--------|-------|------|-------|
| 1  | ...             | ...      | ...      | ...      | 2      | R     | 0.87 | ...   |
| 2  | ...             | ...      | ...      | ...      | 1      | C     | 0.92 | ...   |
```

**Formato de Notas** (texto plano, dos líneas):

```
Cita: [texto exacto del documento]
Justificación: [conexión cita→respuesta en ≤18 palabras] | Concepto: [X] | Sección: [Y] | Distractores: [D1:estrategia:0.52] [D2:estrategia:0.61] [D3:estrategia:0.48]
```

### 5.2 Informe Técnico Resumido

```markdown
## INFORME TÉCNICO DE GENERACIÓN

### Estadísticas Generales
- Documento analizado: [X] palabras, [Y] secciones, densidad conceptual [Z]
- Objetivo calculado: [N] preguntas
- Preguntas generadas: [M] preguntas (cumplimiento: X%)
- Conceptos totales: [A]
- Conceptos cubiertos: [B] ([B/A]%)

### Distribución por Taxonomía de Bloom
- Recordar: [X]% (objetivo: 65% ±5)
- Comprender: [Y]% (objetivo: 30% ±5)
- Aplicar: [Z]% (objetivo: 5% ±2)
- **Estado**: [DENTRO/FUERA de tolerancias]

### Control de Calidad
- Duplicados detectados: [N]
- Duplicados eliminados: [M]
- Solapamientos críticos: [K]
- Preguntas reescritas: [L]
- Confidence promedio: [0.XX]

### Matriz de Cobertura
| Concepto | Preguntas | Niveles Bloom | Confidence Media |
|----------|-----------|---------------|------------------|
| C1       | 3         | R,C,C         | 0.85             |
| C2       | 2         | R,A           | 0.79             |
...

### Conceptos NO Cubiertos (si existen)
- [Concepto X]: Razón [explicación]
- [Concepto Y]: Razón [explicación]

### Correcciones Aplicadas
- Total cambios: [N]
- Prioridad ALTA: [M]
- Prioridad MEDIA: [K]
- Prioridad BAJA: [L]

### Verificación Cruzada
- Preguntas que requieren revisión humana: [N]
  * [ID]: [motivo]
  * [ID]: [motivo]

### Cobertura Semántica
[Visualización de distribución por clusters temáticos]
```

### 5.3 Diff de Cambios Aplicados

```diff
=== REGISTRO DE MODIFICACIONES ===
Versión anterior: [hash_v1]
Versión actual: [hash_v2]

PREGUNTA 5:
- Original: "¿Cuál es el concepto de X mencionado en el texto?"
+ Reescrito: "¿Cómo define el texto el concepto de X?"
  Motivo: Duplicado parcial con pregunta 12 (sim=0.72)
  Prioridad: ALTA

PREGUNTA 18:
~ Ajuste de opciones (longitudes homogeneizadas ±10%)
  Motivo: Desviación en opción 3 (35% más larga)
  Prioridad: MEDIA
```

### 5.4 JSON de Metadatos (opcional, para exportación)

```json
{
  "metadata": {
    "documento_fuente": "nombre_archivo.pdf",
    "timestamp": "2025-10-15T14:23:00Z",
    "modelo_usado": "Claude-Opus-4.1",
    "version_prompt": "v2.0",
    "objetivo_preguntas": 50,
    "preguntas_generadas": 52,
    "conceptos_totales": 38,
    "conceptos_cubiertos": 36,
    "confidence_promedio": 0.82
  },
  "preguntas": [
    {
      "id": 1,
      "pregunta": "...",
      "opciones": ["...", "...", "..."],
      "respuesta_correcta": 2,
      "nivel_bloom": "Recordar",
      "confidence_score": 0.87,
      "cita_textual": "...",
      "concepto_origen": "...",
      "seccion_origen": "...",
      "distractores_metadata": [
        {"texto": "...", "estrategia": "negación", "similitud": 0.52},
        {"texto": "...", "estrategia": "generalización", "similitud": 0.61},
        {"texto": "...", "estrategia": "confusión_entidades", "similitud": 0.48}
      ]
    }
  ]
}
```

---

## SECCIÓN ESPECIAL: MANEJO DE CONTENIDO COMPLEJO

### Figuras y Tablas

```
SI leyenda textual existe:
  → Usar como cita
  → Generar pregunta sobre la información de la leyenda

SI NO hay leyenda:
  → Describir figura en ≤20 palabras
  → Referenciar párrafo que menciona la figura
  → Ejemplo: "La figura muestra [descripción breve] según se menciona en [párrafo X]"

SI NO es extraíble:
  → Marcar 'FIGURA_NO_EXTRAÍBLE'
  → NO generar pregunta derivada
  → Registrar en conceptos NO cubiertos
```

### Ecuaciones Matemáticas

```
SI hay LaTeX o notación matemática:
  → Preservar notación
  → Añadir descripción verbal en la pregunta
  → Ejemplo: "La ecuación E = mc² relaciona..."

Pregunta tipo:
  "Según el texto, ¿qué magnitudes relaciona la ecuación [mostrar]?"
  
Cita debe incluir:
  → Párrafo explicativo (no solo la ecuación)
```

### Código Fuente (si aparece en documento)

```
NO evaluar sintaxis del código
Evaluar: funcionalidad DESCRITA en el texto

Pregunta tipo:
  "Según la explicación del texto, ¿qué hace la función X?"

Cita:
  → Párrafo explicativo que describe el código
  → NO el código en sí
```

### Datos Tabulares

```
Extraer: insights explícitamente mencionados en el texto

Ejemplo correcto:
  Si texto dice: "la tabla muestra que X supera a Y en un 20%"
  Pregunta: "¿En qué porcentaje X supera a Y según los datos presentados?"

NUNCA inferir datos no mencionados explícitamente
```

---

## AUTODIAGNÓSTICO FINAL

Antes de entregar, ejecutar checklist completo:

```
CHECKLIST DE CALIDAD:
□ Usé código ejecutable para cálculos críticos
□ Todas las citas son textuales (0% inventadas)
□ Cada concepto extraído tiene ≥1 pregunta (salvo justificados)
□ No hay pares con similitud >0.70 sin justificar
□ Distribución Bloom dentro de tolerancias (±5%)
□ Confidence promedio del set final ≥0.70
□ Todos los distractores aparecen en el documento
□ Longitudes de opciones dentro de ±10%
□ Formato de Notas cumple estructura (Cita + Justificación)
□ Informe técnico incluye todos los apartados obligatorios
□ Matriz de cobertura generada
□ Conceptos NO cubiertos listados con razones
□ Cobertura semántica analizada (clusters)
□ Versionado y diff generados

Si CUALQUIER ítem falla → Re-ejecutar fase correspondiente
```

---

## OBSERVACIONES FINALES

### Escalabilidad

- Para documentos >10,000 palabras, el sistema puede generar hasta 100 preguntas si hay suficientes conceptos viables.
- Justificar en informe técnico cuando se exceda el objetivo base.

### Casos de Incompletitud

Si el documento no permite alcanzar el mínimo absoluto (30 preguntas):

- Entregar el conjunto viable
- Marcar 'INCOMPLETO' en el informe
- Listar conceptos/secciones que no pudieron cubrirse
- Explicar razones específicas

### Extensiones Futuras (no implementar ahora)

- Módulo de aprendizaje iterativo con feedback del usuario
- Preguntas reflexivas cada 15 ítems
- Análisis de predictibilidad mediante ML heurístico

---

## INICIO DE EJECUCIÓN

**Confirmación de parámetros actuales**:

- Modo: EJECUCIÓN COMPLETA (aplicar correcciones automáticamente)
- Resumen: OMITIDO (ir directo a generación)
- Modelo: Razonamiento extendido + Código

**Esperando documento...**

Una vez recibido el documento, ejecutar FASE 0: CALIBRACIÓN INTELIGENTE.
