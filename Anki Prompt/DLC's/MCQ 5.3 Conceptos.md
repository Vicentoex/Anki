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
- Recordar: 100% 
- Comprender: 0% 
- Aplicar: 0%

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

## EXTRACCIÓN Y TAREA

**Generar preguntas para:**

- Todos los conceptos identificados en el documento

---

## FORMATO DE PREGUNTA

**Estructura obligatoria:**

**Pregunta:** La definición textual completa extraída literalmente del documento (cita exacta entre comillas del texto original)

**Opciones de respuesta:**

- Deben ser nombres de conceptos relacionados
- Todas las opciones deben pertenecer a la misma categoría conceptual (ej: si la correcta es un tipo de atención, todas deben ser tipos de atención)
- Nivel de dificultad ALTO: Las opciones deben ser extremadamente similares y difíciles de distinguir
- Las opciones incorrectas (distractores) deben ser conceptos que:
    - Compartan la misma familia o categoría (ej: todas son alteraciones cuantitativas de consciencia)
    - Tengan definiciones que puedan confundirse fácilmente con la pregunta
    - Requieran conocimiento preciso del matiz específico para descartarlas
- **Prohibido:** opciones obvias, de categorías diferentes, o fácilmente descartables

**Respuesta correcta:** El nombre exacto del concepto cuya definición aparece en la pregunta

---

**EJEMPLO CORRECTO:**

**Pregunta:** "En este caso la persona pierde la capacidad de reconocer como propios sus pensamientos, impulsos, sentimientos o acciones"

**Opción 1:** Confusión de los límites del yo  
**Opción 2:** Pérdida de atribución personal  
**Opción 3:** Deterioro en la unidad del yo

**Answer:** Pérdida de atribución personal

_(Las tres opciones son alteraciones de la conciencia del sí mismo, muy similares entre sí)_

---

**EJEMPLO INCORRECTO:**

**Pregunta:** "Se refiere a la capacidad para atender a un estímulo sin confundirse con distractores"

**Opción 1:** Atención selectiva  
**Opción 2:** Depresión  
**Opción 3:** Hipervigilancia

**Razón:** "Depresión" es un trastorno (categoría diferente), no un tipo de atención. Las opciones no son comparables.

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
- **Confianza:** 0.00-1.00 (cita válida 30% + distractores plausibles 40% + fidelidad 30%)

**Ejemplo Nota:**
```
'Los IECA bloquean enzima convertidora de angiotensina'; No son las otras opciones porque [Byte-a-byte idéntico a opción 1, sin prefijo, o palabra clave que resuma la alternativa, sin poner el número de opción] describe ARA-II que bloquean receptores downstream y [Byte-a-byte idéntico a opción 2, sin prefijo, o palabra clave que resuma la alternativa, sin poner el número de opción] describe inhibidores de renina upstream
```

---

### Informe Técnico (máx 300 palabras)

Incluir: longitud documento, densidad conceptual, conceptos extraídos, objetivo calculado, preguntas generadas, **matriz de cobertura (conceptos | preguntas asignadas | pase)**, cobertura esenciales, **listado NO_CUBIERTOS (conceptos sin preguntas con razón)**, duplicados detectados/resueltos, confianza promedio, distribución Bloom (% por nivel), incidencias (INCOMPLETAS con razón, palabras prohibidas corregidas, ajustes longitud, distractores adaptados de contexto).

Formato: párrafo continuo, datos separados con `;`

---

## SALIDA

Entregar:
1. Tabla Markdown completa
2. Informe Técnico (incluir matriz cobertura y NO_CUBIERTOS)

No mostrar razonamiento interno.

---

**FIN DEL PROMPT**