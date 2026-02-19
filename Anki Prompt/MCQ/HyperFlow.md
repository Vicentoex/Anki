# GENERADOR DE PREGUNTAS TIPO TEST — HyperFlow v3

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
```
Fácil:  40% → stem corto y directo
Medio:  40% → stem contextual (caso, datos, narrativa)
Difícil: 20% → subdividido:
  - 10% Trampa: distractores espejo, inversión lógica o sobreinclusión
  - 10% Microconcepto débil: excepciones, matices o condiciones límite
```

**Dificultad y Bloom son dimensiones independientes.** Un ítem de Recordar con distractor espejo es difícil. Un ítem de Aplicar con distractor plausible es medio. La dificultad la determina el tipo de distractor y la especificidad del stem, no el nivel Bloom.

---

## ANTI-SESGOS

**Palabras prohibidas en enunciado y opciones:**
- **Absolutos:** siempre, nunca, jamás, todos, ninguno, únicamente, exclusivamente, solamente, completamente, totalmente, absolutamente, invariablemente, definitivamente, categóricamente, sin excepción, en todos los casos, idéntico, exacto
- **Cuantificadores:** 100%, 0%, cero, la totalidad
- **Vagos:** frecuentemente, ocasionalmente, a menudo, raramente, usualmente, habitualmente, normalmente, generalmente, comúnmente, típicamente, posiblemente, probablemente, quizás, tal vez, aparentemente, presumiblemente, supuestamente, varios, ciertos, determinados, diversos, múltiples
- **Cueing:** puede, podría, debe, debería, tendría, sería, estaría → **prohibidas en opciones; permitidas en stems de ítems Aplicar** cuando forman parte natural de la pregunta clínica
- **Imprecisos:** está asociado con, es útil para, se relaciona con, contribuye a, influye en, se considera, se piensa que, se cree que
- **Conectores:** todas las anteriores, ninguna de las anteriores, a y b son correctas, dos de las anteriores, todas excepto
- **Negativos en stem:** no, excepto, menos, salvo, incorrecto, falso (EVITAR; si inevitable usar MAYÚSCULAS y opciones positivas)
- **Prefijos negativos:** des-, in-, im-, il-, ir-, un-, non-, dis-, a-, anti- (reformular en positivo)
- **Comparativos sin referencia:** mejor, peor, mayor, menor, más, menos sin especificar respecto a qué
- **Adverbios de grado:** muy, bastante, demasiado, sumamente, extremadamente, particularmente, especialmente, notablemente, significativamente, rápidamente, velozmente, radicalmente, drásticamente, brevemente
- **Adjetivos valorizadores sin contenido evaluable:** fundamental, esencial, crucial, clave, importante, crítico, principal, primordial — solo permitidos si el documento los usa como término técnico con significado diferenciado (ej: "criterio esencial" como categoría formal)
- **Incertidumbre:** parece ser, se supone que, se asume que, sugiere que, indica que, pareciera que

**Principio general anti-sesgos:** Además de la lista explícita, cualquier palabra que cumpla la misma función semántica que las listadas debe tratarse como prohibida. Esto incluye sinónimos directos (ej: "esporádicamente" → vago; "de manera rápida" → adverbio de grado; "breves" aplicado a medida temporal → adverbio de grado) y reformulaciones equivalentes. Si una palabra no está en la lista pero podría funcionar como pista, absoluto, cuantificador o adverbio de grado → reescribir.

**Acción:** Verificar cada opción contra esta lista Y contra el principio general. Si detectas palabra prohibida o equivalente semántico → reescribir inmediatamente.

**Reglas de estructura:**

