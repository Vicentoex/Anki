# GENERADOR DE SIMULACRO — Expansion

## TAREA

Generar simulacro de examen tipo test sobre documento(s) suministrado(s). Español formal. Orientado a rendimiento: menos preguntas, más difíciles, distribución realista de examen.

**Cantidad de preguntas:** Definida por el usuario. Si no especifica, default = 30.

**Fidelidad obligatoria:**

- Preguntas y respuestas correctas: 100% del documento (prohibido inventar)
- Distractores: construidos recombinando elementos que SÍ aparecen en el documento
    - Permitido: recombinar relaciones entre conceptos del documento (atribuir mecanismo A a receptor Y si ambos existen en el texto)
    - Prohibido: introducir conceptos, términos o mecanismos no mencionados en el documento
- Si información insuficiente para pregunta o respuesta correcta → marcar INCOMPLETO

---

## PARÁMETROS

```
num_options = 3
objetivo = definido por usuario (default 30)
longitud_opciones = ±10% media
```

---

## NIVEL COGNITIVO (Bloom × DOK)

Columna unificada que cruza la acción cognitiva (Bloom) con la complejidad del pensamiento (DOK de Webb).

|Nivel|Bloom|DOK|Qué evalúa|Stem típico|Carga cognitiva|
|---|---|---|---|---|---|
|Nivel 1|Recordar|DOK 1: Reproducir|Datos, definiciones, hechos; recuperación directa de memoria|Directo y cerrado: "¿Cuál es el mecanismo de acción de...?"|Baja: 1 paso cognitivo|
|Nivel 2|Comprender / Aplicar|DOK 2: Habilidades y conceptos|Interpretar, comparar, clasificar, aplicar en contexto familiar; conexión de 2 variables|Contextual con caso simple: "Un paciente presenta X, ¿qué indica...?"|Media: 2 pasos, relación causa-efecto lineal|
|Nivel 3|Analizar|DOK 3: Pensamiento estratégico|Integrar datos contradictorios, resolver problemas no rutinarios, justificar decisiones entre alternativas válidas|Caso complejo con datos contradictorios o ruido: "Ante estos resultados opuestos, ¿qué intervención prioriza...?"|Alta: 3+ pasos, reconocimiento de patrones, integración|

**Distribución objetivo:**

- Nivel 1: 20% ±5
- Nivel 2: 60% ±5
- Nivel 3: 20% ±5

**Reglas de validación por nivel:**

- Nivel 1: Prohibido usar stems abiertos como "¿Cuál de las siguientes es correcta?". Obligatorio stem cerrado que especifique qué se pregunta.
- Nivel 3: Obligatorio incluir datos contradictorios o información irrelevante (ruido) en el stem para forzar razonamiento, no simple asociación de patrones.

---

## DISTRIBUCIÓN DE DIFICULTAD

```
Fácil: 20%   → Stem directo y corto; distractores con 2 diferencias conceptuales
Medio: 60%   → Stem contextual; distractores con 1-2 diferencias conceptuales
Difícil: 20% → Subdividido:
  - 10% Trampa: distractores espejo, inversión lógica o sobreinclusión (1 diferencia)
  - 10% Microconcepto débil: evalúa excepciones, matices o condiciones límite
```

**Dificultad y Nivel Cognitivo son dimensiones independientes.** Un ítem Nivel 1 con distractor espejo es difícil. Un ítem Nivel 3 con distractor plausible es medio. La dificultad la determina el tipo de distractor y la especificidad del stem.

---

## ANTI-SESGOS

**Palabras prohibidas en enunciado y opciones:**

- **Absolutos:** siempre, nunca, jamás, todos, ninguno, únicamente, exclusivamente, solamente, completamente, totalmente, absolutamente, invariablemente, definitivamente, categóricamente, sin excepción, en todos los casos
- **Cuantificadores:** 100%, 0%, cero, la totalidad
- **Vagos:** frecuentemente, ocasionalmente, a menudo, raramente, usualmente, habitualmente, normalmente, generalmente, comúnmente, típicamente, posiblemente, probablemente, quizás, tal vez, aparentemente, presumiblemente, supuestamente, varios, ciertos, determinados
- **Cueing:** puede, podría, debe, debería, tendría, sería, estaría → **prohibidas en opciones; permitidas en stems de Nivel 2-3** cuando forman parte natural de la pregunta clínica
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

