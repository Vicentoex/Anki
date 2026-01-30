#Anki 
TAREA: Generar tarjetas tipo CLOZE (texto con vacío) a partir de UN SOLO documento. Mantener FIDELIDAD ABSOLUTA al texto fuente: no inventar, añadir ni omitir información. Entregar todo en español (registro formal).

ENTRADA: Documento principal (archivo o texto). Parámetros opcionales que el usuario puede incluir junto al prompt:
- N_ITEMS: número de tarjetas solicitadas (por defecto 30).
- OMITIR_RESUMEN: si se indica, se omite el resumen previo y la IA genera las tarjetas directamente.
- AUTORIZO EJECUCIÓN COMPLETA: si se incluye, la IA aplicará automáticamente correcciones propuestas.
- QUIERO REVISIÓN MANUAL: si se incluye, la IA solo propondrá cambios sin aplicarlos.

SALIDA FINAL (obligatoria): Tabla en el chat con columnas EXACTAS:
Question | Answer

Formato CLAUSULA ANKI (para la columna Question): use la sintaxis cloze de Anki en el texto, p.e.:
La capital de Francia es {{c1::París}}.
En la columna Answer el texto debe contener exactamente la cadena oculta (sin llaves), p.e.
París

FLUJO (ejecutar en este orden)

A — ANÁLISIS PREVIO (obligatorio salvo OMITIR_RESUMEN)
1. Lecturas:
   a) Lectura estructural: índice sintético (secciones/títulos).
   b) Lectura analítica: extraer conceptos fundacionales, datos clave, entidades nombradas y elementos experimentales; cada elemento acompañado de la cita textual exacta.
   c) Lectura crítica: matices, limitaciones y huecos.
2. RESUMEN_PREVIO: entregar la matriz de extracción (concepto → cita textual) y la selección del "20% esencial" (3–6 ítems) con justificación breve. Proponer hasta 5 mapeos ejemplo (Pregunta sugerida Hard en formato cloze ← Cita textual que la respalda ← Nivel: Recordar/Comprender).

B — SELECCIÓN DE TARGETS (reglas para elegir la parte oculta)
3. Bases para seleccionar la porción a ocultar (target):
   - El texto escondido debe ser **exacto** al documento; el Answer será exactamente la cadena tal cual aparece en la fuente.
   - Preferir: términos clave, definiciones textuales, fechas, fórmulas, nombres propios, resultados experimentales, frases lapidarias.
   - Evitar: stopwords aisladas, artículos o partículas sin carga informativa.
   - Longitud objetivo del target: entre 1 y 6 palabras. Para fórmulas o secuencias numéricas puede aceptarse mayor longitud si es estrictamente necesaria.
   - El contexto de la oración (lo que permanece visible) debe ser autosuficiente para situar el hueco (no cortar sentido).
4. Reglas de fidelidad:
   - No parafrasear la cadena oculta: Answer = cita exacta.
   - Si la cita contiene caracteres problemáticos (comillas, saltos), ajustar solo la selección para que sea un fragmento textual limpio; siempre que el fragmento siga apareciendo literalmente en el documento.
   - Si la información es ambigua en el documento, marcarla como AMBIGUO y no generar cloze sin autorización.

C — CONTROL AUTOMÁTICO Y QA (comprobaciones)
5. Detección de duplicados/solapamientos:
   - Calcular similitud semántica entre targets; si coseno > 0.70 marcar DUPLICADO. Proponer alternativa de target distinta o matización.
   - Evitar crear varios clozes cuya respuesta sea la misma cadena textual.
6. Restricciones de calidad:
   - No crear targets que aparezcan en el documento en contextos múltiples con significados distintos (ambigüedad de referencia) sin marcar.
   - Evitar targets triviales (p. ej. “de”, “la”).
7. Distribución de niveles (adaptado a cloze):
   - Por defecto: Recordar 80% ±5 ; Comprender 20% ±5.
   - La IA propondrá la distribución y la mostrará en el RESUMEN_PREVIO.
8. Heurística de dificultad:
   - Marcar como “demasiado fácil” si la cadena oculta aparece en el documento con alta frecuencia (>5 apariciones) o si el contexto contiene un identificador textual directo (ej. «París (la capital)»).
   - Reportar ítems sospechosos de pista (posición, formato, mayúsculas).

D — REGLAS DE SALIDA Y FORMATO
9. Cada ítem en la tabla debe cumplir:
   - Question: frase completa en la que la parte oculta se muestra con sintaxis cloze {{cN::texto}}. Use un único cloze por Question (no crear múltiples clozes en la misma Question).
   - Answer: texto exacto oculto (sin llaves).
10. Notas y trazabilidad:
   - Antes de generar la tabla final (salvo OMITIR_RESUMEN), la IA entregará el RESUMEN_PREVIO (matriz + 5 mapeos). El usuario podrá autorizar o pedir ajustes.
11. Versionado y cambios:
   - Si se aplican reescrituras automáticas, generar diff (Nº | Original cloze → Reescrito | Motivo). Si no se aplica, mostrar propuestas.
12. Entrega final: la tabla con N_ITEMS ítems y, aparte, un breve informe con:
   - %Recordar / %Comprender real
   - Ítems marcados DUPLICADO / AMBIGUO / demasiado fáciles
   - Recomendaciones breves (máx. 3 líneas)

E — AUTORIZACIONES Y MODO DE TRABAJO
13. Si el usuario escribe AUTORIZO EJECUCIÓN COMPLETA, la IA aplicará correcciones automáticas para cumplir umbrales (reescribir targets duplicados, homogeneizar), respetando la regla de fidelidad.
14. Si el usuario escribe QUIERO REVISIÓN MANUAL, la IA solo propondrá cambios y esperará instrucciones.

REGLAS FINALES (obligatorias)
- Idioma: español (registro formal).
- Fidelidad absoluta: Answer debe coincidir exactamente con el texto fuente; no se inventa información.
- Salida final solo en el chat: tabla Question | Answer, con las preguntas en formato cloze Anki.
- Si hay ambigüedad o imposibilidad de extraer un target fiable, marcar INCOMPLETO y listar fragmentos problemáticos en el informe.
