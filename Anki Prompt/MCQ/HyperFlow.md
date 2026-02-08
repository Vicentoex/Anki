# GENERADOR DE PREGUNTAS TIPO TEST — HyperFlow v2

## TAREA

Generar banco de preguntas tipo test sobre documento(s) suministrado(s). Español formal.

**Fidelidad obligatoria:**

- Preguntas y respuestas correctas: 100% del documento (prohibido inventar)
- Distractores: construidos recombinando elementos que SÍ aparecen en el documento
    - Permitido: si el documento menciona "mecanismo A actúa en receptor X" y "mecanismo B actúa en receptor Y", un distractor válido es atribuir mecanismo A a receptor Y
    - Prohibido: introducir conceptos, términos o mecanismos no mencionados en el documento
- Si información insuficiente para pregunta o respuesta correcta → marcar INCOMPLETO

---
## PARÁMETROS

```
num_options = 3
objetivo = 40-55 preguntas (adaptativo según documento)
longitud_opciones = ±10% media
```

**Distribución Bloom:**

- Recordar: 40% ±5
- Comprender: 40% ±5
- Aplicar: 20% ±5

**Distribución Dificultad:**

- Fácil: 40% → stem corto y directo; distractores tipo plausible o casi-correcto
- Medio: 40% → stem contextual; distractores tipo plausible o casi-correcto
- Difícil: 20% → distractores tipo espejo, inversión lógica o sobreinclusión

**Dificultad y Bloom son dimensiones independientes.** Un ítem de Recordar con distractor espejo es difícil. Un ítem de Aplicar con distractor plausible es medio. La dificultad la determina el tipo de distractor y la especificidad del stem, no el nivel Bloom.

---

## ANTI-SESGOS

**Palabras prohibidas en enunciado y opciones:**

- **Absolutos:** siempre, nunca, jamás, todos, ninguno, únicamente, exclusivamente, solamente, completamente, totalmente, absolutamente, invariablemente, definitivamente, categóricamente, sin excepción, en todos los casos
- **Cuantificadores:** 100%, 0%, cero, la totalidad
- **Vagos:** frecuentemente, ocasionalmente, a menudo, raramente, usualmente, habitualmente, normalmente, generalmente, comúnmente, típicamente, posiblemente, probablemente, quizás, tal vez, aparentemente, presumiblemente, supuestamente, varios, ciertos, determinados
- **Cueing:** puede, podría, debe, debería, tendría, sería, estaría → **prohibidas en opciones; permitidas en stems de ítems Aplicar** cuando forman parte natural de la pregunta (ej: "¿qué intervención es la indicada?")
- **Imprecisos:** está asociado con, es útil para, se relaciona con, contribuye a, influye en, se considera, se piensa que, se cree que
- **Conectores:** todas las anteriores, ninguna de las anteriores, a y b son correctas, dos de las anteriores, todas excepto
- **Negativos en stem:** no, excepto, menos, salvo, incorrecto, falso (EVITAR; si inevitable usar MAYÚSCULAS y opciones positivas)
- **Prefijos negativos:** des-, in-, im-, il-, ir-, un-, non-, dis-, a-, anti- (reformular en positivo)
- **Comparativos sin referencia:** mejor, peor, mayor, menor, más, menos sin especificar respecto a qué
- **Adverbios de grado:** muy, bastante, demasiado, sumamente, extremadamente, particularmente, especialmente, notablemente, significativamente
- **Incertidumbre:** parece ser, se supone que, se asume que, sugiere que, indica que, pareciera que

**Acción:** Verificar cada opción contra esta lista. Si detectas palabra prohibida → reescribir inmediatamente.

**Reglas de estructura:**

Enunciado:

- Prohibido: espacios en blanco, enunciados incompletos, información innecesaria o contradictoria, múltiples objetivos, más de 4 líneas sin justificación
- Obligatorio: completo y autónomo, información común en stem (no repetida en opciones), una sola pregunta clara, 15-40 palabras

Opciones:

- Prohibido: número variable entre ítems, verdadero/falso, formato K-Type, opciones que incluyen otras, mutuamente excluyentes obvias, longitudes dispares
- Obligatorio: misma categoría lógica, longitud similar (±10%), mismo nivel técnico, coherencia gramatical con stem, paralelismo sintáctico

