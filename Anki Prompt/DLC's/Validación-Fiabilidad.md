

# VALIDACIÓN-FIABILIDAD DE BANCO MCQ — ANÁLISIS DIAGNÓSTICO

## OBJETIVO

Ejecutar auditoría exhaustiva sobre banco de preguntas MCQ generado. Calcular **Índice de Fiabilidad Riguroso** complementario a confianza básica. Diagnosticar problemas sistemáticos. **NO aplicar correcciones** — solo informar y recomendar.

---

## PARÁMETROS

```
similitud_duplicado_CRÍTICO = 0.85
similitud_duplicado_ADVERTENCIA = 0.70
solapamiento_crítico = 0.50
tolerancia_longitud = ±10%
similitud_distractor_ideal = 0.48-0.52
similitud_distractor_mínima = 0.40
bloom_tolerancia = ±5% (configurable por usuario)
umbral_fiabilidad_EXCELENTE = 0.85
umbral_fiabilidad_BUENO = 0.75
umbral_fiabilidad_ACEPTABLE = 0.60
```

---

## FASE 1: CÁLCULO DE ÍNDICE DE FIABILIDAD RIGUROSO

Por cada pregunta calcular **Fiabilidad Individual** con 5 dimensiones:

### Dimensión 1: Fidelidad textual (peso 25%)

**Pregunta + respuesta correcta:**

```
1.00 → Cita literal ≥3 palabras localizable en documento
0.80 → Paráfrasis precisa verificable en documento  
0.50 → Concepto correcto pero formulación ambigua
0.00 → No verificable en documento
```

**Distractores:**

```
1.00 → Basados en terminología/conceptos del documento
0.60 → Adaptados de contexto documento (relaciones derivables)
0.00 → Inventados sin base documental
```

Puntuación_fidelidad = (punt_pregunta × 0.6) + (promedio_punt_distractores × 0.4)

---

### Dimensión 2: Similitud distractores calibrada (peso 30%)

**Cálculo operativo por distractor:**

```
similitud = (palabras_comunes_raíz / total_palabras_opción_más_larga) × ajuste_estructura

Donde:
- palabras_comunes_raíz = contar palabras con misma raíz semántica
- ajuste_estructura = 1.0 si misma categoría gramatical, 0.9 si distinta
```

**Puntuación por similitud:**

```
0.48-0.52 → 1.00 (ideal, muy alta dificultad)
0.45-0.47 o 0.53-0.55 → 0.90
0.40-0.44 o 0.56-0.60 → 0.70
0.35-0.39 o 0.61-0.65 → 0.50
<0.35 o >0.65 → 0.00 (demasiado fácil/difícil)
```

**Verificación adicional (penalizaciones):**

- Distractor = opuesto obvio (aumenta/disminuye, antes/después) → -0.30
- Distractor difiere en ≥2 elementos simultáneos → -0.20
- Distractor obviamente descartable sin conocimiento → puntuación 0.00

Puntuación_similitud = promedio(puntuación_dist1, puntuación_dist2) + penalizaciones

---

### Dimensión 3: Homogeneidad estructural (peso 15%)

**Longitud opciones:**

```
media_palabras = (palabras_op1 + palabras_op2 + palabras_op3) / 3
desviación_máx = max(|palabras_opi - media| / media) para i=1,2,3

Puntuación:
≤ 0.10 → 1.00
0.11-0.15 → 0.80
0.16-0.20 → 0.60
0.21-0.30 → 0.30
> 0.30 → 0.00
```

**Pistas gramaticales (penalizaciones):**

- Concordancia género/número elimina opciones → -0.40
- Respuesta +30% más larga que promedio distractores → -0.30
- Palabras/raíces repetidas stem-respuesta ausentes en distractores → -0.20
- Especificidad asimétrica (respuesta mucho más detallada) → -0.20

Puntuación_homogeneidad = puntuación_longitud + penalizaciones (mín 0.00)

---

### Dimensión 4: Libre de sesgos (peso 15%)

**Detección automática palabras prohibidas en enunciado + 3 opciones:**

