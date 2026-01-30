#Anki  Actualizado a [[Datos/Anki/MCQ/MCQ 5.1]]
 >[!WARNING] Atención:
 >Solo sirve para 1 Documento
 >Para verficación admite [[Datos/Anki/DLC's/Verificación pro (5 y 5.1)]]
 >Se puede elegir automatismo total o parte manual humana 
 >
 >UTORIZACIÓN PARA EJECUCIÓN:  
 -Si desea que el sistema aplique automáticamente las correcciones propuestas, incluya al final del mensaje: AUTORIZO EJECUCIÓN COMPLETA
-Si prefiere revisar las propuestas antes de aplicar cualquier cambio, incluya: QUIERO REVISIÓN MANUAL
-Para omitir el resumen previo, incluya: OMITIR_RESUMEN
 
 

TAREA: Verificación, refinamiento y generación de un banco de preguntas a partir de UN SOLO documento. Mantener FIDELIDAD ABSOLUTA al texto fuente (no introducir información externa ni inventada). Entregar todo en español (registro formal). Mínimo 15 preguntas si es posible, máximo 50 preguntas

ENTRADA: Documento principal (archivo o texto).  
SALIDA FINAL (obligatoria en el chat): tabla con columnas:
Nº | Pregunta (Hard) | Opción 1 | Opción 2 | Opción 3 | Answer | Notas

FLUJO DE TRABAJO (ejecutar en este orden)

A — EXTRACCIÓN (tres lecturas obligatorias)
1. Lectura estructural: identificar títulos, secciones y jerarquía de contenidos; entregar un índice sintético de secciones.
2. Lectura analítica: extraer y listar —con citas— conceptos fundacionales, datos/estadísticas clave, entidades nombradas, afirmaciones contundentes, relaciones causa→efecto y elementos experimentales (objetivo, muestra, técnicas, conclusiones).
3. Lectura crítica: identificar matices, limitaciones, contradicciones y huecos del texto.

B — Síntesis previa y mapeo (resumen previo)

4. Generar la matriz de extracción (concepto → cita textual → breve nota de vínculo).
5. Identificar el "20% esencial" que explica ~80% del temario (3–6 ítems) con justificación breve y citas.
6. Proponer 5 mapeos ejemplo pregunta ← cita (formato: Nº ejemplo | Pregunta sugerida Hard | Cita textual que la respalda | Nivel Bloom sugerido).
7. Si el usuario indica 'OMITIR_RESUMEN', saltar al apartado C.

C — REVISIÓN AUTOMÁTICA (criterios cuantificables)
8. Calcular similitud semántica entre todas las preguntas (embeddings/Tf-idf).
   - Umbral duplicado: coseno > 0.70 → marcar DUPLICADO.
9. Calcular solapamiento de entidades clave entre pares.
   - Umbral crítico: intersección/menor_conjunto > 50% → marcar SOLAPAMIENTO_CRÍTICO.
10. Comprobar uniformidad de longitud de opciones: tolerancia ±10% (número de caracteres).
11. Comprobar distribución Bloom objetivo (configurable por el usuario; por defecto):
   - Recordar 65% ±5
   - Comprender 30% ±5
   - Aplicar 5% ±2
12. Control de posición de respuestas: no es relevante (rotación externa aceptada).

D — Reglas obligatorias de fidelidad y manejo de contenido
13. FIDELIDAD ABSOLUTA (operativa):
    - No introducir información que no aparezca explícitamente en el documento.
    - No inferir hechos nuevos ni completar datos ausentes.
    - Se permite paráfrasis mínima en opciones (máx. 25% cambio léxico) solo si la cita textual es excesivamente larga; la Nota debe incluir la cita textual completa que respalda la opción correcta.
    - Si una afirmación es ambigua, marcar 'AMBIGUO' y no generar pregunta sin autorización.
14. FIGURAS / TABLAS / ECUACIONES:
    - Si existe leyenda textual usarla como cita.
    - Si no hay leyenda, describir la figura en ≤20 palabras y referenciar el párrafo que la menciona.
    - Si no es extraíble ni describible con seguridad, marcar 'FIGURA_NO_EXTRAÍBLE' y no generar pregunta derivada sin autorización.

E — Propuestas de corrección (si se detectan problemas)
15. Para cada DUPLICADO (coseno > 0.7): proponer eliminar o REESCRIBIR y ofrecer texto alternativo que mantenga fidelidad textual.
16. Para cada SOLAPAMIENTO_CRÍTICO: proponer matización (definición vs ejemplo) o ajuste de nivel Bloom; ofrecer texto alternativo.
17. Priorizar propuestas: Alta (cambio urgente), Media (recomendado), Baja (opcional).
18. FALLBACK por defecto: marcar pares como 'PAREADO' y proponer reescritura alternativa. No eliminar ítems automáticamente salvo que el usuario incluya explícitamente 'AUTORIZO EJECUCIÓN COMPLETA'.

F — REFINAMIENTO (aplicar cambios si AUTORIZADO)
19. Aplicar correcciones autorizadas por el usuario:
    - Reescritura de preguntas (manteniendo terminología del documento).
    - Homogeneización de longitudes de opciones (±10%).
    - Eliminación o reemplazo de ítems solo si se ha autorizado.
20. Registrar VERSIONADO antes de aplicar cambios: guardar hash de la versión previa (sha1/sha256) y, tras aplicar cambios, generar diff con formato: Nº | Original → Reescrito | Motivo | Prioridad.

G — Verificación final
21. Recalcular similitud y solapamiento. Condiciones de éxito:
    - No quedar pares con coseno > 0.70 (o quedar sólo los marcados y justificados).
    - No quedar pares con solapamiento de entidades > 50% (salvo casos justificados).
    - Bloom y longitudes dentro de tolerancias indicadas.
22. Entregar:
    - Tabla final en el chat (Nº | Pregunta Hard | Opción1 | Opción2 | Opción3 | Answer | Notas)
    - Informe técnico resumido (duplicados/solapamientos, estadísticas Bloom, % opciones fuera de ±10%)
    - Diff de reescrituras (solo ítems cambiados)

H — Distractores y dificultad (criterio para alta dificultad)
23. Reglas para distractores extremadamente plausibles:
    - Longitud de opciones ±10% (caracteres) y estructura sintáctica paralela.
    - Todos los distractores deben aparecer textual o conceptualmente en el documento.
    - Similitud semántica objetivo entre distractor y correcta: 0.45–0.70.
    - Al menos un distractor será una contraposición de matiz; otro será una generalización o reducción.
    - En Notas indicar plausibilidad(sim:X) ; ocurrencias_in_texto:Y.

I — QA heurística (estimaciones de dificultad y pistas)
24. Ejecutar evaluaciones heurísticas:
    - Test Novicio: estimación P(novicio acierta)
    - Test Experto: estimación P(experto acierta)
    - Test ML (heurístico): detección de pistas formales (longitud, tokens distintivos, posición)
25. Reportar ítems: demasiado fáciles (novicio P>0.70) o sospechosos de pista.

J — Control humano y autorizaciones
26. Antes de aplicar cambios de Alta prioridad (eliminación o reescritura que altere >30% del texto), emitir etiqueta 'APROBACIÓN REQUERIDA'.
27. Modos de ejecución:
    - Si el usuario incluye: AUTORIZO EJECUCIÓN COMPLETA → aplicar automáticamente las correcciones propuestas.
    - Si el usuario incluye: QUIERO REVISIÓN MANUAL → entregar informe y propuestas sin aplicar cambios.


K — Notas: formato plano y trazable (obligatorio)
30. Notas deben constar de dos líneas, texto plano, sin comillas:
    - Línea 1 — Cita: texto exacto tal cual aparece en el documento
    - Línea 2 — Justificación: explicación breve (≤18 palabras) que conecte la cita con la respuesta correcta
    Ejemplo:
    Cita: texto exacto ...
    Justificación: conecta la cita con la respuesta porque ...

L — Limitaciones y supuestos
31. No realizar búsquedas externas ni usar otras fuentes.  
32. No inventar información ni completar datos ausentes. En caso de ambigüedad o ausencia, marcar 'INCOMPLETO' y listar fragmentos que requieren aclaración.  
33. Reescrituras >30% requieren aprobación humana.

M — Extras para fomentar reflexión.
34. Por cada 15 ítems, generar 2 preguntas reflexivas (basadas exclusivamente en el texto) para fomentar pensamiento profundo; incluir la cita que motiva la pregunta.

AUTORIZACIÓN PARA EJECUCIÓN:
 AUTORIZO EJECUCIÓN COMPLETA: deseo que el sistema aplique automáticamente las correcciones propuestas



## Parte 2
Segunda Iteración: Cierre de Brechas

- **Detección final de omisiones**:
    
    - Preguntas clave:
        
        "¿Quedan conceptos esenciales sin cubrir?"
        
        "¿Incluiría un experto esto en evaluación?"
        
    - Considerar SOLO si:
        
        ✓ Es fundamental para comprensión
        
- **Generación final**:
    
    - Crear preguntas solo para huecos críticos
    - Priorizar dificultad extrema y matices

Nota:Formato de respuesta

- Formato de tabla con columnas:
- Nº
- Pregunta
- Opción 1 / Opción 2 / Opción 3 (En diferentes columnas)
- Answer (texto exacto de la opción correcta)
- Notas (justificación de la respuesta y explicación de esta; si es experimento, describir objetivo, muestra, técnicas y conclusiones).
- Confirmar que la tabla final incluya siempre las columnas: Nº, Pregunta, Opción 1/2/3, Answer y Notas.