**Pistas a eliminar:**
- Palabras o raíces repetidas entre stem y respuesta correcta
- Respuesta correcta más de 30% más larga o más corta que distractores
- Respuesta correcta más específica, detallada o completa que distractores
- Concordancia de género o número que elimina opciones
- Discordancia verbal entre stem y alguna opción
- Dos opciones opuestas (señalan que una de ellas es correcta)

---
## FILTRO DE RELEVANCIA

**ALTA relevancia (prioridad):**
- Marcos teóricos centrales
- Definiciones consensuadas de conceptos clave
- Mecanismos de acción fundamentales
- Criterios diagnósticos y clasificaciones oficiales
- Relaciones causales bien establecidas
- Limitaciones metodológicas mayores

**BAJA relevancia (solo si espacio):**
- Datos de un solo estudio sin réplica
- Números sin implicación clínica directa
- Fechas de publicación
- Nombres de autores sin teoría epónima
- Datos epidemiológicos hiperespecíficos

**Regla de oro:** Si el dato no aparecería en un manual de referencia → baja relevancia.

---
## EXTRACCIÓN

Extraer el 20% del contenido que explique el 80% del documento. Guardar citas textuales (≥3 palabras) con ubicación aproximada.

**Clasificar cada concepto extraído:**
**ESENCIAL** — Título o subtítulo principal; definición mencionada ≥3 veces; marco teórico central; criterio diagnóstico o clasificación oficial; mecanismo explicado en ≥2 párrafos.
**TRONCAL** — Subtítulos secundarios; relaciones causales entre conceptos esenciales; aplicaciones de marcos teóricos; comparaciones entre enfoques; limitaciones explícitas.
**SUBIDEA** — Ejemplos de aplicación; matices de conceptos esenciales; estudios citados ≥2 veces; datos cuantitativos con implicación clara.
**MENOR** — Datos de un solo estudio; porcentajes sin contexto; nombres sin teoría epónima. Solo usar si se necesitan para alcanzar mínimo 40 preguntas.
**DESCARTABLE** — Fechas de publicación, tamaños muestrales, detalles metodológicos de estudios individuales. No generar preguntas.
**Output:** Listar todos los conceptos con su clasificación antes de generar preguntas.

---
## ESTRATIFICACIÓN Y OBJETIVO

### Calcular objetivo

```
<2.000 palabras → objetivo_base = 40
2.000-5.000 → objetivo_base = 48
5.000-10.000 → objetivo_base = 55
>10.000 → objetivo_base = 55
```

Ajustar: si la ratio de conceptos ESENCIAL + TRONCAL por cada 1.000 palabras es alta (>0.7), subir 15%. Si es baja (<0.3), bajar 15%. Límites finales: mínimo 40, máximo 55.

### Distribuir cuotas

- 45% del objetivo → preguntas sobre conceptos ESENCIAL
- 30% → sobre TRONCAL
- 20% → sobre SUBIDEA
- 5% restante → sobre MENOR (solo si necesario para alcanzar mínimo 40)

### Asignar preguntas por concepto

- Repartir equitativamente dentro de cada nivel. Si un concepto ESENCIAL tiene más extensión o más menciones en el documento, asignarle una pregunta adicional.
- Garantizar mínimo 1 pregunta por concepto ESENCIAL y por concepto TRONCAL (si hay cuota).
- Variar Bloom dentro de cada concepto: si tiene 2+ preguntas, usar niveles Bloom distintos.

### Documentar NO_CUBIERTOS

Conceptos sin preguntas: anotar razón (cuota agotada, información insuficiente, clasificado descartable, distractores imposibles).

---

## GENERACIÓN DE PREGUNTAS

### Objetivo del ítem

Cada ítem evalúa un microconcepto específico. Ejemplos: diferencia entre A y B en contexto Z; excepción X dentro del concepto Y; condición necesaria vs suficiente.

### Stem