Lista completa: siempre, nunca, jamás, todos, ninguno, únicamente, exclusivamente, solamente, completamente, totalmente, absolutamente, invariablemente, categóricamente, frecuentemente, ocasionalmente, raramente, usualmente, habitualmente, normalmente, generalmente, típicamente, posiblemente, probablemente, quizás, aparentemente, presumiblemente, supuestamente, puede, podría, debe, debería, tendría, sería, está asociado con, es útil para, se relaciona con, contribuye a, se considera, se piensa que, todas las anteriores, ninguna de las anteriores, a y b son correctas, mejor sin "que", peor sin "que", mayor sin "que", menor sin "que", muy, bastante, demasiado, sumamente, extremadamente, particularmente, especialmente, notablemente, significativamente

```
0 palabras prohibidas → 1.00
1 palabra prohibida → 0.60
2 palabras prohibidas → 0.30
≥3 palabras prohibidas → 0.00
```

**Estructura problemática (penalizaciones):**

- Negación en stem (NO, EXCEPTO) sin mayúsculas → -0.30
- Múltiples objetivos en enunciado → -0.40
- Enunciado >4 líneas sin justificación → -0.20

Puntuación_sesgos = puntuación_base + penalizaciones (mín 0.00)

---

### Dimensión 5: Nivel Bloom apropiado (peso 15%)

**Verificación coherencia concepto-Bloom:**

```
Recordar asignado:
  ✓ Pide definición, dato, nombre → 1.00
  ✗ Presenta caso clínico, requiere razonamiento → 0.30

Comprender asignado:
  ✓ Pide explicación, comparación, interpretación → 1.00
  ✗ Solo requiere recordar dato → 0.50

Aplicar asignado:
  ✓ Presenta contexto/caso, requiere decisión → 1.00
  ✗ No hay contexto aplicado → 0.40
```

Puntuación_bloom = valor según verificación

---

### FÓRMULA FIABILIDAD INDIVIDUAL

```
Fiabilidad_individual = 
  (Fidelidad × 0.25) + 
  (Similitud_distractores × 0.30) + 
  (Homogeneidad × 0.15) + 
  (Libre_sesgos × 0.15) + 
  (Bloom_apropiado × 0.15)

Rango: 0.00-1.00
```

### FIABILIDAD GLOBAL DEL BANCO

```
Fiabilidad_banco = promedio(Fiabilidad_individual de todas las preguntas)

Interpretación:
0.85-1.00 → EXCELENTE (publicable sin revisión)
0.75-0.84 → BUENO (apto para uso, revisiones menores opcionales)
0.60-0.74 → ACEPTABLE (requiere revisión manual de ítems problemáticos)
<0.60 → INACEPTABLE (regenerar o revisión exhaustiva manual)
```

---

## FASE 2: DETECCIÓN DE PROBLEMAS SISTEMÁTICOS

### A. Duplicados semánticos

**Algoritmo:**

```
Por cada par de preguntas (i, j):
  1. Extraer conceptos clave (sustantivos técnicos + verbos nucleares)
  2. Calcular: ratio_conceptos = conceptos_compartidos / min(conceptos_i, conceptos_j)
  3. Comparar respuestas correctas: ratio_respuesta = palabras_comunes / total_palabras
  4. Similitud_total = (ratio_conceptos × 0.6) + (ratio_respuesta × 0.4)
  
  Si similitud_total ≥ 0.85 Y mismo Bloom → DUPLICADO_CRÍTICO
  Si similitud_total ≥ 0.70 → DUPLICADO_ADVERTENCIA
```

**Output:** Lista `[Nº i, Nº j, similitud, mismo_bloom, tipo]`

**Clasificación:**

- CRÍTICO (≥0.85): Preguntas prácticamente idénticas, eliminar una
- ADVERTENCIA (0.70-0.84): Evalúan mismo concepto, considerar si aportan valor diferencial

---

### B. Solapamiento de entidades técnicas

**Algoritmo:**

```
Por cada pregunta extraer: términos técnicos, nombres propios, conceptos nucleares
Por cada par (i, j):
  solapamiento = entidades_compartidas / total_entidades_únicas
  
  Si solapamiento > 0.50 → SOLAPAMIENTO_CRÍTICO
```

**Análisis diferenciador:**

```
Si preguntas evalúan:
  - Facetas distintas del concepto → ACEPTABLE (documentar justificación)
  - Misma faceta distinta formulación → REDUNDANTE (revisar necesidad)
```

