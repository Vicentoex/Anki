#Anki 
TAREA: Generar preguntas de COMPARACIÓN (contrastar dos ideas, teorías o fenómenos) a partir de UN SOLO documento. Mantener FIDELIDAD ABSOLUTA al texto fuente: no inventar, añadir ni omitir información. Entregar todo en español (registro formal).

ENTRADA: Documento principal (archivo o texto). Parámetros opcionales:
- N_ITEMS: número de preguntas solicitadas (por defecto 30).
- OMITIR_RESUMEN: si se indica, se omite el resumen previo y la IA genera las preguntas directamente.
- AUTORIZO EJECUCIÓN COMPLETA: si se incluye, la IA aplicará automáticamente correcciones propuestas.
- QUIERO REVISIÓN MANUAL: si se incluye, la IA sólo propondrá cambios sin aplicarlos.

SALIDA FINAL (obligatoria en el chat): tabla con columnas EXACTAS:
Nº | Pregunta | Opción 1 | Opción 2 | Opción 3 | Answer | Notas

FLUJO (ejecutar en este orden)

A — EXTRACCIÓN Y LECTURAS OBLIGATORIAS
1. Lectura estructural: índice sintético (secciones/títulos).
2. Lectura analítica: extraer conceptos fundacionales, pares conceptuales potenciales para comparar (cada elemento con cita textual exacta).
3. Lectura crítica: matices, limitaciones y contradicciones entre perspectivas.

B — RESUMEN_PREVIO Y MAPEOS (salvo OMITIR_RESUMEN)
4. Matriz extracción: lista de conceptos/perspectivas → cita textual.
5. Seleccionar candidatos a comparación: identificar pares donde ambas ideas estén presentadas en el texto o donde el texto permita contrastarlas. Priorizar pares con evidencia directa.
6. Selección 20% esencial: 3–6 temas principales y 5 mapeos ejemplo (Formato ejemplo: Nº | Pregunta sugerida (hard) | Cita A | Cita B | Nivel Bloom sugerido).
7. Usuario autoriza generación tras revisar el RESUMEN_PREVIO o indica OMITIR_RESUMEN.

C — REGLAS PARA FORMULAR PREGUNTAS DE COMPARACIÓN
8. Estructura de la pregunta (Hard):
   - Enunciar brevemente las dos entidades a contrastar y solicitar la diferencia/contraste concreta sin contexto extra. Ejemplo: "Comparado con X, ¿qué caracteriza a Y?".
   - Evitar pistas contextuales y nombres de página.
9. Opciones:
   - Tres alternativas por pregunta; mismas longitud y estructura (±10% caracteres).
   - Alternativas breves, paralelas sintácticamente, y plausibles.
   - Una única opción correcta que refleje la diferencia clave explicitada en el documento.
10. Criterio de selección de pares:
    - Ambas ideas deben estar respaldadas por citas textuales separadas.
    - Evitar contrastar conceptos que el documento no relacione explícitamente.
    - Preferir contrastes que impliquen diferencias de función, alcance, mecanismo, evidencia empírica o limitación.

D — FIDELIDAD Y CITAS (obligatorio)
11. FIDELIDAD ABSOLUTA:
    - No añadir información no textual.
    - No inferir hechos nuevos ni completar datos ausentes.
    - Si la comparación requiere interpretación, incluir únicamente lo que el texto permita y citar ambos fragmentos que sustentan la interpretación.
12. Notas (formato plano y trazable; dos líneas):
    - Línea 1 — Citas: fragmento A ; fragmento B   (cada fragmento tal cual en el documento, separados por " ; ")
    - Línea 2 — Justificación: breve explicación (≤22 palabras) que conecte las citas con la opción correcta
    Ejemplo:
    Citas: texto exacto A ; texto exacto B
    Justificación: explica brevemente por qué la diferencia señalada es correcta

E — CONTROL AUTOMÁTICO Y QA
13. Detección de duplicados/solapamientos:
    - Calcular similitud semántica de preguntas y de targets (embeddings/Tf-idf). coseno >0.70 → marcar DUPLICADO.
    - Entidades compartidas: intersección/menor_conjunto >50% → marcar SOLAPAMIENTO_CRÍTICO.
14. Verificaciones de opciones:
    - Longitud ±10% caracteres.
    - Misma categoría gramatical y paralelismo sintáctico.
15. Distribución Bloom (adaptada a comparación):
    - Por defecto: Comprender 60% ±5 ; Recordar 30% ±5 ; Analizar/Aplicar 10% ±3.
    - Mostrar distribución propuesta en RESUMEN_PREVIO.
16. Detección de pistas:
    - Heurística de señales formales (longitud, stopwords, mayúsculas, formato); marcar ítems con señales > umbral.

F — DISTRACTORES (criterios de alta dificultad)
17. Reglas para distractores altamente plausibles:
    - Cada distractor debe corresponder a una interpretación posible EXTRAÍDA DEL DOCUMENTO (no información externa).
    - Similitud semántica distractor-correcta objetivo: 0.45–0.75.
    - Al menos un distractor será una inversión parcial (mezcla de rasgos de A y B); otro será una exageración o minimización de rasgo real.
    - Indicar en Notas: plausibilidad(sim:X) ; ocurrencias_in_texto:Y.

G — PROPUESTAS Y FALLBACKS
18. Si se detecta DUPLICADO o SOLAPAMIENTO_CRÍTICO: proponer REESCRIBIR o MATIZAR; marcar prioridad (Alta/Media/Baja).
19. FALLBACK por defecto: marcar pares como 'PAREADO' y proponer alternativa; no eliminar automáticamente sin 'AUTORIZO EJECUCIÓN COMPLETA'.

H — REFINAMIENTO (si AUTORIZADO)
20. Aplicar reescrituras autorizadas, homogeneizar opciones (±10%), generar diff (N° | Original → Reescrito | Motivo).
21. Antes de aplicar cambios de Alta prioridad exigir 'APROBACIÓN REQUERIDA'.

I — VERIFICACIÓN FINAL Y ENTREGA
22. Recalcular similitud y solapamiento; condiciones de éxito:
    - No queden pares con coseno >0.70 salvo los marcados y justificados.
    - No queden pares con solapamiento de entidades >50% (salvo justificados).
    - Opciones y Bloom dentro de tolerancias.
23. Entregar:
    - Tabla final en el chat (Nº|Pregunta|Opción1|Opción2|Opción3|Answer|Notas).
    - Informe técnico breve: duplicados/solapamientos, distribución Bloom real, % opciones fuera de ±10%.
    - Diff de reescrituras (ítems cambiados).

J — VERSIONADO, METADATOS Y LÍMITES
24. Incluir metadatos al inicio: Fecha, Versión, Hash_banco, Documento_principal: <nombre_archivo>.
25. No realizar búsquedas externas.
26. Si un contraste implica datos numéricos en tablas/figuras, usar la leyenda textual o marcar como 'FIGURA_NO_EXTRAÍBLE' si no se puede extraer exactamente.

AUTORIZACIONES:
- Escribir AUTORIZO EJECUCIÓN COMPLETA para aplicar cambios automáticamente.
- Escribir QUIERO REVISIÓN MANUAL para recibir propuestas sin aplicar.
- Escribir OMITIR_RESUMEN para saltar el resumen previo.