- Tipos: directo (recuerdo), contextual (caso o narrativa), analítico (integración), negativo (¿cuál NO es...?, solo si bien construido)
- Claro, sin ambigüedades, sin pistas, sin dobles negaciones
- No repetir palabras que aparecen en una sola opción

### Answer

La mejor opción entre las presentadas (no perfecta, sino la más correcta). Sin pistas por longitud, tecnicismo o repetición de palabras del stem.

### Distractores

**Regla de diferencia graduada:** Cada distractor comparte estructura, terminología y extensión con la respuesta correcta, diferenciándose en un número controlado de elementos conceptuales (mecanismo, temporalidad, agente, condición o contexto).

Grado de diferencia según dificultad del ítem:

- Fácil: 2 diferencias conceptuales respecto a la correcta
- Medio: 1-2 diferencias conceptuales
- Difícil: exactamente 1 diferencia conceptual

Verificación para cada distractor:

1. Alinear distractor con respuesta correcta palabra por palabra
2. Contar puntos de divergencia conceptual
3. Si divergen en más elementos de lo permitido por su dificultad → reescribir
4. Si divergen en 0 elementos → reescribir (ambiguo)

**Tipos de distractor por dificultad:**

FÁCIL y MEDIO:

- **Plausible:** parece correcto si no se domina el concepto; usa terminología del campo correctamente pero la aplica al concepto equivocado
- **Casi-correcto:** correcto en otro contexto del documento, requiere discriminar en qué contexto aplica cada afirmación

DIFÍCIL:

- **Espejo:** casi idéntico a la respuesta correcta pero con un detalle crítico cambiado (un valor, una condición, una dirección causal); construido modificando exactamente 1 elemento de la respuesta correcta
- **Inversión lógica:** invierte una relación del documento (causa↔efecto, antecedente↔consecuente, condición↔resultado, general↔específico)
- **Sobreinclusión:** toma la respuesta correcta y añade un elemento extra que la invalida; parece más completa o profesional pero es incorrecta por el añadido

**Distractor mal construido (señales de alerta):**

- Obviamente incorrecto o sin relación con el stem
- Longitud muy distinta a las otras opciones
- Errores gramaticales
- Contiene palabras prohibidas de la lista anti-sesgos

**Si es imposible generar distractores con la regla de diferencia graduada → marcar INCOMPLETO.**

### Cita textual

Cada pregunta requiere un fragmento literal del documento (≥3 palabras) que soporte la respuesta correcta.

### Nivel Bloom

- **Recordar:** identificar, definir, reconocer, listar
- **Comprender:** explicar, interpretar, comparar, diferenciar, relacionar
- **Aplicar:** resolver caso, decidir en contexto, implementar, ejecutar

---

## VALIDACIÓN

Durante generación, verificar cada ítem:

1. Cita ≥3 palabras del documento presente
2. Longitud de opciones dentro de ±10% de la media
3. Cero palabras prohibidas en stem y opciones
4. Answer byte-a-byte idéntico a una de las opciones
5. Sin duplicados (mismo concepto + mismo enfoque → reescribir uno)
6. Pistas eliminadas (revisar lista de pistas)

---

## BLUEPRINT

**Dificultad ≠ Bloom.** Cruce libre entre ambas dimensiones.

|Dificultad|% del banco|Subtipo|Tipo de distractor|Bloom permitido|
|---|---|---|---|---|
|Fácil|40%|—|Plausible, casi-correcto|Cualquiera|
|Medio|40%|—|Plausible, casi-correcto|Cualquiera|
|Difícil|10%|Trampa|Espejo, inversión, sobreinclusión|Cualquiera|
|Difícil|10%|Microconcepto débil|Plausible o casi-correcto; la dificultad viene de evaluar excepciones, matices o condiciones límite del concepto, no del tipo de distractor|Cualquiera|

La diferencia entre Fácil y Medio: el stem fácil es directo y corto (distractores con 2 diferencias); el stem medio es contextual con caso, datos o narrativa (distractores con 1-2 diferencias). Los ítems Difícil-Trampa usan distractores con exactamente 1 diferencia. Los ítems Difícil-Microconcepto evalúan contenido ultra-específico (excepciones, matices, condiciones límite).

**Tabla blueprint de salida:**