- Prohibido: espacios en blanco, enunciados incompletos, información innecesaria o contradictoria (salvo ruido intencional en Nivel 3), múltiples objetivos, más de 4 líneas sin justificación
- Obligatorio: completo y autónomo, información común en stem (no repetida en opciones), una sola pregunta clara, 15-40 palabras (Nivel 3 puede extenderse hasta 60 palabras si incluye caso con datos)

Opciones:

- Prohibido: número variable entre ítems, verdadero/falso, formato K-Type, opciones que incluyen otras, mutuamente excluyentes obvias, longitudes dispares
- Obligatorio: misma categoría lógica, longitud similar (±10%), mismo nivel técnico, coherencia gramatical con stem, paralelismo sintáctico

**Pistas a eliminar:**

- Palabras o raíces repetidas entre stem y respuesta correcta
- Respuesta correcta más de 30% más larga o más corta que distractores
- Respuesta correcta más específica, detallada o completa que distractores
- Concordancia de género o número que elimina opciones
- Discordancia verbal entre stem y alguna opción
- Dos opciones opuestas

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

---

## EXTRACCIÓN

Extraer el 20% del contenido que explique el 80% del documento. Guardar citas textuales (≥3 palabras) con ubicación aproximada.

**Clasificar cada concepto:**

**ESENCIAL** — Título o subtítulo principal; definición mencionada ≥3 veces; marco teórico central; criterio diagnóstico o clasificación oficial; mecanismo explicado en ≥2 párrafos.

**TRONCAL** — Subtítulos secundarios; relaciones causales entre conceptos esenciales; aplicaciones de marcos teóricos; comparaciones entre enfoques; limitaciones explícitas.

**SUBIDEA** — Ejemplos de aplicación; matices de conceptos esenciales; estudios citados ≥2 veces; datos cuantitativos con implicación clara.

**MENOR** — Datos de un solo estudio; porcentajes sin contexto; nombres sin teoría epónima. Solo usar si necesario para alcanzar objetivo mínimo.

**DESCARTABLE** — Fechas de publicación, tamaños muestrales, detalles metodológicos de estudios individuales. No generar preguntas.

---

## ESTRATIFICACIÓN

### Distribuir cuotas

- 45% del objetivo → conceptos ESENCIAL
- 30% → TRONCAL
- 20% → SUBIDEA
- 5% restante → MENOR (solo si necesario)

### Asignar preguntas por concepto

- Repartir equitativamente dentro de cada nivel. Conceptos con más extensión o menciones reciben prioridad.
- Variar Nivel Cognitivo dentro de cada concepto: si tiene 2+ preguntas, usar niveles distintos.
- Garantizar mínimo 1 pregunta por concepto ESENCIAL.

### Documentar NO_CUBIERTOS con razón.

---

## GENERACIÓN DE PREGUNTAS

### Objetivo del ítem

Cada ítem evalúa un microconcepto específico.

### Stem

- Nivel 1: directo y cerrado, especifica qué se pregunta
- Nivel 2: contextual, caso simple o interpretación de datos
- Nivel 3: caso complejo con datos contradictorios o ruido intencional; fuerza razonamiento, no asociación

### Answer

La mejor opción entre las presentadas. Sin pistas por longitud, tecnicismo o repetición de palabras del stem.

### Distractores

**Regla de diferencia graduada:**

|Dificultad|Diferencias conceptuales vs correcta|
|---|---|
|Fácil|2 diferencias|
|Medio|1-2 diferencias|
|Difícil|Exactamente 1 diferencia|

Verificación:

1. Alinear distractor con respuesta correcta
2. Contar puntos de divergencia conceptual
3. Si divergen en más de lo permitido → reescribir
4. Si divergen en 0 → reescribir

**Tipos de distractor:**

FÁCIL y MEDIO:

- **Plausible:** terminología correcta aplicada al concepto equivocado
- **Casi-correcto:** correcto en otro contexto del documento

DIFÍCIL — TRAMPA:

- **Espejo:** modifica exactamente 1 elemento de la correcta
- **Inversión lógica:** invierte 1 relación (causa↔efecto, condición↔resultado)
- **Sobreinclusión:** añade 1 elemento extra que invalida la respuesta

DIFÍCIL — MICROCONCEPTO DÉBIL:

- Distractores plausible o casi-correcto, pero el contenido evaluado es una excepción, matiz o condición límite

**Si imposible generar distractores con la regla de diferencia graduada → marcar INCOMPLETO.**

### Cita textual

Fragmento literal del documento (≥3 palabras) que soporte la respuesta correcta.

---

## VALIDACIÓN

Verificar cada ítem:

1. Cita ≥3 palabras del documento presente
2. Longitud de opciones dentro de ±10% de la media
3. Cero palabras prohibidas en stem y opciones
4. Answer byte-a-byte idéntico a una de las opciones
5. Sin duplicados
6. Pistas eliminadas
7. Nivel 1: stem cerrado (no "¿cuál es correcta?")
8. Nivel 3: stem incluye datos contradictorios o ruido

---

## RELEVANCIA DE EBEL

Clasificar cada ítem según su relevancia para la práctica:

|Categoría|Criterio|Peso en examen|
|---|---|---|
|Esencial|Conocimiento crítico; error aquí implica consecuencia directa|Alta prioridad|
|Importante|Conocimiento necesario para práctica competente|Prioridad media|
|Aceptable|Detalles técnicos finos; diferencia al experto del competente|Prioridad baja|

Distribución orientativa: 40% Esencial, 40% Importante, 20% Aceptable.

---

## PROBABILIDAD DE ANGOFF (Cold Start)

Para cada ítem, estimar la probabilidad de que un candidato mínimamente competente (θ_MCC) lo acierte. Actúa como juez experto sintético.

**Algoritmo paso a paso:**

1. **Clasificar:** Asignar Nivel Cognitivo (1/2/3) y detectar errores de construcción del ítem (IWFs).
2. **Estimar dificultad teórica (b):** Basándose en:
    - Nivel Cognitivo: Nivel 1 → b bajo; Nivel 2 → b medio; Nivel 3 → b alto
    - Tipo de distractor: plausible → b menor; espejo/inversión → b mayor
    - Carga cognitiva: pasos necesarios para acertar
    - Plausibilidad semántica de distractores: a mayor similitud con la clave, mayor b
3. **Calcular probabilidad:** Aplicar modelo logístico simplificado:
    
    ```
    P(acierto) = 1 / (1 + e^(b - θ_MCC))
    ```
    
    Donde θ_MCC = 0 (candidato límite por convención).
4. **Expresar como porcentaje:** Redondear a múltiplos de 5%.

**Rangos orientativos:**

```
Nivel 1 + distractor fácil       → 75-85%
Nivel 1 + distractor difícil     → 55-65%
Nivel 2 + distractor fácil/medio → 55-70%
Nivel 2 + distractor difícil     → 40-55%
Nivel 3 + distractor medio       → 35-50%
Nivel 3 + distractor difícil     → 25-40%
```

---

## FORMATO DE SALIDA

### 1. Tabla de Simulacro

**Cabecera exacta:**

```
| Nº | Pregunta | Opción 1 | Opción 2 | Opción 3 | Answer | Nota | Nivel Cognitivo | Dificultad |
```

**Columnas:**

- **Nº:** secuencial
- **Pregunta:** enunciado completo
- **Opción 1, 2, 3**
- **Answer:** byte-a-byte idéntico a la opción correcta
- **Nota:** formato →

```
'cita textual del documento'; No son las otras porque [razón distractor 1] y [razón distractor 2]
```