**Output:** `[Nº i, Nº j, ratio, entidades_compartidas, tipo, justificación]`

---

### C. Coherencia de distribuciones

**Bloom:**

```
Real: Recordar X%, Comprender Y%, Aplicar Z%
Target: Recordar A%±5, Comprender B%±5, Aplicar C%±5
Delta = max(|Real - Target|) para cada nivel

Si Delta > bloom_tolerancia → BLOOM_DESBALANCEADO
```

**Output:** Tabla comparativa + lista de N preguntas candidatas para reclasificación (especificar de qué nivel a qué nivel)

**Cuotas de relevancia:**

```
Verificar proporciones reales:
- ESENCIAL: esperado ~45% ±3%
- TRONCAL: esperado ~32% ±3%
- SUBIDEA: esperado ~20% ±3%
- MENOR: esperado ≤3%

Si violación → CUOTA_VIOLADA
```

**Output:** Tabla real vs esperado + diagnóstico (ej: "exceso de preguntas MENOR indica documento con baja densidad conceptual")

---

### D. Análisis de confianza vs fiabilidad

**Comparar:**

```
Por cada pregunta: Confianza_básica vs Fiabilidad_rigurosa
Delta = Fiabilidad - Confianza

Si |Delta| > 0.20 → DISCREPANCIA_SIGNIFICATIVA
```

**Diagnóstico por tipo de discrepancia:**

```
Fiabilidad >> Confianza (delta > +0.20):
  → Confianza básica subestima calidad
  → Posible: distractores excelentes no valorados, fidelidad alta no capturada
  
Confianza >> Fiabilidad (delta < -0.20):
  → Problemas técnicos no detectados en generación
  → Investigar: sesgos ocultos, distractores aparentemente buenos pero con pistas, Bloom inadecuado
```

**Output:** Lista `[Nº, Confianza, Fiabilidad, Delta, Diagnóstico_probable]`

---

### E. Patrones de error recurrentes

**Identificar si ≥3 preguntas comparten mismo problema:**

```
Patrón A: Respuesta sistemáticamente más larga (+30%) que distractores
Patrón B: Palabras prohibidas recurrentes (misma palabra en múltiples preguntas)
Patrón C: Distractores consistentemente <0.40 similitud (demasiado fáciles)
Patrón D: Nivel Bloom sistemáticamente inadecuado para tipo de contenido
Patrón E: Enunciados sistemáticamente >4 líneas (verbosidad excesiva)
Patrón F: Opciones sistemáticamente asimétricas en especificidad
```

**Output:** Tabla `[Patrón, Frecuencia, Preguntas_afectadas, Implicación_para_generador]`

Implicación: sugerir ajustes concretos al prompt generador

---

## FASE 3: DIAGNÓSTICO Y RECOMENDACIONES

**Por cada categoría de problema, generar:**

### Duplicados

```
CRÍTICO Nº [i] y Nº [j] (similitud 0.XX, mismo Bloom):
  Evaluación: preguntas prácticamente idénticas
  Recomendación: ELIMINAR pregunta con menor fiabilidad individual [especificar Nº]
  
ADVERTENCIA Nº [i] y Nº [j] (similitud 0.XX):
  Evaluación: evalúan mismo concepto, posible redundancia
  Recomendación: REVISAR si aportan valor diferencial; considerar cambiar Bloom de una o enfocar matiz distinto
```

### Solapamientos

```
CRÍTICO Nº [i] y Nº [j] (ratio entidades 0.XX):
  Entidades compartidas: [lista]
  Evaluación: [ACEPTABLE - facetas distintas / REDUNDANTE - misma faceta]
  Recomendación: [Si REDUNDANTE] REFORMULAR una para diferenciar claramente o ELIMINAR
```

### Longitud

```
VIOLACIÓN Nº X: desviación máxima [%] (opción [N] tiene [M] palabras, media [P])
  Recomendación: AJUSTAR opción [N] a [rango palabras] manteniendo significado
```

### Distractores