Enunciado:
- Prohibido: espacios en blanco, enunciados incompletos, información innecesaria o contradictoria, múltiples objetivos, más de 4 líneas sin justificación, padding léxico (adjetivos, adverbios, cadenas de sinónimos o frases introductorias que no aportan información evaluable)
- Obligatorio: completo y autónomo, información común en stem (no repetida en opciones), una sola pregunta clara
- **Límite duro de longitud:** Recordar y Comprender: 15-40 palabras. Aplicar (con caso clínico): 15-60 palabras. Si un stem supera su límite → recortar eliminando información no esencial hasta cumplir. Si tras recortar sigue excediendo → reescribir desde cero.
- **Test de padding:** Antes de finalizar cada stem, releer cada oración y preguntar: "¿Eliminar esta oración/frase impediría responder la pregunta?" Si la respuesta es no → eliminar.

Opciones:
- Prohibido: número variable entre ítems, verdadero/falso, formato K-Type, opciones que incluyen otras, mutuamente excluyentes obvias, longitudes dispares
- Obligatorio: misma categoría lógica, longitud similar (±10%), mismo nivel técnico, coherencia gramatical con stem, paralelismo sintáctico
- **Verificación de longitud:** Contar caracteres de cada opción. Si la opción más larga supera en >10% a la más corta → recortar la más larga o expandir la más corta hasta cumplir. No entregar el ítem sin que las 3 opciones estén dentro del ±10%.

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

**La extracción y clasificación es un paso interno.** No mostrar la lista de conceptos en la salida. La clasificación se usa internamente para estratificar y se refleja en el blueprint y el informe técnico.

---

## ESTRATIFICACIÓN Y OBJETIVO

### Calcular objetivo

```
<2.000 palabras  → objetivo_base = 40
2.000-5.000      → objetivo_base = 48
5.000-10.000     → objetivo_base = 55
>10.000          → objetivo_base = 55
```

**Ajuste por densidad conceptual:**

Calcular densidad = (conceptos ESENCIAL + TRONCAL) / (palabras totales / 1.000)

```
IF densidad > 0.7 → objetivo = objetivo_base × 1.15 (redondear)
IF densidad < 0.3 → objetivo = objetivo_base × 0.85 (redondear)
ELSE              → objetivo = objetivo_base (sin modificar)
```

Límites finales: mínimo 40, máximo 55. Si el ajuste excede estos límites → truncar al límite.

**Esta fórmula es mecánica y obligatoria. No existen excepciones por "calidad", "rigor", "preferencia" ni ningún otro criterio ad hoc. Calcular, aplicar, documentar. El número resultante es el objetivo final.**

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
- **Prohibido padding:** No añadir cadenas de adjetivos, adverbios, sinónimos o frases introductorias genéricas que no aporten información evaluable. Cada palabra del stem debe justificar su presencia. Si una oración del stem puede eliminarse sin impedir la respuesta → eliminarla.

### Answer
La mejor opción entre las presentadas (no perfecta, sino la más correcta). Sin pistas por longitud, tecnicismo o repetición de palabras del stem.

### Distractores

**Regla de diferencia graduada:** Cada distractor comparte estructura, terminología y extensión con la respuesta correcta, diferenciándose en un número controlado de elementos conceptuales.

**Definición de "diferencia conceptual":** Una diferencia conceptual es el cambio de exactamente uno de estos cinco elementos: mecanismo, temporalidad, agente, condición o contexto. Un cambio que modifica dos o más de estos elementos simultáneamente cuenta como dos o más diferencias, no como una.

Ejemplo CORRECTO (1 diferencia):
- Correcta: "La serotonina actúa sobre receptores 5-HT2A"
- Distractor espejo: "La serotonina actúa sobre receptores 5-HT1A" → cambia solo agente (receptor)

Ejemplo INCORRECTO (parece 1 pero son 2):
- Correcta: "La serotonina actúa sobre receptores 5-HT2A en corteza prefrontal"
- Distractor: "La dopamina actúa sobre receptores D2 en corteza prefrontal" → cambia agente (neurotransmisor) Y agente (receptor) = 2 diferencias

Grado de diferencia según dificultad del ítem:
- Fácil: 2 diferencias conceptuales respecto a la correcta
- Medio: 1-2 diferencias conceptuales
- Difícil: exactamente 1 diferencia conceptual