- Cita entre comillas simples; separador `;`; máx 50 palabras; solo texto `:` y `;` (prohibido `|` `"` saltos de línea)
- **Nivel Cognitivo:** N1 | N2 | N3
- **Dificultad:** Fácil | Medio | Difícil-Trampa | Difícil-Micro

---

### 2. Blueprint

|Nº|Tema|Subtema / Microconcepto|Dificultad|Nivel Cognitivo|Carga Cognitiva|Tipo distractor|Ebel|Prob.Angoff|
|---|---|---|---|---|---|---|---|---|
||||Fácil/Medio/Difícil|N1/N2/N3|Baja/Media/Alta|Plausible/Casi-correcto/Espejo/Inversión/Sobreinclusión|Esencial/Importante/Aceptable|%|

**Resúmenes al final del blueprint:**

```
Nivel Cognitivo: N1 [N, %]; N2 [N, %]; N3 [N, %]; target [20/60/20]; desviación [deltas]
Dificultad: Fácil [N, %]; Medio [N, %]; Difícil-Trampa [N, %]; Difícil-Micro [N, %]; target [20/60/10/10]; desviación [deltas]
Ebel: Esencial [N, %]; Importante [N, %]; Aceptable [N, %]
Angoff promedio: [%]; rango [min%-max%]
```

---

### 3. Mapa de Calor por Microconcepto

Tabla que clasifica cada microconcepto evaluado en el simulacro según cobertura y dificultad estimada.

**El usuario usará esta tabla para actualizar su estudio tras resolver el simulacro.** Los colores indican prioridad de repaso.

|Microconcepto|Nº ítems|Nivel Cognitivo predominante|Dificultad predominante|Angoff medio|Estado|
|---|---|---|---|---|---|
|[nombre]|[N]|N1/N2/N3|Fácil/Medio/Difícil|[%]|🟢 / 🟡 / 🔴|

**Criterios de color:**

```
🟢 Verde: Angoff medio ≥65% Y dificultad predominante Fácil o Medio
         → Microconcepto accesible; repaso ligero suficiente

🟡 Amarillo: Angoff medio 45-64% O dificultad mixta
         → Microconcepto con riesgo; requiere repaso dirigido

🔴 Rojo: Angoff medio <45% O dificultad predominante Difícil
         → Microconcepto crítico; requiere estudio profundo antes del examen
```

**Instrucción al usuario:** Tras resolver el simulacro, marcar los ítems fallados. Los microconceptos con ítems fallados suben un nivel de alerta (verde→amarillo, amarillo→rojo).

---

### 4. Análisis de Errores por Tipo de Distractor

Tabla de referencia para que el usuario identifique patrones en sus errores tras resolver el simulacro.

|Tipo de distractor|Nº ítems en banco|Descripción del error que detecta|Acción de repaso|
|---|---|---|---|
|Plausible|[N]|Falta de dominio del concepto base; confunde terminología correcta con concepto equivocado|Repasar definiciones y diferencias entre conceptos cercanos|
|Casi-correcto|[N]|Falta de discriminación contextual; sabe el concepto pero no distingue en qué contexto aplica|Repasar condiciones de aplicación y excepciones|
|Espejo|[N]|Lectura superficial; no detecta el detalle modificado|Practicar lectura lenta de opciones; comparar opciones entre sí antes de elegir|
|Inversión lógica|[N]|Confusión de dirección causal o relacional|Repasar relaciones causa-efecto; dibujar diagramas de flujo|
|Sobreinclusión|[N]|Sesgo hacia la opción más completa; no detecta el elemento invalidante|Practicar identificación de "el elemento que sobra" en cada opción|

**Instrucción al usuario:** Tras resolver, clasificar cada error en el tipo de distractor que lo causó. Si ≥3 errores del mismo tipo → patrón cognitivo a trabajar.

---

### 5. Entrenamiento de Microconceptos Fallados

Sección generada solo si el usuario proporciona sus resultados del simulacro (ítems fallados).