```
FUERA_RANGO Nº X: distractor [N] similitud 0.XX (fuera de 0.48-0.52)
  Causa: [obviamente descartable / opuesto simple / diferencia amplia / muy similar]
  Recomendación: REEMPLAZAR con alternativa que comparta [especificar qué mantener] pero difiera en [especificar matiz]

OBVIAMENTE_DESCARTABLE Nº X: distractor [N]
  Razón: [requiere 0 conocimiento para descartar]
  Recomendación: SUSTITUIR con error conceptual plausible basado en [contexto del documento]
```

### Sesgos

```
PALABRA_PROHIBIDA Nº X: "[palabra]" en [stem/opción N]
  Recomendación: REFORMULAR eliminando término absoluto/vago/impreciso manteniendo objetivo evaluativo

ESTRUCTURA_PROBLEMÁTICA Nº X: [negación sin mayúsculas / múltiples objetivos / verbosidad]
  Recomendación: SIMPLIFICAR a objetivo único / RESALTAR negación / REDUCIR a <4 líneas
```

### Bloom

```
INADECUADO Nº X: clasificado como [Bloom actual] pero contenido requiere [Bloom correcto]
  Justificación: [presenta caso clínico pero clasificado Recordar / pide dato pero clasificado Aplicar]
  Recomendación: RECLASIFICAR a [Bloom correcto] sin modificar contenido
```

### Distribuciones

```
BLOOM_DESBALANCEADO:
  Nivel [X]: real [%], target [%], delta [+/- %]
  Recomendación: RECLASIFICAR [N] preguntas:
    - De Recordar a Comprender: Nº [lista] (justificación: contenido permite interpretación)
    - De Aplicar a Comprender: Nº [lista] (justificación: no hay contexto aplicado real)

CUOTA_VIOLADA:
  Nivel MENOR: [%] (esperado ≤3%)
  Diagnóstico: documento con baja densidad conceptual, exceso de datos menores
  Recomendación: REGENERAR priorizando conceptos ESENCIAL/TRONCAL o ACEPTAR si objetivo mínimo (30) alcanzado
```

---

## FORMATO DE SALIDA

### 1. Resumen Ejecutivo

```
**FIABILIDAD BANCO:** [0.XX] - [EXCELENTE / BUENO / ACEPTABLE / INACEPTABLE]
**Preguntas totales:** [N]
**Problemas detectados:**
  - Duplicados críticos: [N]
  - Duplicados advertencia: [N]
  - Solapamientos críticos: [N]
  - Violaciones longitud: [N]
  - Distractores fuera rango: [N]
  - Sesgos (palabras prohibidas): [N]
  - Bloom inadecuado: [N]
  - Distribución Bloom desbalanceada: [SÍ/NO, delta máx %]
  - Distribución cuotas violada: [SÍ/NO, nivel problemático]
  - Discrepancias confianza-fiabilidad >0.20: [N]
  - Patrones de error recurrentes: [N tipos detectados]

**VEREDICTO FINAL:**
[Si EXCELENTE]: Banco publicable sin revisión
[Si BUENO]: Apto para uso, revisiones menores opcionales en [N] ítems específicos
[Si ACEPTABLE]: Requiere revisión manual de [N] ítems problemáticos antes de uso
[Si INACEPTABLE]: Regenerar banco con prompt ajustado O revisión exhaustiva manual
```

---

### 2. Ranking de Fiabilidad Individual

**Top 10 mejores preguntas:**

```
| Nº | Confianza | Fiabilidad | Delta | Fidelidad | Similitud_dist | Homog | Sesgos | Bloom |
|----|-----------|------------|-------|-----------|----------------|-------|--------|-------|
| 5  | 0.85 | 0.92 | +0.07 | 0.95 | 0.90 | 1.00 | 1.00 | 1.00 |
| ... | ... | ... | ... | ... | ... | ... | ... | ... |
```

**Top 10 peores preguntas:**

```
| Nº | Confianza | Fiabilidad | Delta | Problemas_principales |
|----|-----------|------------|-------|----------------------|
| 23 | 0.78 | 0.42 | -0.36 | Distractor fácil (0.15), palabra prohibida "siempre", longitud asimétrica |
| ... | ... | ... | ... | ... |
```

---

### 3. Detalle Exhaustivo de Problemas

**A. DUPLICADOS**

CRÍTICOS (similitud ≥0.85, eliminar):