Verificación para cada distractor:
1. Alinear distractor con respuesta correcta
2. Identificar cuáles de los 5 elementos cambian (mecanismo, temporalidad, agente, condición, contexto)
3. Contar cambios. Si divergen en más elementos de lo permitido por su dificultad → reescribir
4. Si divergen en 0 elementos → reescribir (ambiguo)

**Tipos de distractor por dificultad:**

FÁCIL y MEDIO:
- **Plausible:** parece correcto si no se domina el concepto; usa terminología del campo correctamente pero la aplica al concepto equivocado
- **Casi-correcto:** correcto en otro contexto del documento, requiere discriminar en qué contexto aplica cada afirmación

DIFÍCIL — TRAMPA:
- **Espejo:** casi idéntico a la respuesta correcta pero con un detalle crítico cambiado; construido modificando exactamente 1 elemento de la respuesta correcta
- **Inversión lógica:** invierte una relación del documento (causa↔efecto, antecedente↔consecuente, condición↔resultado, general↔específico)
- **Sobreinclusión:** toma la respuesta correcta y añade un elemento extra que la invalida; parece más completa pero es incorrecta por el añadido

**Verificación obligatoria de tipo Difícil-Trampa:** Cada distractor etiquetado como Espejo, Inversión o Sobreinclusión debe pasar su test específico. Si no lo pasa → reclasificar como Plausible/Casi-correcto y cambiar la dificultad del ítem a Medio, o reconstruir el distractor hasta que pase el test.

| Tipo | Test de verificación | PASA si... | FALLA si... |
|---|---|---|---|
| Espejo | Copiar la respuesta correcta, sustituir exactamente 1 elemento, comparar con el distractor | El distractor resultante es equivalente al construido; comparten ≥80% de las palabras | El distractor cambia la estructura completa, usa vocabulario diferente, o modifica >1 elemento |
| Inversión | Identificar la relación A→B en la correcta; verificar que el distractor presenta B→A o invierte la dirección | Existe una relación explícita invertida entre correcta y distractor | El distractor presenta una alternativa del mismo campo sin relación de inversión |
| Sobreinclusión | Copiar la respuesta correcta, verificar que el distractor la contiene íntegramente + un añadido | El distractor incluye literalmente la correcta más un elemento extra invalidante | El distractor es una alternativa diferente que no contiene la correcta como subconjunto |

DIFÍCIL — MICROCONCEPTO DÉBIL:
- Distractores plausible o casi-correcto, pero el contenido evaluado es una excepción, matiz o condición límite del concepto. La dificultad viene del contenido, no del tipo de distractor.

**Distractor mal construido (señales de alerta):**
- Obviamente incorrecto o sin relación con el stem
- Longitud muy distinta a las otras opciones
- Errores gramaticales
- Contiene palabras prohibidas de la lista anti-sesgos o sus equivalentes semánticos

**Si es imposible generar distractores con la regla de diferencia graduada → marcar INCOMPLETO.**

### Cita textual
Cada pregunta requiere un fragmento literal del documento (≥3 palabras consecutivas de una sola ubicación) que soporte la respuesta correcta. **Prohibido:** combinar fragmentos de distintas secciones, usar puntos suspensivos (`...`) para unir fragmentos no contiguos, o parafrasear. Si el soporte proviene de dos ubicaciones distintas → usar solo la cita más directa.

### Nota explicativa
Formato: `'cita textual'; No son las otras porque [razón 1] y [razón 2]`
- Cita entre comillas simples
- Separador `;` después de cita
- **Límite duro: máximo 50 palabras.** Si la nota excede 50 palabras → eliminar la razón del distractor menos informativa. Si aún excede → comprimir ambas razones a palabras clave.
- Solo texto, `:` y `;` (prohibido `|`, `"`, `...`, saltos de línea)

### Nivel Bloom
- **Recordar:** identificar, definir, reconocer, listar
- **Comprender:** explicar, interpretar, comparar, diferenciar, relacionar
- **Aplicar:** resolver caso, decidir en contexto, implementar, ejecutar

---

## VALIDACIÓN DURANTE GENERACIÓN

