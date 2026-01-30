#Anki 
TAREA: Generar PREGUNTAS DE ANÁLISIS DE CASO (escenario + interpretación/solución) a partir de UN SOLO documento. Mantener FIDELIDAD ABSOLUTA al texto fuente: no inventar, añadir ni omitir información. Entregar todo en español (registro formal).

ENTRADA: Documento principal (archivo o texto). Parámetros opcionales:
- N_ITEMS: número de preguntas solicitadas (por defecto 20).
- OMITIR_RESUMEN: si se indica, se omite el resumen previo y la IA genera las preguntas directamente.
- AUTORIZO EJECUCIÓN COMPLETA: si se incluye, la IA aplicará automáticamente correcciones propuestas.
- QUIERO REVISIÓN MANUAL: si se incluye, la IA sólo propondrá cambios sin aplicarlos.

SALIDA FINAL (obligatoria en el chat): tabla con columnas EXACTAS:
Nº | Pregunta (escenario + pregunta Hard) | Opción 1 | Opción 2 | Opción 3 | Answer | Notas

FLUJO (ejecutar en este orden)

A — EXTRACCIÓN Y LECTURAS OBLIGATORIAS
1. Lectura estructural: índice sintético (secciones/títulos) y localización de apartados con casos, ejemplos o datos aplicables.
2. Lectura analítica: extraer conceptos fundacionales, evidencias experimentales, variables, limitaciones metodológicas y resultados relevantes; cada elemento con su cita textual exacta.
3. Lectura crítica: identificar matices, supuestos del texto, contradicciones y huecos que afectan interpretación o aplicación.

B — RESUMEN_PREVIO Y MAPEOS (salvo OMITIR_RESUMEN)
4. Matriz de extracción: lista de elementos (concepto/variable/resultado) → cita textual.
5. Identificar situaciones del texto susceptibles de convertirse en caso (hechos/textos que constituyan un escenario operativo) y listar hasta 8 candidatas.
6. Seleccionar el "20% esencial" (3–6 ítems) que sustentan la mayoría de los casos propuestos.
7. Proponer 5 mapeos ejemplo (Formato: Nº ejemplo | Escenario (resumen, 1–2 frases) | Pregunta sugerida Hard | Citas que sustentan el escenario | Nivel Bloom sugerido).
8. El usuario revisa y autoriza generación (o indica OMITIR_RESUMEN para generar sin revisión previa).

C — DISEÑO DE CASOS (reglas para construir el escenario)
9. Origen del escenario:
   - El escenario debe crearse **solo** con hechos y frases que aparecen en el documento (puede combinar varias citas contiguas o próximas para construir contexto), sin añadir contexto externo.
   - Si el documento no presenta hechos suficientes para un escenario coherente, marcar como INCOMPLETO y no generar el caso.
10. Longitud y claridad:
   - Escenario: 2–6 oraciones; contener solo la información necesaria para plantear la pregunta.
   - La pregunta Hard (al final del escenario) debe formularse en una sola línea y pedir una interpretación, diagnóstico, causa probable, priorización de acción o solución concreta basada en el texto.
11. Autonomía del escenario:
   - El escenario debe ser autosuficiente: el lector con conocimiento habitual de la asignatura (pero sin acceso al documento) debe poder intentar responder basándose en el escenario y las opciones, aunque la justificación correcta estará sólidamente respaldada por las citas.
12. Transparencia de fuentes:
   - Indicar en Notas las citas textuales exactas que se usaron para construir el escenario (ver apartado Notas).

D — OPCIONES Y ESTRUCTURA DE LA PREGUNTA
13. Formato:
   - Una pregunta por caso.
   - Tres alternativas por pregunta; mismas longitud y estructura (±10% caracteres).
   - Alternativas cortas, paralelas y plausibles.
14. Tipos de resolución permitidos:
   - Diagnóstico de causa, identificación de variable clave, recomendación prioritaria, interpretación de resultados, evaluación de validez interna/externa, propuesta de mejora metodológica.
15. Restricciones de fidelidad:
   - La respuesta correcta debe derivarse **exclusivamente** de las citas indicadas en Notas; no se pueden usar datos que no estén presentes en las citas.

E — FIDELIDAD, CITAS Y NOTAS (obligatorio)
16. FIDELIDAD ABSOLUTA:
    - No introducir información que no aparezca explícitamente en el documento.
    - No inferir hechos nuevos ni completar datos ausentes.
    - Si se requiere interpretación, presentar solo lo que las citas permiten inferir razonablemente; si hay ambigüedad, marcar 'AMBIGUO'.