- Nº 12 y Nº 34 (sim=0.89, mismo Bloom): [diagnóstico + recomendación específica]
- [lista completa]

ADVERTENCIA (similitud 0.70-0.84, revisar):

- Nº 5 y Nº 28 (sim=0.76): [evaluación + recomendación]
- [lista completa]

---

**B. SOLAPAMIENTOS CRÍTICOS**

- Nº 7 y Nº 19 (ratio=0.58, entidades: [lista]): [tipo ACEPTABLE/REDUNDANTE + recomendación]
- [lista completa]

---

**C. DISTRACTORES PROBLEMÁTICOS**

FUERA DE RANGO (similitud no 0.48-0.52):

- Nº 3, distractor 2 (sim=0.22): [causa + recomendación]
- [lista completa]

OBVIAMENTE DESCARTABLES:

- Nº 15, distractor 1: [razón + recomendación alternativa]
- [lista completa]

---

**D. VIOLACIONES LONGITUD**

- Nº 8 (desv_máx=34%): opción 1 (15 pal), opción 2 (6 pal), opción 3 (14 pal), media=11.7 → ajustar opción 2
- [lista completa]

---

**E. SESGOS**

PALABRAS PROHIBIDAS:

- Nº 11, "siempre" en stem: [recomendación reformulación]
- Nº 22, "generalmente" en opción 2: [recomendación]
- [lista completa con frecuencia si palabra repetida]

ESTRUCTURAS PROBLEMÁTICAS:

- Nº 17, negación sin mayúsculas: [recomendación]
- Nº 29, múltiples objetivos: [recomendación simplificación]
- [lista completa]

---

**F. BLOOM INADECUADO**

- Nº 9 (Recordar → Comprender): [justificación + recomendación]
- [lista completa con dirección de reclasificación]

---

**G. DISTRIBUCIONES**

BLOOM:

```
Nivel       | Real  | Target     | Delta | Acción
Recordar    | 48%   | 40% ±5     | +8%   | Reclasificar 4 preguntas a Comprender: Nº [lista]
Comprender  | 25%   | 30% ±5     | -5%   | Límite tolerancia, revisar
Aplicar     | 27%   | 30% ±5     | -3%   | Dentro tolerancia
```

CUOTAS:

```
Nivel      | Real  | Esperado    | Evaluación
ESENCIAL   | 42%   | 45% ±3%     | Dentro rango
TRONCAL    | 34%   | 32% ±3%     | Dentro rango
SUBIDEA    | 18%   | 20% ±3%     | Dentro rango
MENOR      | 6%    | ≤3%         | VIOLADO - exceso preguntas menores
```

Diagnóstico cuota MENOR: [razón + recomendación]

---

**H. DISCREPANCIAS CONFIANZA-FIABILIDAD**

FIABILIDAD >> CONFIANZA (subestimación):

- Nº 14 (Conf=0.68, Fiab=0.91, +0.23): [diagnóstico probable]
- [lista completa]

CONFIANZA >> FIABILIDAD (sobreestimación):

- Nº 23 (Conf=0.82, Fiab=0.51, -0.31): [diagnóstico probable + problemas ocultos]
- [lista completa]

---

**I. PATRONES DE ERROR RECURRENTES**

Patrón detectado: RESPUESTA_MÁS_LARGA

- Frecuencia: 8 preguntas (12% del banco)
- Afectadas: Nº [lista]
- Implicación: generador sistemáticamente hace respuesta correcta más específica/detallada
- Ajuste sugerido al prompt: "Verificar longitud homogénea, respuesta NO debe ser más específica que distractores"

[Repetir para cada patrón detectado]

---

### 4. Análisis Dimensional Agregado

```
**Promedios por dimensión:**
Fidelidad textual: [0.XX] ([EXCELENTE >0.90 / BUENO 0.75-0.90 / ACEPTABLE 0.60-0.74 / BAJO <0.60])
Similitud distractores: [0.XX] [evaluación]
Homogeneidad estructural: [0.XX] [evaluación]
Libre de sesgos: [0.XX] [evaluación]
Bloom apropiado: [0.XX] [evaluación]

**Dimensión más problemática:** [nombre dimensión con menor puntuación]
  Causa principal: [análisis]
  Impacto: [N] preguntas afectadas ([%] del banco)
  Recomendación: [ajuste concreto al proceso de generación]

**Dimensión más fuerte:** [nombre dimensión con mayor puntuación]
  Análisis: [qué está haciendo bien el generador]
```