**Si el usuario indica qué ítems falló, generar:**

Para cada microconcepto con ≥1 fallo:

```
MICROCONCEPTO: [nombre]
CONCEPTO CLAVE: [explicación en 2-3 frases basada en el documento]
ERROR DETECTADO: [qué tipo de distractor causó el fallo y qué implica]
ÍTEM DE REFUERZO: [1 pregunta adicional sobre el mismo microconcepto con enfoque distinto al del simulacro]
```

Reglas del ítem de refuerzo:

- Mismo microconcepto, diferente ángulo (si el simulacro preguntó definición, el refuerzo pregunta aplicación, o viceversa)
- Mismas reglas de calidad que el simulacro (anti-sesgos, fidelidad, cita)
- Si el fallo fue en distractor espejo → el refuerzo usa distractor casi-correcto (evitar repetir la misma trampa)

**Si el usuario no proporciona resultados:** Omitir esta sección e indicar: "Tras resolver el simulacro, indica los Nº de ítems fallados para generar entrenamiento específico."

---

## INFORME TÉCNICO (máx 300 palabras)

Formato: párrafo continuo separado con `;`

**Documento:** palabras totales [N]; tipo [Teórico/Clínico/Mixto]

**Extracción:** ESENCIAL [N conceptos: nombres]; TRONCAL [N: nombres]; SUBIDEA [N total, N seleccionados]; MENOR [N]; DESCARTABLE [N]

**Generadas:** total [N]; por nivel: ESENCIAL [N, %]; TRONCAL [N, %]; SUBIDEA [N, %]; MENOR [N, %]

**Nivel Cognitivo real:** N1 [N, %]; N2 [N, %]; N3 [N, %]; desviación vs target

**Dificultad real:** Fácil [N, %]; Medio [N, %]; Difícil-Trampa [N, %]; Difícil-Micro [N, %]

**Ebel:** Esencial [N, %]; Importante [N, %]; Aceptable [N, %]

**Angoff:** promedio [%]; rango [min%-max%]; ítems con Angoff <35% [N: cuáles]

**Cobertura ESENCIAL:** [Concepto: N preguntas]; cobertura [% conceptos con ≥2 preguntas]

**NO_CUBIERTOS:** [N conceptos: razones]

**Incidencias:** INCOMPLETAS [N: razones]; palabras prohibidas corregidas [N]; distractores recombinados [N]; Nivel 3 sin ruido en stem [N, corregidos]

**Alertas:** [Nivel Cognitivo fuera tolerancia: deltas]; [Angoff promedio <50%: razón]; [ESENCIAL con <2 preguntas: cuáles]

---

## FEEDBACK ADAPTATIVO

|Señal|Umbral|Corrección|Documentar|
|---|---|---|---|
|Ítems INCOMPLETO excesivos|>25% del banco|Bajar Nivel Cognitivo (N3→N2→N1); si persiste: "documento insuficiente"|Sí|
|Nivel Cognitivo imposible|Contenido homogéneo|Permitir ±10% desviación en distribución|Sí|
|Distractores imposibles|>15% ítems sin distractor válido|Permitir 1 diferencia adicional para esos ítems|Sí|
|Objetivo inalcanzable|Documento corto para objetivo pedido|Reducir objetivo; documentar|Sí|
|Angoff promedio muy bajo|<40% promedio|Revisar si Nivel 3 está sobrerepresentado; ajustar distribución|Sí|
|Nivel 3 sin ruido en stem|Cualquier ítem N3 sin datos contradictorios|Reescribir stem añadiendo dato contradictorio o irrelevante del documento|Sí|

---

## SALIDA

Entregar:

1. Tabla de Simulacro
2. Blueprint con resúmenes
3. Mapa de Calor
4. Análisis de Errores por Tipo de Distractor
5. Entrenamiento de Microconceptos (si usuario proporcionó resultados; si no, indicar instrucción)
6. Informe Técnico

No mostrar razonamiento interno ni pasos intermedios.

---

**FIN DEL PROMPT**