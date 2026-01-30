>DLC's
[[Datos/Anki/DLC's/Verificación 5.3]]
[[Datos/Anki/DLC's/Validación-Fiabilidad 5.3]]
[[Datos/Anki/DLC's/MCQ 5.3 Conceptos]]
[[Datos/Anki/DLC's/GENERADOR DE PREGUNTAS COMPLEMENTARIAS|GENERADOR DE PREGUNTAS COMPLEMENTARIAS]]

# GENERADOR DE PREGUNTAS TIPO TEST — NIVEL MIR/PIR

## TAREA
Generar banco de preguntas tipo test sobre documento(s) suministrado(s). Español formal. 

**Fidelidad obligatoria:**
- Preguntas y respuestas correctas: 100% del documento (prohibido inventar)
- Distractores: pueden construirse/adaptarse basándose en contexto del documento si necesario para cumplir criterio de dificultad
- Si información insuficiente para pregunta/respuesta → marcar INCOMPLETO

---

## PARÁMETROS
```
num_options = 3
objetivo = 30-70 preguntas (adaptativo según documento)
longitud_opciones = ±10% media
temperature = 0.0
plausibilidad_distractores = 0.48-0.52  # Similitud con respuesta correcta (muy alta dificultad)
```

**Distribución Bloom (configurable por usuario):**
- Recordar: 40% ±5
- Comprender: 30% ±5  
- Aplicar: 30% ±5

**Multi-documento:** Si hay 2 o más documentos, priorizar contradicciones en caso de contradicciones (hacer preguntas de aquello que salga en uno y no en otro). Documentar contradicción en informe.

---

## ANTI-SESGOS (palabras prohibidas y estructura)

**Palabras prohibidas en enunciado/opciones:**
- **Absolutos:** siempre, nunca, jamás, todos, ninguno, únicamente, exclusivamente, solamente, completamente, totalmente, absolutamente, invariablemente, definitivamente, categóricamente, sin excepción, en todos los casos
- **Cuantificadores:** 100%, 0%, cero, la totalidad
- **Vagos:** frecuentemente, ocasionalmente, a menudo, raramente, usualmente, habitualmente, normalmente, generalmente, comúnmente, típicamente, posiblemente, probablemente, quizás, tal vez, aparentemente, presumiblemente, supuestamente, varios, ciertos, determinados
- **Cueing:** puede, podría, debe, debería, tendría, sería, estaría
- **Imprecisos:** está asociado con, es útil para, se relaciona con, contribuye a, influye en, se considera, se piensa que, se cree que
- **Conectores:** todas las anteriores, ninguna de las anteriores, a y b son correctas, dos de las anteriores, todas excepto
- **Negativos en stem:** no, excepto, menos, salvo, incorrecto, falso (EVITAR; si inevitable MAYÚSCULAS y opciones positivas)
- **Prefijos negativos:** des-, in-, im-, il-, ir-, un-, non-, dis-, a-, anti- (reformular positivo)
- **Razón/causa:** porque, ya que, debido a, puesto que (evitar en verdadero/falso)
- **Comparativos sin referencia:** mejor, peor, mayor, menor, más, menos sin especificar "que qué"
- **Adverbios:** muy, bastante, demasiado, sumamente, extremadamente, particularmente, especialmente, notablemente, significativamente
- **Incertidumbre:** parece ser, se supone que, se asume que, sugiere que, indica que, pareciera que

**Enunciado:**
- Prohibido: espacios en blanco, enunciados incompletos, información innecesaria/contradictoria, múltiples objetivos, >4 líneas sin justificación
- Obligatorio: completo y autónomo, información común en stem (no en opciones), una pregunta clara, contexto realista

**Opciones:**
- Prohibido: número variable, verdadero/falso, formato K-Type, opciones que incluyen otras, mutuamente excluyentes obvias, sin paralelismo, longitudes dispares
- Obligatorio: misma categoría lógica, longitud similar (±10%), mismo nivel técnico/complejidad, coherencia gramatical con stem

**Distractores:**
- Prohibido: obviamente incorrectos, absurdos, implausibles, humorísticos, sin relación, contradictorios, de relleno, desactualizados
- Obligatorio: errores conceptuales comunes, plausibles, homogéneos, funcionales (>5% selección)

**Pistas (eliminar):**
- Palabras/raíces repetidas stem-respuesta
- Respuesta +30% más larga o -30% más corta que distractores
- Respuesta más específica/detallada/completa
- Concordancia género/número que elimina opciones
- Discordancia verbal
- Dos opciones opuestas

**Acción:** Verificar cada opción contra lista. Si detectas → reescribir inmediatamente.

---
## FILTRO DE RELEVANCIA (Pre-extracción)

**Información de ALTA relevancia (prioridad absoluta):**
- Marcos teóricos centrales (conceptos que organizan el campo)
- Definiciones consensuadas de conceptos clave
- Mecanismos de acción fundamentales
- Criterios diagnósticos/clasificaciones oficiales
- Indicaciones/contraindicaciones de primera línea
- Relaciones causales bien establecidas
- Limitaciones metodológicas mayores

**Información de BAJA relevancia (generar solo si espacio):**
- Porcentajes/datos de UN solo estudio sin réplica mencionada
- Números específicos sin implicación clínica directa
- Fechas de publicación de estudios individuales
- Nombres de autores de estudios únicos (salvo teorías epónimas)
- Datos epidemiológicos hiperespecíficos (prevalencia en subgrupo muy concreto)
- Detalles metodológicos de estudios concretos

**REGLA DE ORO:** Si el dato NO aparecería en un manual de referencia o guía clínica → BAJA relevancia

**Criterios de decisión rápida:**
1. ¿Es generalizable más allá del estudio específico? NO → Baja
2. ¿Cambiaría la práctica clínica/comprensión teórica? NO → Baja  
3. ¿Aparece en múltiples fuentes del documento? NO → Baja
4. ¿Es un número sin contexto interpretativo? SÍ → Baja

---

## EXTRACCIÓN

Extraer: el 20% que explique el 80% total, definiciones, metodología (si aplica), autores/teorías relevantes y repetidas, relaciones causales, limitaciones. Guardar citas textuales (≥3 palabras) con ubicación aproximada.

**Durante extracción, clasificar cada concepto:**

**ESENCIAL** (alta prioridad)
- Aparece en título/subtítulo principal del documento
- Definición de concepto clave mencionado ≥3 veces
- Marco teórico central explícitamente identificado como tal
- Criterio diagnóstico/clasificación oficial
- Mecanismo fundamental explicado en ≥2 párrafos

**TRONCAL** (prioridad media)
- Subtítulos secundarios del documento
- Relaciones causales entre conceptos esenciales
- Aplicaciones directas de marcos teóricos
- Comparaciones entre enfoques principales mencionadas
- Limitaciones mayores mencionadas explícitamente

**SUBIDEA** (prioridad baja, generar si espacio)
- Ejemplos de aplicación de ideas troncales
- Matices de conceptos esenciales
- Estudios relevantes citados ≥2 veces en documento
- Datos cuantitativos con implicación clínica clara explicada

**MENOR** (muy baja prioridad, solo si crítico para mínimo 30)
- Datos de un solo estudio sin réplica mencionada
- Porcentajes sin contexto o sin implicación clara
- Nombres propios sin teoría epónima asociada
- Números específicos sin interpretación clínica

**DESCARTABLE** (NUNCA generar preguntas)
- Fechas de publicación de estudios
- Detalles metodológicos específicos (tamaño muestral, diseño concreto)
- Números de muestra de estudios individuales
- Datos epidemiológicos hiperespecíficos (prevalencia en subgrupo muy concreto sin réplica)

**Output de extracción:** Listar todos los conceptos identificados con su clasificación.

---

## ESTRATIFICACIÓN Y OBJETIVO

### Paso 1: Calcular objetivo base

**Por longitud del documento:**
```
<2.000 palabras → objetivo_base = 30
2.000-5.000 → objetivo_base = 50
5.000-10.000 → objetivo_base = 70
>10.000 → objetivo_base = 70
```

**Ajuste por densidad conceptual:**
```
densidad = (num_ESENCIAL + num_TRONCAL) / (palabras_documento / 1000)

Si densidad > 0.7 → objetivo = objetivo_base × 1.20
Si densidad < 0.3 → objetivo = objetivo_base × 0.80
Si no → objetivo = objetivo_base

Límites finales: MIN(70, MAX(30, objetivo))
```

### Paso 2: Asignar cuotas por nivel de relevancia

**Distribución porcentual fija:**
```
Cuota_ESENCIAL = ROUND(objetivo × 0.45)    # 40-50% del total
Cuota_TRONCAL = ROUND(objetivo × 0.325)    # 30-35% del total
Cuota_SUBIDEA = ROUND(objetivo × 0.20)     # 15-25% del total
Cuota_MENOR = objetivo - (Cuota_ESENCIAL + Cuota_TRONCAL + Cuota_SUBIDEA)  # Residuo (0-10%)
```

### Paso 3: Verificar viabilidad

**Si hay muy pocos conceptos ESENCIAL:**
```
mínimo_por_esencial = 2  # Garantía de calidad

Si num_conceptos_ESENCIAL × mínimo_por_esencial > Cuota_ESENCIAL:
    → Incrementar objetivo en 20% Y recalcular cuotas
    O
    → Reducir mínimo_por_esencial a 1.5 (redondear al generar)
```

**Si hay demasiados conceptos ESENCIAL:**
```
Si num_conceptos_ESENCIAL > Cuota_ESENCIAL:
    → Repartir equitativamente (algunas esenciales tendrán 1 pregunta)
    → Priorizar por frecuencia de aparición en documento
```

### Paso 4: Calcular distribución por concepto

**Para conceptos ESENCIAL:**
```
preguntas_base_por_esencial = FLOOR(Cuota_ESENCIAL / num_conceptos_ESENCIAL)
residuo_esencial = Cuota_ESENCIAL MOD num_conceptos_ESENCIAL

Asignar preguntas_base a todos los conceptos ESENCIAL
Asignar +1 pregunta a los "residuo_esencial" conceptos con:
  - Mayor extensión (más párrafos dedicados)
  - Mayor frecuencia (más menciones en documento)
  - Más relaciones con otros conceptos
```

**Para conceptos TRONCAL:**
```
preguntas_base_por_troncal = FLOOR(Cuota_TRONCAL / num_conceptos_TRONCAL)
residuo_troncal = Cuota_TRONCAL MOD num_conceptos_TRONCAL

Si num_conceptos_TRONCAL ≤ 10:
    → Garantizar mínimo 1 pregunta por concepto
    → Distribuir residuo por frecuencia

Si num_conceptos_TRONCAL > 10:
    → Algunos conceptos pueden quedar sin pregunta
    → Priorizar por frecuencia/extensión
```

**Para conceptos SUBIDEA:**
```
Si Cuota_SUBIDEA ≥ num_conceptos_SUBIDEA:
    → 1 pregunta por concepto
    → Distribuir residuo por relevancia

Si Cuota_SUBIDEA < num_conceptos_SUBIDEA:
    → Seleccionar los Cuota_SUBIDEA conceptos más relevantes:
        - Mencionados ≥2 veces
        - Con ejemplos concretos
        - Con datos cuantitativos interpretados
```

**Para conceptos MENOR:**
```
Solo usar Cuota_MENOR si:
  - Total de preguntas ESENCIAL + TRONCAL + SUBIDEA < 30
  - Y no hay más conceptos SUBIDEA disponibles

Máximo 1 pregunta por concepto MENOR
Confianza máxima forzada a 0.60
```

---

## ESTRATEGIA DE COBERTURA PRIORIZADA

**Pase 1 — Esenciales (hasta agotar Cuota_ESENCIAL)**

Para cada concepto ESENCIAL:
1. Generar número de preguntas asignado en Paso 4
2. Variar nivel Bloom dentro del concepto:
   - Si 3+ preguntas → incluir Recordar, Comprender, Aplicar
   - Si 2 preguntas → Recordar + Comprender O Comprender + Aplicar
   - Si 1 pregunta → nivel más apropiado al contenido
3. Registrar cobertura

**Pase 2 — Troncales (hasta agotar Cuota_TRONCAL)**

Para cada concepto TRONCAL seleccionado:
1. Generar número de preguntas asignado
2. Variar Bloom si tiene 2+ preguntas
3. Si concepto no tiene preguntas asignadas → añadir a NO_CUBIERTOS con razón "cuota troncal agotada, priorizado por frecuencia"

**Pase 3 — Subideas (hasta agotar Cuota_SUBIDEA)**

Para cada concepto SUBIDEA seleccionado:
1. Generar 1 pregunta (raramente 2)
2. Nivel Bloom más apropiado al tipo de información
3. Conceptos SUBIDEA no seleccionados → añadir a NO_CUBIERTOS con razón "cuota subidea agotada, priorizados conceptos con ejemplos/datos"

**Pase 4 — Control de calidad**

Verificar:
```
Total_generado = preguntas_ESENCIAL + preguntas_TRONCAL + preguntas_SUBIDEA

Si Total_generado < 30:
    → Generar preguntas de conceptos MENOR hasta alcanzar 30
    → Documentar en informe: "Añadidas X preguntas MENOR (confianza máx 0.60)"

Si Total_generado > 70:
    → ERROR de cálculo, revisar distribución
    
Si Total_generado entre 30-70:
    → Proceder a validación
```

### Conceptos NO_CUBIERTOS

Documentar conceptos sin preguntas con razón específica:
- **Cuota agotada:** "Concepto TRONCAL sin asignación, cuota agotada tras priorizar por frecuencia"
- **Información insuficiente:** "Cita ambigua, no permite pregunta de calidad"
- **Clasificado DESCARTABLE:** "Fecha de publicación, información no examinable"
- **Distractores imposibles:** "No se pueden generar distractores 0.48-0.52 ni adaptando contexto"

---
## SISTEMA DE CONFIANZA

**Objetivo:** Cuantificar calidad técnica Y relevancia examinable de cada pregunta.

**Fórmula multidimensional:**
```
Confianza = (Relevancia × 0.35) + (Cita_válida × 0.25) + (Distractores_plausibles × 0.25) + (Fidelidad × 0.15)
```

### Dimensión 1: Relevancia (peso 35%)

Refleja importancia del contenido evaluado:
```
ESENCIAL → 1.00    # Marcos teóricos, definiciones clave, criterios diagnósticos
TRONCAL → 0.85     # Relaciones causales, aplicaciones, limitaciones mayores
SUBIDEA → 0.70     # Ejemplos, matices, estudios relevantes citados ≥2 veces
MENOR → 0.40       # Datos únicos, porcentajes sin réplica
```

**Techo forzado:** Preguntas MENOR tienen confianza final máxima 0.60 independiente de otros componentes.

### Dimensión 2: Cita válida (peso 25%)
```
1.00 → Cita literal ≥3 palabras del documento
0.50 → Paráfrasis válida identificable en documento
0.00 → Sin cita o cita no localizable
```

### Dimensión 3: Distractores plausibles (peso 25%)

Similitud promedio de los 2 distractores con respuesta correcta:
```
1.00 → Similitud 0.48-0.52 (ideal, muy alta dificultad)
0.90 → Similitud 0.45-0.47 o 0.53-0.55
0.70 → Similitud 0.40-0.44 o 0.56-0.60
0.50 → Similitud 0.35-0.39 o 0.61-0.65
0.30 → Similitud 0.30-0.34 o 0.66-0.70
0.00 → Similitud <0.30 o >0.70
```

**Cálculo:**
```
similitud_distractor_1 = proporción_terminología_compartida_con_correcta
similitud_distractor_2 = proporción_terminología_compartida_con_correcta
puntuación_distractores = promedio(similitud_1, similitud_2) → aplicar tabla arriba
```

### Dimensión 4: Fidelidad (peso 15%)
```
1.00 → Pregunta Y respuesta correcta 100% del documento
0.50 → Respuesta correcta del documento, distractores adaptados de contexto documento
0.00 → Información inventada sin base en documento
```

### Aplicación durante generación

Al generar cada pregunta:
1. Identificar nivel de relevancia del concepto (ya asignado en extracción)
2. Verificar existencia de cita literal ≥3 palabras
3. Estimar similitud de distractores con correcta
4. Confirmar fidelidad al documento
5. Calcular confianza con fórmula
6. **Si pregunta es MENOR → aplicar techo de 0.60 final**

### Interpretación de valores finales
```
0.85-1.00 → Pregunta de alta calidad técnica y alta relevancia (ideal)
0.70-0.84 → Calidad buena, revisar si se puede mejorar distractores
0.60-0.69 → Aceptable (típico de preguntas MENOR por techo)
0.40-0.59 → Baja calidad, considerar marcar INCOMPLETO
<0.40 → Rechazar o marcar INCOMPLETO
```

---
---

## GENERACIÓN DE PREGUNTAS

### Enunciado
Claro, auto-contenido, 15-40 palabras. Evitar dobles negaciones. Una sola respuesta correcta posible. **Basado exclusivamente en documento.**

### Opciones
 Opción 1 / Opción 2 / Opción 3`

**Longitud uniforme:** Contar palabras de cada opción. Calcular media. Cada opción debe estar en rango media ±10%. Si fuera → ajustar.

### Distractores (RANGO 0.48-0.52 — Muy alta dificultad)

**Criterio de similitud:** Cada distractor debe tener similitud 0.48-0.52 con respuesta correcta (compartir casi toda la terminología, diferenciarse solo en matices específicos).

**Características obligatorias:**
- Compartir >80% terminología con correcta
- Diferencia en: matices específicos (mecanismo primario vs secundario, timing preciso, dosis específica, contexto clínico puntual)
- Requerir conocimiento muy detallado para descartarlos
- **Basados en contexto del documento** (conceptos, procesos, terminología mencionados)

**EXCEPCIÓN DE FIDELIDAD:**
- Si documento contiene información detallada → usar literalmente
- Si documento carece de suficiente detalle para distractores 0.48-0.52 → **PERMITIDO construir/adaptar** distractores basándose en:
  - Terminología del documento
  - Procesos mencionados en documento
  - Contexto clínico/conceptual del documento
  - Relaciones lógicas derivables del contenido
- **PROHIBIDO:** inventar conceptos/términos no relacionados con documento

**Evitar:** opuestos obvios (antes/después, aumenta/disminuye, oral/intravenoso), conceptos generales relacionados (IECA vs ARA-II si solo difieren en clase farmacológica).

**Si imposible generar distractores 0.48-0.52 incluso adaptando contexto → marcar INCOMPLETO.**

### Cita textual
Fragmento literal del documento (≥3 palabras) que soporte respuesta correcta. Anotar ubicación aproximada.

### Answer
Byte-a-byte idéntico a opción correcta, sin prefijo:  [texto completo]

### Bloom
- **Recordar:** definición, dato, nombre
- **Comprender:** explicar, interpretar, comparar
- **Aplicar:** caso clínico, decisión terapéutica

Ajustar generación para distribución objetivo.

### Anti-sesgos aplicado
Verificar cada opción contra lista de palabras prohibidas. Si detectas → reescribir.

### Información faltante
Si concepto requiere información no presente para **pregunta o respuesta correcta** → NO inventar. Marcar INCOMPLETO, documentar qué falta, continuar.

---

## VALIDACIÓN BÁSICA

Durante generación verificar:
- Cita ≥3 palabras presente
- Opciones ±10%
- Cero palabras prohibidas
- Answer coincide con opción

Si duplicado obvio (misma información, mismo enfoque) → reescribir con enfoque distinto.

---

## FORMATO DE SALIDA

### Tabla Markdown

**Cabecera exacta:**
```
| Nº | Pregunta | Opción 1 | Opción 2 | Opción 3 | Answer | Nota | Bloom | Confianza |
```

**Columnas:**

- **Nº:** Secuencial (1, 2, 3...)
- **Pregunta:** Enunciado completo
- **Opción 1, 2, 3** 
- **Answer:** Byte-a-byte idéntico a opción correcta
- **Nota:** →
```
  'Cita textual'; No son las otras opciones porque [palabra clave o frase clave de distractor] y [palabra clave o frase clave de distractor]
```
  - Cita entre comillas simples
  - Separador: `;` después de cita
  - Explicación máx 50-60 palabras 
  - Solo texto, `:` y `;` (prohibido `|`, `"`, saltos línea)
  
- **Bloom:** Recordar | Comprender | Aplicar
- **Confianza:** 0.00-1.00 calculada como: ``` Confianza = (Relevancia × 0.35) + (Cita_válida × 0.25) + (Distractores_plausibles × 0.25) + (Fidelidad × 0.15) ```

**Ejemplo Nota:**
```
'Los IECA bloquean enzima convertidora de angiotensina'; No son las otras opciones porque [Byte-a-byte idéntico a opción 1, sin prefijo, o palabra clave que resuma la alternativa, sin poner el número de opción] describe ARA-II que bloquean receptores downstream y [Byte-a-byte idéntico a opción 2, sin prefijo, o palabra clave que resuma la alternativa, sin poner el número de opción] describe inhibidores de renina upstream
```

---

### Informe Técnico (máx 400 palabras)

**Formato:** Párrafo continuo, separado con `;`
**Contenido obligatorio:**
**Documento:** palabras totales [N]; densidad [(E+T)/(pal/1000)] [valor]; tipo [Teórico/Clínico/Mixto]; elementos no textuales [tablas N, figuras N, listas N] clasificados como [nivel predominante]
**Extracción:** ESENCIAL [N conceptos: lista nombres]; TRONCAL [N conceptos: lista]; SUBIDEA [N total, N seleccionados]; MENOR [N]; DESCARTABLE [N, razón principal]
**Objetivo:** base [N]; densidad ajustada [factor]; final [N]
**Cuotas asignadas:** ESENCIAL [N, %]; TRONCAL [N, %]; SUBIDEA [N, %]; MENOR [N, %]
**Generadas:** total [N]; ESENCIAL [N, %real]; TRONCAL [N, %real]; SUBIDEA [N, %real]; MENOR [N, %real]
**Bloom:** Recordar [N, %]; Comprender [N, %]; Aplicar [N, %]; target [distribución configurada]; desviación [máx delta%]
**Confianza:** promedio [0.XX]; ESENCIAL [0.XX]; TRONCAL [0.XX]; SUBIDEA [0.XX]; MENOR [0.XX]
**Matriz cobertura ESENCIAL:** [Concepto1: N preguntas (R:X, C:X, A:X)]; [Concepto2: ...]; cobertura [%conceptos con ≥2 preguntas]
**NO_CUBIERTOS:** TRONCAL [N conceptos: razones]; SUBIDEA [N: razones]; conceptos con info insuficiente [N: nombres]
**Incidencias:** INCOMPLETAS [N: razones específicas]; palabras prohibidas corregidas [N ejemplos]; longitud ajustada [N preguntas]; distractores adaptados contexto [N]; duplicados detectados [N]; reclasificaciones [si aplica]
**Alertas críticas:** [si confianza <0.70 indicar razón]; [si ESENCIAL con <2 preguntas indicar cuáles]; [si cuota MENOR utilizada indicar N]; [si Bloom fuera tolerancia indicar deltas]


## SALIDA

Entregar:
1. Tabla Markdown completa
2. Informe Técnico (incluir matriz cobertura y NO_CUBIERTOS)

No mostrar razonamiento interno.
### FEEDBACK ADAPTATIVO

**Durante generación, si se detecta:**
```
Confianza promedio <0.65 tras 20 preguntas → ampliar tolerancia distractores a 0.45-0.55; documentar en informe
>30% preguntas INCOMPLETO → bajar Bloom (Aplicar→Comprender→Recordar); si persiste documentar "documento insuficiente"
Cuota ESENCIAL inalcanzable → reclasificar top-3 TRONCAL como ESENCIAL; recalcular cuotas; documentar reclasificación
Bloom imposible (contenido homogéneo) → permitir ±10% desviación; documentar adaptación
``````
---

**FIN DEL PROMPT**