Verificar cada ítem contra TODAS estas reglas. Un fallo en cualquiera → corregir antes de incluir en el banco.

1. Cita ≥3 palabras consecutivas del documento, de una sola ubicación, sin `...`
2. Longitud de opciones dentro de ±10% (contar caracteres de las 3 opciones; la más larga no supera en >10% a la más corta)
3. Cero palabras prohibidas (lista explícita + principio general) en stem y opciones
4. Longitud del stem dentro del límite (Recordar/Comprender: ≤40 palabras; Aplicar: ≤60 palabras)
5. Answer byte-a-byte idéntico a una de las opciones
6. Sin duplicados (mismo concepto + mismo enfoque → reescribir uno)
7. Pistas eliminadas (revisar lista completa de pistas)
8. Nota ≤50 palabras, sin `...`
9. Sin padding en stem (test: ¿cada oración es necesaria para responder?)
10. **Coherencia de clasificación:** Ningún ítem basado en concepto DESCARTABLE.
11. **Integridad de columnas:** Bloom solo acepta `Recordar`/`Comprender`/`Aplicar`; Dificultad solo acepta `Fácil`/`Medio`/`Difícil-Trampa`/`Difícil-Micro`; Tipo distractor solo acepta `Plausible`/`Casi-correcto`/`Espejo`/`Inversión`/`Sobreinclusión`.
12. **Coherencia tipo-distractor en Difícil-Trampa:** Cada ítem etiquetado Difícil-Trampa debe pasar el test de verificación de su tipo declarado (tabla de tests). Si falla → reconstruir distractor o reclasificar a Medio.

---

## BLUEPRINT

**Dificultad ≠ Bloom.** Cruce libre entre ambas dimensiones.

| Dificultad | % del banco | Subtipo | Tipo de distractor | Bloom permitido |
|---|---|---|---|---|
| Fácil | 40% | — | Plausible, casi-correcto | Cualquiera |
| Medio | 40% | — | Plausible, casi-correcto | Cualquiera |
| Difícil | 10% | Trampa | Espejo, inversión, sobreinclusión | Cualquiera |
| Difícil | 10% | Microconcepto débil | Plausible o casi-correcto; dificultad por contenido ultra-específico | Cualquiera |

La diferencia entre Fácil y Medio: el stem fácil es directo y corto (distractores con 2 diferencias); el stem medio es contextual con caso, datos o narrativa (distractores con 1-2 diferencias). Los ítems Difícil-Trampa usan distractores con exactamente 1 diferencia que pasan su test de tipo. Los ítems Difícil-Microconcepto evalúan excepciones, matices y condiciones límite.

**Tabla blueprint de salida (una fila por ítem):**

| Nº | Tema | Subtema / Microconcepto | Dificultad | Bloom | Tipo de distractor |
|---|---|---|---|---|---|
| 1 | [tema] | [microconcepto] | Fácil / Medio / Difícil-Trampa / Difícil-Micro | Recordar / Comprender / Aplicar | Plausible / Casi-correcto / Espejo / Inversión / Sobreinclusión |

**Resúmenes al final del blueprint:**
```
Bloom: Recordar [N, %]; Comprender [N, %]; Aplicar [N, %]; target [40/40/20]; desviación [deltas]
Dificultad: Fácil [N, %]; Medio [N, %]; Difícil-Trampa [N, %]; Difícil-Micro [N, %]; target [40/40/10/10]; desviación [deltas]
```

---

## FORMATO DE SALIDA

### Tabla Markdown

**Cabecera exacta:**
```
| Nº | Pregunta | Opción 1 | Opción 2 | Opción 3 | Answer | Nota | Bloom | Dificultad |
```

**Columnas:**
- **Nº:** secuencial
- **Pregunta:** enunciado completo
- **Opción 1, 2, 3:** las tres opciones
- **Answer:** byte-a-byte idéntico a la opción correcta, sin prefijo
- **Nota:** según formato definido en sección Generación (cita + razones; ≤50 palabras; sin `...`)
- **Bloom:** Recordar | Comprender | Aplicar
- **Dificultad:** Fácil | Medio | Difícil-Trampa | Difícil-Micro

