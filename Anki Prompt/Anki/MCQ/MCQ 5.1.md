#Anki 
- Ventajas de este prompt con respecto a [[Datos/Anki/MCQ/MCQ 5]]: 
	- ↑Nº de Preguntas y de conceptos cubiertos
	- ↑Flexibilidad en las preguntas
	- Igual en todo lo demás al [[Datos/Anki/MCQ/MCQ 5]] pero con matices
	- ↑ Velocidad con respecto a otros como antecesores como [[Datos/Anki/MCQ/MCQ 4]]  o predecesores como [[Datos/Anki/MCQ/MCQ 5.2]]
 >[!WARNING] Atención:
 >Solo sirve para 1 Documento
 >Para verficación admite [[Datos/Anki/DLC's/Verificación pro (5 y 5.1)]]
 >Se puede elegir automatismo total o parte manual humana 
 >
 >UTORIZACIÓN PARA EJECUCIÓN:  
 -Si desea que el sistema aplique automáticamente las correcciones propuestas, incluya al final del mensaje: AUTORIZO EJECUCIÓN COMPLETA
-Si prefiere revisar las propuestas antes de aplicar cualquier cambio, incluya: QUIERO REVISIÓN MANUAL
-Para omitir el resumen previo, incluya: OMITIR_RESUMEN



CABECERA Y PARÁMETROS (obligatorio)
- Idioma: Español (registro formal).
- Fidelidad: Fidelidad absoluta al texto fuente (no introducir información externa ni inventada).
- MaxPorConcepto: 4
- Objetivo_base: 50 preguntas (rango preferente ±20 → objetivo flexible 30–70)
- Mínimo absoluto aceptable: 30 preguntas
- Entrada: Documento principal (archivo o texto).
- Salida final obligatoria en el chat: tabla con columnas EXACTAS:
  Nº | Pregunta (Hard) | Opción 1 | Opción 2 | Opción 3 | Answer | Notas

AUTORIZACIONES (modo de trabajo)
- AUTORIZO EJECUCIÓN COMPLETA → aplicar correcciones propuestas automáticamente.
- QUIERO REVISIÓN MANUAL → entregar propuestas sin aplicar cambios.
- OMITIR_RESUMEN → saltar el resumen previo y generar directamente (no recomendado para cobertura).

FLUJO DE TRABAJO (ejecutar en este orden)

A — EXTRACCIÓN (tres lecturas obligatorias)
1. Lectura estructural: identificar títulos, secciones y jerarquía; entregar índice sintético de secciones.
2. Lectura analítica: extraer y listar —con cita textual— conceptos fundacionales, datos/estadísticas clave, entidades nombradas, afirmaciones contundentes, relaciones causa→efecto y elementos experimentales (objetivo, muestra, técnicas, conclusiones).
3. Lectura crítica: identificar matices, limitaciones, contradicciones y huecos del texto.

B — SÍNTESIS PREVIA Y MAPEOS (resumen previo)
4. Generar matriz de extracción: concepto → cita textual → breve nota de vínculo.
5. Identificar el "20% esencial" (3–6 ítems) que explica ~80% del temario, con justificación breve y citas.
6. Proponer 5 mapeos ejemplo pregunta ← cita (formato: Nº ejemplo | Pregunta sugerida Hard | Cita textual que la respalda | Nivel Bloom sugerido).
7. Si el usuario indica 'OMITIR_RESUMEN', saltar a la siguiente sección; en caso contrario, esperar autorización para continuar.

B.1 — ADICIÓN: ESTRATEGIA DE COBERTURA Y GENERACIÓN ITERATIVA (OBLIGATORIA, ejecutar tras el paso 7)
OBJETIVO_DE_GENERACIÓN
- Objetivo preferente: 50 preguntas (rango preferente 30–70).
- Mínimo absoluto: 15 preguntas.
- Prioridad absoluta: cubrir todos los conceptos extraídos (al menos 1 pregunta por concepto cuando sea posible).

Pase 0 — Preparación
- ListaConceptos := lista de conceptos únicos extraídos en B.
- N_conceptos := longitud(ListaConceptos).
- Si N_conceptos == 0 → marcar INCOMPLETO y terminar (no generar preguntas).

Pase 1 — Cobertura mínima
- Por cada concepto C en ListaConceptos:
  - Generar 1 pregunta Hard (preferente nivel Recordar) basada en la cita asociada a C.
  - Registrar metadatos: concepto=C, pase=1.

Pase 2 — Expansión enfocada
- Calcular objetivo_total_real:
  - Si usuario indica N_ITEMS explícito: objetivo := clamp(N_ITEMS, 15, ∞) y avisar si >70.
  - Si no lo indica: objetivo := 50 (preferente). Rango flexible 30–70; si documento extenso y hay suficientes conceptos viables, se podrá exceder 70 con nota justificativa.
- Q_restantes := objetivo_total_real - |Preguntas_P1|.
- Priorizar conceptos para añadir preguntas adicionales en este orden:
  1. Conceptos dentro del 20% esencial.
  2. Conceptos con mayor densidad de citas/evidencia.
  3. Conceptos asociados a experimentos o datos numéricos.
- Para cada concepto en orden de prioridad:
  - Añadir hasta MaxPorConcepto total (incluyendo la de P1).
  - Generar preguntas de distinta categoría Bloom (preferir Comprender y Aplicar en estas adiciones).
  - Registrar metadatos: concepto, pase=2, nivelBloom.
  - Reducir Q_restantes hasta 0 o agotar candidatos.
- Si Q_restantes > 0 tras agotar candidatos:
  - Generar preguntas adicionales basadas en:
    - Comparaciones entre conceptos relacionados.
    - Matices, implicaciones, limitaciones metodológicas.
    - Interpretaciones de resultados experimentales citados.
  - Cada nuevo ítem requiere cita textual en Notas; respetar MaxPorConcepto.

Pase 3 — Afinado antirepetición y calidad
- Ejecutar C — REVISIÓN AUTOMÁTICA sobre el conjunto (P1+P2).
- Para duplicados (coseno >0.70): proponer REESCRIBIR o marcar para eliminación según autorización.
- Asegurar no superar MaxPorConcepto; si algún concepto excede, priorizar reducción de preguntas menos relevantes.
- Ajustar Bloom global mediante reescrituras para cumplir distribución objetivo (si necesario).
- Ejecutar QA heurística (I) y marcar ítems demasiado fáciles o sospechosos de pistas.

Salida parcial:
- Matriz de cobertura: para cada concepto indicar Nº preguntas asignadas y pase(s).
- Listado NO_CUBIERTOS con razones (p. ej. cita ambigua, FIGURA_NO_EXTRAÍBLE).

C — REVISIÓN AUTOMÁTICA (criterios cuantificables)
8. Calcular similitud semántica entre todas las preguntas (embeddings/Tf-idf).
   - Umbral duplicado: coseno > 0.70 → marcar DUPLICADO.
9. Calcular solapamiento de entidades clave entre pares.
   - Umbral crítico: intersección/menor_conjunto > 50% → marcar SOLAPAMIENTO_CRÍTICO.
10. Comprobar uniformidad de longitud de opciones: tolerancia ±10% (número de caracteres).
11. Comprobar distribución Bloom objetivo (configurable):
   - Por defecto: Recordar 65% ±5 ; Comprender 30% ±5 ; Aplicar 5% ±2. (La estrategia iterativa puede ajustar la mezcla; mostrar distribución real en informe).
12. Control de posición de respuestas: no es relevante (rotación externa aceptada).

D — Reglas obligatorias de fidelidad y manejo de contenido
13. FIDELIDAD ABSOLUTA (operativa):
    - No introducir información que no aparezca explícitamente.
    - No inferir hechos nuevos ni completar datos ausentes.
    - Paráfrasis mínima en opciones (máx. 25% cambio léxico) solo si la cita es excesivamente larga; la Nota incluirá la cita textual completa.
    - Si afirmación ambigua, marcar 'AMBIGUO' y no generar pregunta sin autorización.
14. FIGURAS / TABLAS / ECUACIONES:
    - Si existe leyenda textual, usarla como cita.
    - Si no, describir la figura ≤20 palabras y referenciar el párrafo que la menciona.
    - Si no es extraíble, marcar 'FIGURA_NO_EXTRAÍBLE' y no generar pregunta derivada sin autorización.

E — Propuestas de corrección (si se detectan problemas)
15. Para cada DUPLICADO (coseno > 0.7): proponer eliminar o REESCRIBIR y ofrecer texto alternativo que mantenga fidelidad textual.
16. Para cada SOLAPAMIENTO_CRÍTICO: proponer matización (definición vs ejemplo) o ajuste de nivel Bloom; ofrecer texto alternativo.
17. Priorizar propuestas: Alta/Media/Baja.
18. FALLBACK por defecto: marcar pares como 'PAREADO' y proponer reescritura alternativa. No eliminar ítems automáticamente salvo instrucción 'AUTORIZO EJECUCIÓN COMPLETA'.

F — REFINAMIENTO (aplicar cambios si AUTORIZADO)
19. Aplicar correcciones autorizadas:
    - Reescritura de preguntas (mantener terminología del documento).
    - Homogeneización de longitudes de opciones (±10%).
    - Eliminación o reemplazo solo si autorizado.
20. Registrar VERSIONADO antes de aplicar cambios: guardar hash de la versión previa (sha1/sha256) y generar diff (Nº | Original → Reescrito | Motivo | Prioridad).

G — Verificación final
21. Recalcular similitud y solapamiento. Condiciones de éxito:
    - No queden pares con coseno > 0.70 (salvo justificadas).
    - No queden pares con solapamiento de entidades >50% (salvo justificadas).
    - Bloom y longitudes dentro de tolerancias.
22. Entregar:
    - Tabla final en el chat (Nº | Pregunta Hard | Opción1 | Opción2 | Opción3 | Answer | Notas)
    - Informe técnico resumido: duplicados/solapamientos, estadísticas Bloom, % opciones fuera de ±10%, matriz de cobertura (preguntas por concepto), listado NO_CUBIERTOS (si aplica)
    - Diff de reescrituras (solo ítems cambiados)

H — Distractores y dificultad (criterio para alta dificultad)
23. Reglas:
    - Longitud de opciones ±10% y estructura sintáctica paralela.
    - Distractores deben aparecer textual o conceptualmente en el documento.
    - Similitud semántica distractor-correcta objetivo: 0.45–0.70.
    - Al menos un distractor: contraposición de matiz; otro: generalización/reducción.
    - En Notas indicar: plausibilidad(sim:X) ; ocurrencias_in_texto:Y.

I — QA heurística (estimaciones de dificultad y pistas)
24. Ejecutar tests heurísticos:
    - Test Novicio: estimación P(novicio acierta)
    - Test Experto: estimación P(experto acierta)
    - Test ML heurístico: detección de pistas formales (longitud, tokens distintivos, posición)
25. Reportar ítems: demasiado fáciles (novicio P>0.70) o sospechosos de pista.

J — Control humano y autorizaciones
26. Antes de aplicar cambios de Alta prioridad (reescrituras >30% o eliminaciones) emitir 'APROBACIÓN REQUERIDA'.
27. Modos de ejecución:
    - AUTORIZO EJECUCIÓN COMPLETA → aplicar correcciones automáticamente.
    - QUIERO REVISIÓN MANUAL → entregar informe y propuestas sin aplicar.

K — Notas: formato plano y trazable (obligatorio)
28. Notas deben constar de dos líneas, texto plano, sin comillas:
    - Línea 1 — Cita: texto exacto tal cual aparece en el documento
    - Línea 2 — Justificación: explicación breve (≤18 palabras) que conecte la cita con la respuesta correcta
    Ejemplo:
    Cita: texto exacto ...
    Justificación: conecta la cita con la respuesta porque ...

L — Limitaciones y supuestos
29. No realizar búsquedas externas ni usar otras fuentes.
30. No inventar información ni completar datos ausentes: marcar 'INCOMPLETO' si no se pueden generar preguntas legítimas.
31. Reescrituras >30% requieren aprobación humana.

M — Extras para fomentar reflexión
32. Por cada 15 ítems generados, añadir 2 preguntas reflexivas (basadas exclusivamente en el texto) con su cita motivadora.


OBSERVACIONES FINALES
- Si el documento es muy extenso y existen suficientes conceptos viables, el sistema puede exceder 70 preguntas justificando la decisión en el informe técnico.
- Si el documento no permite alcanzar el mínimo absoluto, se entregará el conjunto viable y se marcará INCOMPLETO con lista de faltas.

INSTRUCCIONES DE EJECUCIÓN:
AUTORIZO EJECUCIÓN COMPLETA  
OMITIR_RESUMEN