17. Notas (formato plano y trazable; dos líneas):
    - Línea 1 — Citas: lista de citas textuales usadas (separadas por " ; "), sin páginas.
    - Línea 2 — Justificación: explicación breve (≤25 palabras) que conecte las citas con la opción correcta.
    Ejemplo:
    Citas: texto exacto A ; texto exacto B
    Justificación: explicación breve que liga evidencia→respuesta

F — CONTROL AUTOMÁTICO Y QA
18. Detección de duplicados/solapamientos:
    - Calcular similitud semántica entre preguntas y entre escenarios (embeddings/Tf-idf). Umbral duplicado: coseno >0.70 → marcar DUPLICADO.
    - Solapamiento entidades: intersección/menor_conjunto >50% → marcar SOLAPAMIENTO_CRÍTICO.
19. Verificaciones de opciones:
    - Longitud ±10% caracteres; paralelismo sintáctico.
    - Todas las opciones deben ser compatibles con el escenario (no introducir opciones absurdas).
20. Distribución Bloom (adaptado a casos):
    - Por defecto: Aplicar 50% ±5 ; Analizar 40% ±5 ; Evaluar 10% ±3.
    - Mostrar distribución propuesta en RESUMEN_PREVIO.
21. Detección de pistas:
    - Heurística para detectar pistas formales (palabras idénticas en escenario y opción, puntuación, mayúsculas, posición). Marcar ítems sospechosos.

G — DISTRACTORES (criterios para alta dificultad)
22. Reglas:
    - Cada distractor debe corresponder a una interpretación plausiblemente derivable del escenario o a una conclusión que sería comprensible pero incorrecta según las citas.
    - Similitud semántica distractor-correcta objetivo: 0.45–0.75.
    - Al menos uno de los distractores debe ser una respuesta "plausible pero incompleta" (parte correcta y parte errónea); otro debe ser una "conclusión alternativa" basada en el texto pero refutada por una cita.
    - Indicar en Notas: plausibilidad(sim:X) ; ocurrencias_in_texto:Y si procede.

H — PROPUESTAS Y FALLBACKS
23. Si se detecta DUPLICADO o SOLAPAMIENTO_CRÍTICO: proponer REESCRIBIR el escenario/pregunta o MATIZAR la opción; marcar prioridad (Alta/Media/Baja).
24. FALLBACK por defecto: marcar pares/escenarios como 'PAREADO' y proponer alternativas; no eliminar automáticamente sin 'AUTORIZO EJECUCIÓN COMPLETA'.

I — REFINAMIENTO (si AUTORIZADO)
25. Aplicar reescrituras autorizadas, homogeneizar opciones (±10%), generar diff (N° | Original → Reescrito | Motivo).
26. Cambios de Alta prioridad (p. ej. eliminar escenario por falta de evidencia) requieren etiqueta 'APROBACIÓN REQUERIDA'.

J — VERIFICACIÓN FINAL Y ENTREGA
27. Recalcular similitud y solapamiento; condiciones de éxito:
    - No queden pares con coseno >0.70 salvo los marcados y justificados.
    - No queden pares con solapamiento de entidades >50% (salvo justificados).
    - Bloom y longitudes dentro de tolerancias.
28. Entregar:
    - Tabla final en el chat (Nº|Pregunta (escenario+pregunta Hard)|Opción1|Opción2|Opción3|Answer|Notas).
    - Informe técnico breve: duplicados/solapamientos, distribución Bloom real, % opciones fuera de ±10%, ítems marcados AMBIGUO/PAREADO.
    - Diff de reescrituras (ítems cambiados).

K — VERSIONADO, METADATOS Y LÍMITES
29. Incluir metadatos al inicio: Fecha, Versión, Hash_banco, Documento_principal: <nombre_archivo>.
30. No realizar búsquedas externas.
31. Si el caso requiere datos numéricos de una tabla/figura, usar la leyenda textual o marcar 'FIGURA_NO_EXTRAÍBLE' si no es posible extraer con seguridad.

L — AUTORIZACIONES Y MODOS
32. AUTORIZO EJECUCIÓN COMPLETA → aplicar cambios automáticamente.
33. QUIERO REVISIÓN MANUAL → solo propuestas.
34. OMITIR_RESUMEN → generar casos/preguntas sin resumen previo.

REGLAS FINALES (obligatorias)
- Idioma: español (registro formal).
- Fidelidad absoluta: no se inventa información ni se omite evidencia explícita.
- Notas: formato plano (Citas ; Justificación).
- Si hay ambigüedad o imposibilidad de construir un escenario fiable, marcar INCOMPLETO y listar fragmentos problemáticos.