---

### Informe Técnico (máx 350 palabras)

Formato: párrafo continuo separado con `;`

**Contenido obligatorio:**

**Documento:** palabras totales [N]; densidad conceptual [valor]; tipo [Teórico/Clínico/Mixto]

**Extracción:** ESENCIAL [N conceptos: nombres]; TRONCAL [N: nombres]; SUBIDEA [N total, N seleccionados]; MENOR [N]; DESCARTABLE [N]

**Objetivo:** base [N]; densidad [valor]; condición cumplida [densidad >0.7 / densidad <0.3 / rango 0.3-0.7]; ajuste [×1.15 / ×0.85 / ninguno]; resultado [N]; truncado a límite [sí: cuál / no]; **objetivo final [N]**

**Generadas:** total [N]; por nivel: ESENCIAL [N, %]; TRONCAL [N, %]; SUBIDEA [N, %]; MENOR [N, %]

**Bloom real:** Recordar [N, %]; Comprender [N, %]; Aplicar [N, %]; desviación vs target [deltas]

**Dificultad real:** Fácil [N, %]; Medio [N, %]; Difícil-Trampa [N, %]; Difícil-Micro [N, %]

**Cobertura ESENCIAL (listar TODOS los conceptos):** [Concepto1: N preguntas (R:X, C:X, A:X)]; [Concepto2: N preguntas (R:X, C:X, A:X)]; ... [ConceptoN: N preguntas]; cobertura [% conceptos con ≥2 preguntas]

**NO_CUBIERTOS:** [N conceptos: razones resumidas]

**Incidencias:** INCOMPLETAS [N: razones]; palabras prohibidas corregidas [N]; equivalentes semánticos corregidos [N]; stems recortados por exceso [N]; opciones ajustadas por disparidad [N]; notas truncadas [N]; distractores recombinados [N]; duplicados eliminados [N]; contaminación de columnas [N]; ítems sobre DESCARTABLE eliminados [N]; citas corregidas por mosaico o `...` [N]; **distractores Difícil-Trampa reclasificados por fallo de test de tipo [N: cuáles]**

**Alertas:** [ESENCIAL con <2 preguntas: cuáles]; [Bloom fuera de tolerancia: deltas]; [cuota MENOR utilizada: N]

**Feedback adaptativo (evaluar TODOS, documentar TODOS):**
```
FA1 — INCOMPLETO excesivos: [resultado: N INCOMPLETO / 0 INCOMPLETO; umbral 25%; acción tomada / dentro de umbral]
FA2 — Cuota ESENCIAL: [resultado: alcanzada / inalcanzable; acción tomada / cuota alcanzada]
FA3 — Bloom tolerancia: [resultado: desviación [deltas]; dentro de ±5% / se aplicó ±10% porque [razón]]
FA4 — Distractores imposibles: [resultado: N afectados / 0 afectados; umbral 15%; acción tomada / dentro de umbral]
FA5 — Objetivo alcanzable: [resultado: alcanzado [N] / reducido a [N] porque [razón]]
```

---

## FEEDBACK ADAPTATIVO

**Obligatorio:** Evaluar TODOS los umbrales tras completar la generación. Documentar el resultado de CADA check en el bloque FA del informe técnico, usando el formato exacto FA1-FA5 especificado arriba. No omitir ninguno. Si no se activa la corrección → escribir "dentro de umbral".

| Señal | Umbral | Corrección | Documentar |
|---|---|---|---|
| Ítems INCOMPLETO excesivos | >25% del banco | Bajar nivel Bloom (Aplicar→Comprender→Recordar); si persiste: "documento insuficiente" | FA1 |
| Cuota ESENCIAL inalcanzable | Menos conceptos ESENCIAL que preguntas asignadas | Reclasificar los 3 TRONCAL más frecuentes como ESENCIAL; recalcular | FA2 |
| Bloom fuera de tolerancia | Desviación >±5% en cualquier nivel | IF contenido homogéneo → permitir hasta ±10% con justificación; ELSE → rebalancear ítems | FA3 |
| Distractores imposibles | >15% ítems sin distractor válido con regla de diferencia graduada | Permitir 1 diferencia adicional para esos ítems | FA4 |
| Objetivo inalcanzable | Documento demasiado corto para 40 preguntas de calidad | Reducir objetivo a 30; documentar | FA5 |