|Tema|Subtema / Microconceptos|Dificultad|Bloom|Tipo de distractor|Total|
|---|---|---|---|---|---|

**Resumen Bloom:** Recordar [N, %]; Comprender [N, %]; Aplicar [N, %]; target [40/40/20]; desviación [delta%]

---

## FORMATO DE SALIDA

### Tabla Markdown

**Cabecera exacta:**

```
| Nº | Pregunta | Opción 1 | Opción 2 | Opción 3 | Answer | Nota | Bloom |
```

**Columnas:**

- **Nº:** secuencial
- **Pregunta:** enunciado completo
- **Opción 1, 2, 3:** las tres opciones
- **Answer:** byte-a-byte idéntico a la opción correcta, sin prefijo
- **Nota:** formato →

```
'cita textual del documento'; No son las otras porque [razón distractor 1] y [razón distractor 2] (Explicación clara y justificada)
```

- Cita entre comillas simples
- Separador `;` después de cita
- Explicación máx 50 palabras
- Solo texto, `:` y `;` (prohibido `|`, `"`, saltos de línea)
- Las razones son por qué cada distractor es incorrecto, en palabras clave (no repetir texto completo de la opción)
- **Bloom:** Recordar | Comprender | Aplicar

---

### Informe Técnico (máx 300 palabras)

Formato: párrafo continuo separado con `;`
**Contenido obligatorio:**
**Documento:** palabras totales [N]; densidad conceptual [valor]; tipo [Teórico/Clínico/Mixto]
**Extracción:** ESENCIAL [N conceptos: nombres]; TRONCAL [N: nombres]; SUBIDEA [N total, N seleccionados]; MENOR [N]; DESCARTABLE [N]
**Objetivo:** base [N]; ajustado [N]; final [N]
**Generadas:** total [N]; por nivel: ESENCIAL [N, %]; TRONCAL [N, %]; SUBIDEA [N, %]; MENOR [N, %]
**Bloom real:** Recordar [N, %]; Comprender [N, %]; Aplicar [N, %]; desviación vs target [deltas]
**Dificultad real:** Fácil [N, %]; Medio [N, %]; Difícil [N, %]
**Cobertura ESENCIAL:** [Concepto1: N preguntas (R:X, C:X, A:X)]; cobertura [% conceptos con ≥2 preguntas]
**NO_CUBIERTOS:** [N conceptos: razones resumidas]
**Incidencias:** INCOMPLETAS [N: razones]; palabras prohibidas corregidas [N]; longitud ajustada [N]; distractores recombinados de documento [N]; duplicados eliminados [N]
**Alertas:** [ESENCIAL con <2 preguntas: cuáles]; [Bloom fuera de tolerancia: deltas]; [cuota MENOR utilizada: N]

---

## FEEDBACK ADAPTATIVO

La IA debe monitorear estos umbrales durante la generación y aplicar la corrección indicada:

|Señal|Umbral|Corrección|Documentar en informe|
|---|---|---|---|
|Ítems INCOMPLETO excesivos|>25% del banco|Bajar nivel Bloom (Aplicar→Comprender→Recordar); si persiste, documentar "documento insuficiente para objetivo"|Sí: razón y N afectados|
|Cuota ESENCIAL inalcanzable|Menos conceptos ESENCIAL que preguntas asignadas|Reclasificar los 3 TRONCAL más frecuentes como ESENCIAL; recalcular cuotas|Sí: qué conceptos reclasificados|
|Bloom imposible|Contenido homogéneo impide distribución 40/40/20|Permitir ±10% desviación|Sí: distribución real y razón|
|Distractores imposibles|>15% ítems sin distractor válido con regla de diferencia graduada|Permitir 1 diferencia adicional para esos ítems|Sí: N ítems afectados|
|Objetivo inalcanzable|Documento demasiado corto para 40 preguntas de calidad|Reducir objetivo a 30; documentar|Sí: razón y objetivo final|

---

## SALIDA

Entregar:

1. Tabla Markdown completa
2. Blueprint
3. Informe Técnico

No mostrar razonamiento interno ni pasos intermedios de extracción.

---

**FIN DEL PROMPT**