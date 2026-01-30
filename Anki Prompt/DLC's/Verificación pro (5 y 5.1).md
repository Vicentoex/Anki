#Anki 
Esto es un [[Datos/Anki/DLC's/DLC del MCQ]] a parte

%%Notas: - Si quieres que el asistente aplique correcciones automáticamente, incluye la línea **"AUTORIZO EJECUCIÓN COMPLETA"** al final. (Opción puesta de forma automática)
    
- Si prefieres controlar cambio por cambio, escribe **"QUIERO REVISIÓN MANUAL"** y recibirás solo el informe y las propuestas. (Pegar lo siguiente: { "QUIERO REVISIÓN MANUAL": devuelve el informe y propuestas, sin aplicar cambios).})
    
- Si quieres variantes Hard/Med/Easy incluye además: **"GENERA VARIANTES: SÍ"** (por defecto solo Hard). *Inconveniente*: Se tendrá un banco de preguntas de x3 veces
	-  **Hard**: la versión más seca y mínima (la que ya has visto en la tabla final).
    
	- **Med**: versión intermedia, con redacción algo más neutra o simplificada, sin paréntesis ni tecnicismos innecesarios.
    
	- **Easy**: versión con un pequeño añadido aclaratorio tipo “según el texto”, para que quede más accesible pero sin dar pistas.


%%

Ejecuta el PROCEDIMIENTO FINAL para el banco de preguntas siguiente (aplica a todas las preguntas presentes en la conversación):

PARÁMETROS OBLIGATORIOS:
- Hard = versión más seca y mínima (obligatorio para cada ítem).
- Mantener fidelidad absoluta al documento fuente (cero información no textual).
- Salida final = tabla en el chat con columnas: Nº | Pregunta | Opción 1 | Opción 2 | Opción 3 | Answer | Notas

FASE A — Segundo pase (revisión en cascada)
1. Calcular hash semántico por pregunta y comparar contra todo el banco.
   - Umbral duplicado: similitud coseno > 0.70 ⇒ marcar como DUPLICADO.
2. Comparar solapamiento de entidades clave entre pares.
   - Umbral solapamiento a revisar: intersección/menor_conjunto > 50% ⇒ marcar como SOLAPAMIENTO_CRÍTICO.
3. Validar uniformidad de longitud de opciones:
   - Tolerancia: ±10% longitud (nº de caracteres) entre las 3 opciones de cada pregunta.
4. Validar distribución Bloom target:
   - Recordar: 65% ±5%
   - Comprender: 30% ±5%
   - Aplicar: 5% ±2%
5. Validar balance de posición de respuestas correctas:
   - Target: 33% por columna ±5% (aprox. 20/20/20 en 60 ítems)

Si detectas DUPLICADOS o SOLAPAMIENTO_CRÍTICO, marca los pares y propon soluciones:
- Si duplicado (cos > 0.7): proponer ELIMINAR o REESCRIBIR (diferenciar claramente la propuesta).
- Si solapamiento >50%: proponer MATIZAR o CONVERTIR uno de los ítems en pregunta distinta (definición vs. ejemplo, o distinto nivel Bloom).

FASE B — Informe técnico automático
1. Generar informe con:
   - Lista de duplicados (pares) y su similitud exacta.
   - Lista de solapamientos (>50%) con entidades compartidas.
   - Estadísticas: %Recordar/%Comprender/%Aplicar actuales, balance posiciones respuestas, %de opciones fuera de ±10%.
2. Adjuntar recomendaciones concretas (reemplazo o reescritura propuesta por cada caso).

FASE C — Tercer pase de refinamiento (aplicar cambios autorizados)
1. Aplicar las correcciones autorizadas por el/ la solicitante:
   - Reescribir preguntas marcadas (manteniendo terminología del documento).
   - Eliminar preguntas cuando proceda y reemplazar por nuevas que cubran huecos documentales (si procede).
2. Homogeneizar longitudes de opciones (±10%) conservando significado.
3. Generar variantes por ítem:
   - Hard: versión más seca y mínima (obligatoria).
   - Med: versión intermedia (opcional).
   - Easy: versión con aclarador "según el texto" (opcional).
   - Nota: por defecto solo generar Hard salvo que se pida explícitamente variantes.

FASE D — Generar diff de reescrituras
1. Por cada pregunta modificada, incluir:
   - Nº original → Texto original → Texto reescrito → Motivo del cambio (duplicado/solapamiento/longitud).
2. Mostrar diff conciso (máx. 2 líneas por cambio).

FASE E — Verificación semántica final
1. Recalcular similitud semántica y solapamiento de entidades sobre la versión final.
2. Requerimiento de aprobación (condición de éxito):
   - No quedarán pares con coseno > 0.70.
   - No quedarán pares con solapamiento de entidades > 50% (salvo marcados y justificados).
   - Bloom dentro de tolerancias indicadas.
   - Opciones uniformes ±10%.
   - Balance de respuestas 33% ±5%.

ENTREGABLES (formato final dentro del chat)
- Tabla en el chat con las preguntas finales (Nº|Pregunta|Opción1|Opción2|Opción3|Answer|Notas).
- Informe técnico resumido (duplicados/solapamientos/resumen Bloom).
- Diff de reescrituras (solo preguntas cambiadas).

AUTORIZACIÓN PARA EJECUCIÓN:
 "AUTORIZO EJECUCIÓN COMPLETA" :permito que se apliquen automáticamente todas las correcciones propuestas (reescrituras y homogeneización).