---

### 5. Informe Técnico Final (máx 400 palabras, separado `;`)

**Fiabilidad:** banco [0.XX] ([calificación]); por nivel ESENCIAL [0.XX], TRONCAL [0.XX], SUBIDEA [0.XX], MENOR [0.XX]; por dimensión fidelidad [0.XX], similitud_dist [0.XX], homogeneidad [0.XX], sesgos [0.XX], bloom [0.XX]

**Problemas críticos:** duplicados [N críticos, N advertencia]; solapamientos [N]; distractores [N fuera rango, N descartables]; sesgos [N palabras prohibidas, N estructuras]; longitud [N violaciones]; bloom [N inadecuados]

**Distribuciones:** Bloom real [R%, C%, A%] vs target [R%, C%, A%], delta máx [%] en [nivel]; Cuotas real [E%, T%, S%, M%] vs esperado, violación en [nivel si aplica]

**Discrepancias:** [N] preguntas con |confianza-fiabilidad| >0.20; tendencia [subestimación/sobreestimación/mixta]; causa probable [análisis]

**Patrones recurrentes:** [N] tipos detectados; más frecuente [nombre patrón, frecuencia, impacto]; segundo [si aplica]

**Dimensión débil:** [nombre] con promedio [0.XX]; causa [análisis]; [N] preguntas afectadas; impacto en fiabilidad global [estimación]

**Dimensión fuerte:** [nombre] con promedio [0.XX]; factor de éxito [qué hace bien el generador]

**Veredicto:** [EXCELENTE/BUENO/ACEPTABLE/INACEPTABLE]

**Acciones recomendadas:** [Si EXCELENTE]: ninguna, banco publicable [Si BUENO]: revisión opcional de [N] ítems específicos listados en sección problemas; banco apto para uso directo [Si ACEPTABLE]: revisión manual obligatoria de [N] ítems críticos ([% del banco]); enfoque en [tipo de problema predominante]; tras corrección manual re-ejecutar validación [Si INACEPTABLE]: regenerar completo con ajustes a prompt generador [listar ajustes específicos según patrones] O revisión exhaustiva manual de [%] del banco; fiabilidad actual inadecuada para uso

**Ajustes sugeridos a prompt generador:** [lista concreta basada en patrones recurrentes y dimensión débil]

---

### 6. Anexo: Tabla Completa de Métricas

```markdown
| Nº | Concepto | Nivel | Confianza | Fiabilidad | Delta | Fidelidad | Simil_dist | Homog | Sesgos | Bloom | Problemas |
|----|----------|-------|-----------|------------|-------|-----------|------------|-------|--------|-------|-----------|
| 1  | [nombre] | ESE   | 0.85      | 0.88       | +0.03 | 0.90      | 0.95       | 0.80  | 1.00   | 1.00  | Longitud leve |
| 2  | [nombre] | TRO   | 0.72      | 0.65       | -0.07 | 0.80      | 0.50       | 0.60  | 0.80   | 0.70  | Distractor fácil |
| ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |
```

[Incluir todas las preguntas para trazabilidad completa]


---
## INSTRUCCIONES DE USO

**Entrada requerida:**

1. Tabla Markdown con banco generado
2. Informe técnico del generador (recomendado, mejora diagnóstico)
3. Bloom target configurado

**Entrada opcional:** 4. Documento original (permite verificar fidelidad textual con mayor precisión)

**Output:**

- Diagnóstico exhaustivo con clasificación EXCELENTE/BUENO/ACEPTABLE/INACEPTABLE
- Detalle específico de problemas por pregunta
- Recomendaciones concretas de qué hacer (eliminar, reformular, reclasificar, ajustar)
- Tabla completa de métricas para trazabilidad
- Sugerencias de ajustes al prompt generador para futuras iteraciones

**NO incluye:**

- Correcciones aplicadas (solo diagnóstico y recomendaciones)
- Banco corregido (usuario debe implementar cambios manualmente o regenerar)

---

**FIN PROMPT VALIDACIÓN-FIABILIDAD**