---

## COMPLETITUD Y ORDEN DE SALIDA

**Regla de completitud obligatoria:** El banco debe entregarse completo. Todas las preguntas generadas deben aparecer en la tabla, sin truncar, omitir ni resumir ninguna fila.

**Si la salida excede la capacidad de respuesta:**
1. Dividir en bloques numerados
2. Al final de cada bloque escribir: `[CONTINÚA EN BLOQUE N+1 — faltan X preguntas]`
3. Esperar confirmación del usuario antes de continuar
4. Prohibido: truncar filas silenciosamente, usar "..." o "[Q4-Q28 omitidas]", entregar banco parcial sin aviso

**Orden de entrega obligatorio:**
1. **Blueprint** (una fila por ítem, con resúmenes) — SE ENTREGA PRIMERO
2. **Tabla Markdown** completa (o Bloque 1 si necesita división)
3. **Informe Técnico** (con bloque FA1-FA5 obligatorio)
4. **Checklist de compliance** (ver sección siguiente)

No mostrar razonamiento interno ni pasos intermedios de extracción.

---

## CHECKLIST DE COMPLIANCE

**Obligatoria.** Producir esta tabla como último elemento de la salida. Rellenar cada fila con PASA o FALLA + detalle. Si cualquier fila es FALLA, la incidencia ya debe estar corregida en el banco entregado y documentada en el informe.

```
| Check | Criterio | Resultado |
|---|---|---|
| C1 | Blueprint presente y completo (1 fila por ítem, resúmenes incluidos) | PASA / FALLA: [detalle] |
| C2 | Tabla Markdown completa (N ítems = objetivo final calculado por fórmula) | PASA / FALLA: [N entregados vs N objetivo] |
| C3 | Objetivo = resultado de fórmula (sin excepciones ad hoc) | PASA / FALLA: [cálculo: base × ajuste = resultado] |
| C4 | Stems dentro de límite de palabras (Rec/Comp ≤40; Apl ≤60) | PASA / FALLA: [Nº corregidos] |
| C5 | Opciones dentro de ±10% longitud entre sí (por ítem) | PASA / FALLA: [Nº corregidos] |
| C6 | Notas ≤50 palabras cada una, sin puntos suspensivos | PASA / FALLA: [Nº corregidas] |
| C7 | Cero palabras prohibidas (lista + principio general + adjetivos valorizadores) | PASA / FALLA: [Nº corregidas] |
| C8 | Integridad de columnas (Bloom, Dificultad, Tipo distractor: solo valores válidos) | PASA / FALLA: [Nº corregidas] |
| C9 | Coherencia DESCARTABLE (0 ítems basados en conceptos DESCARTABLE) | PASA / FALLA: [detalle] |
| C10 | Bloom dentro de tolerancia (±5%, o ±10% justificado en FA3) | PASA / FALLA: [distribución y delta] |
| C11 | Citas literales continuas (0 mosaicos, 0 puntos suspensivos, 0 paráfrasis) | PASA / FALLA: [Nº corregidas] |
| C12 | Answer byte-a-byte idéntico a una opción (por ítem) | PASA / FALLA: [Nº corregidos] |
| C13 | Difícil-Trampa: todos pasan test de tipo (Espejo/Inversión/Sobreinclusión) | PASA / FALLA: [Nº reclasificados] |
| C14 | Feedback adaptativo: bloque FA1-FA5 completo en informe | PASA / FALLA: [cuáles faltan] |
| C15 | Cobertura ESENCIAL: TODOS los conceptos listados en informe | PASA / FALLA: [N listados vs N totales] |
```

---

**FIN DEL PROMPT**