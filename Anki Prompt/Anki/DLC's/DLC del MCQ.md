[[Datos/Anki/MCQ/MCQ 4]] #Anki 

```insta-toc
---
title:
  name: Table of Contents
  level: 1
  center: false
exclude: ""
style:
  listType: dash
omit: []
levels:
  min: 1
  max: 6
---

# Table of Contents

- Analisis de Divirgencias
    - Extensión para la PARTE 1: Análisis Profundo de Divergencias
    - 3.bis. Taxonomía y Jerarquización de Divergencias Significativas
    - Extensión para la PARTE 2: Generación de Preguntas Basadas en Divergencias
    - 2.bis. Protocolo de Generación de Preguntas de Razonamiento Crítico (Basado en Divergencias)
- Muy muy Simplificado
- Preguntas más difíciles y de entendimiento
- Revisión Técnica de aleatoriedad: Examen
- Revisión PRO (Examen)
    - 1. Revisión Final Definitiva (Nivel Sigma)
        - 1.1. Mapa de Conocimiento Completo
        - 1.2. Análisis de Brechas Residuales
    - 2.Validación Técnica Multinivel
        - Capa 1 - Coherencia Estructural
        - Capa 2 - Fidelidad Textual
        - Capa 3 - Análisis de Patrones Avanzado
    - Fase 3: Certificación de Calidad Total
        - 1. Auditoría de Dificultad Certificada
        - 2.Sistema Anti-Pistas Ocultas
        - 3.Validación de Explicaciones en Notas
    - Fase 4: Informe Final de Calidad
```
```insta-toc
```



# Analisis de Divirgencias
### **Extensión para la PARTE 1: Análisis Profundo de Divergencias**

_(Este módulo se inserta conceptualmente después del punto `3. Procesamiento comparativo` y antes del punto `4. Validación final` del Paso 1 original. Actúa como un refinamiento obligatorio del análisis comparativo inicial)._

### **3.bis. Taxonomía y Jerarquización de Divergencias Significativas**

**Objetivo:** No solo identificar, sino clasificar y evaluar la naturaleza e implicación de cada divergencia para forzar un razonamiento profundo. Este paso es el fundamento para la creación de preguntas de alto impacto.

3.bis.1. Clasificación Obligatoria de Divergencias:

Para cada punto de divergencia identificado en el paso 3.2, debes clasificarlo OBLIGATORIAMENTE en una de las siguientes categorías:

- **A. Contradicción Factual Directa:** Un documento afirma 'X' y el otro afirma 'No-X' sobre un dato, fecha o hecho objetivo.
    
- **B. Omisión Crítica:** Un documento desarrolla un concepto del "20% esencial" o una "Idea Troncal" que el otro ignora por completo, sugiriendo un vacío argumental o un enfoque diferente.
    
- **C. Diferencia de Énfasis o Perspectiva:** Ambos documentos mencionan un concepto, pero uno lo trata como central para su argumentación mientras que el otro lo presenta como secundario o desde un ángulo distinto (p. ej., teórico vs. práctico).
    
- **D. Discrepancia en la Interpretación:** Ambos documentos parten de datos o evidencias similares pero llegan a conclusiones o interpretaciones opuestas o marcadamente diferentes.
    
- **E. Evolución o Refutación:** Un documento presenta un modelo, teoría o dato que es una versión actualizada, más compleja o una refutación directa de lo presentado en el otro.
    

3.bis.2. Matriz de Implicaciones de Divergencia:

Debes crear una nueva tabla que profundice el análisis. Esta matriz es crucial y servirá de base para la generación de preguntas.

**Formato de Salida Obligatorio para la Matriz de Implicaciones:**

|ID_Divergencia|Tipo (A-E)|Descripción de la Divergencia|Implicación para el Razonamiento Profundo|Pregunta Potencial de Alto Nivel (Borrador)|
|---|---|---|---|---|
|DIV-01|[Letra de la categoría]|[Descripción concisa y neutra de la diferencia]|[Explica por qué esta divergencia es importante. ¿Qué supuesto subyacente revela? ¿Qué habilidad cognitiva se necesita para entenderla? (Ej: "Requiere que el lector evalúe la validez de dos metodologías opuestas")]|[Formula una pregunta borrador que explote esta tensión. (Ej: "¿Bajo qué supuesto la conclusión del Doc 1 sería más válida que la del Doc 2?")]|
|DIV-02|...|...|...|...|

3.bis.3. Validación de Relevancia:

Antes de finalizar este paso, debes autoevaluar y confirmar que cada divergencia incluida en esta matriz tiene una importancia muy relevante. Para ello, responde internamente a estas preguntas:

- ¿Comprender esta divergencia es fundamental para dominar el tema a un nivel de experto?
    
- ¿Podría esta divergencia ser la base de un debate académico o profesional?
    
- ¿Evalúa algo más que la simple memorización de dos textos aislados?
    

Si la respuesta a las tres preguntas no es afirmativa, la divergencia debe ser considerada de baja relevancia y no debe incluirse en la Matriz de Implicaciones.

---

### **Extensión para la PARTE 2: Generación de Preguntas Basadas en Divergencias**

_(Este módulo modifica y añade especificaciones al `Paso 2: Diseño de preguntas` y a la `FASE 2: ITERACIÓN Y PERFECCIONAMIENTO` del prompt original)._

### **2.bis. Protocolo de Generación de Preguntas de Razonamiento Crítico (Basado en Divergencias)**

**Prioridad Absoluta:** Al generar preguntas, las que se derivan de la **Matriz de Implicaciones de Divergencia** tienen la máxima prioridad. Deben constituir el núcleo de las preguntas de nivel cognitivo "Analizar" y "Evaluar" (que se integrarán dentro del 5% de "Aplicar/Entender" del prompt original, ampliando su alcance).

2.bis.1. Arquetipos de Preguntas Obligatorias sobre Divergencias:

Debes formular las preguntas basándote en los siguientes arquetipos, directamente vinculados a la columna "Implicación para el Razonamiento Profundo" de tu matriz:

1. **Preguntas de Reconciliación o Síntesis:** "Considerando la evidencia [X] del Documento 1 y la teoría [Y] del Documento 2, ¿cuál de las siguientes afirmaciones representa la síntesis más lógica o la conclusión más probable?"
    
    - _Objetivo:_ Forzar a la IA a crear un puente conceptual entre dos posturas.
        
2. **Preguntas de Identificación de Supuesto Subyacente:** "La diferencia fundamental en las conclusiones entre ambos documentos sobre [tema] se debe probablemente a una divergencia en el supuesto de..."
    
    - _Objetivo:_ Evaluar la capacidad de identificar las premisas no declaradas que sustentan cada argumento.
        
3. **Preguntas de Evaluación de Fortaleza Argumental:** "¿Qué documento ofrece una base más sólida para afirmar [conclusión], dado que su enfoque se centra en [metodología/evidencia específica mencionada en el texto]?"
    
    - _Objetivo:_ Requerir una valoración crítica de la calidad de la evidencia o la lógica presentada.
        
4. **Preguntas de Consecuencia Lógica:** "Si la perspectiva del Documento 2 sobre [concepto] fuera completamente precisa, ¿cuál sería la principal debilidad o implicación para el argumento presentado en el Documento 1?"
    
    - _Objetivo:_ Probar el razonamiento hipotético y la comprensión de las interdependencias lógicas entre los textos.
        

2.bis.2. Adaptación de la Estructura 'Notas':

Para CADA pregunta generada a partir de la Matriz de Implicaciones de Divergencia, la sección 'Notas' DEBE seguir este formato ampliado y riguroso:

```
Notas: Basado en la Divergencia ID_[ID de la tabla, ej: DIV-01]. Cita Doc 1: ‘[Fragmento exacto]’. Cita Doc 2: ‘[Fragmento exacto]’. La divergencia es crítica porque [resumen de la columna ‘Implicación para el Razonamiento Profundo’]. No son las otras opciones porque [análisis de distractores enfocado en por qué no resuelven la tensión entre los textos].
```

2.bis.3. Integración en la Revisión y la Iteración:

Durante la FASE 2: ITERACIÓN Y PERFECCIONAMIENTO, el análisis de cobertura debe incluir una categoría específica y prioritaria:

- **Análisis de cobertura de Divergencias:**
    
    - Listar todos los `ID_Divergencia` de la Matriz que NO han sido utilizados para generar una pregunta.
        
    - Clasificar por relevancia: Todas las divergencias de la matriz son, por definición, de relevancia **MUY ALTA**.
        
    - **Generación complementaria obligatoria:** Crear preguntas para el 100% de los `ID_Divergencia` no cubiertos antes de proceder a cerrar brechas de otros temas. Se debe asegurar que cada punto crítico de fricción entre los documentos es evaluado.
        

---

# Muy muy Simplificado
    Lee el documento completo y, sin pasos intermedios, genera un banco de preguntas de opción múltiple (3 opciones cada una) que cumpla todos estos requisitos:
    
    - Cobertura total del contenido (conceptos, datos clave y experimentos: objetivo, muestra, técnicas y conclusiones).
    
    • Dificultad muy alta, sin pistas en la redacción.
    
    • Basado en objetivos de aprendizaje y tipologías cognitivas de Bloom, con la siguiente distribución:
    
    – 30 % nivel “comprender”
    
    – 40 % nivel “recordar”
    
    – 30 % nivel “aplicar”
    
    • Opciones uniformes: misma longitud y estructura, sin letras ni números en la redacción.
    
    • Distractores plausibles y retadores al 100 %.
    
    • Reparto equilibrado de respuestas correctas a lo largo del banco.
    
    • Formato de salida en tabla con columnas:
    
    Nº | Pregunta | Opción 1 | Opción 2 | Opción 3 | Respuesta | Notas
    
    – “Respuesta” es el texto exacto de la opción correcta.
    
    – “Notas” incluye justificación/explicación breve, y para experimentos detalla objetivo, muestra, técnicas y conclusiones.
    
    • Auto‑verifica internamente:
    
    – Que no haya pistas ni contexto excesivo.
    
    – Que se cubran todos los subtemas.
    
    – Uniformidad de estilo (tercera persona, sin jerga).
    
    – Equidad y dificultad de las alternativas.
    
    Entregar todo en un solo mensaje, directamente con la tabla completa.
    
# Preguntas más difíciles y de entendimiento    
**Diseño de Preguntas (Niveles Cognitivos Mezclados)**
- Para cada concepto generar 3 variantes:
        1. **Identificación pura** ("¿Qué estudio demostró X?")
        2. **Aplicación inversa** ("¿Qué resultado invalidaría la teoría Y?")
        3. **Análisis cruzado** ("Relacione A y B usando el principio C")
    
**Optimización de Distractores**
- Técnicas avanzadas:
        - **Espejismo contextual:** Usar datos correctos en contexto erróneo
        - **Inversión causal:** Plantear efectos como causas
        - **Exactitud parcial:** 75% verdad + 25% error
# Revisión Técnica de aleatoriedad: Examen

>[!TIP] Esto va en revisión y es para hacer un examen con respuestas aleatorias

    b)Revisión Técnica:
    
    - Test de Fisher para distribución aleatoria de respuestas correctas (p > 0.05)
    - Análisis de longitud mediante desviación estándar (<15% variación)

# Revisión PRO (Examen)

[[Datos/Anki/DLC's/Verificación pro (5 y 5.1)]] Es similar, pero un añadido para preguntas normales
## 1. Revisión Final Definitiva (Nivel Sigma)
    
    _Objetivo: Eliminar el 99.99966% de posibles errores mediante verificación multimodal_
    
    **Fase 1: Verificación Exhaustiva de Cobertura**
    
### 1.1. Mapa de Conocimiento Completo
        - Cruzar 100% de las preguntas con la **Matriz de Control de Contenido**
        - Requisito: Cada celda en "Estado" debe marcar **Cubierto**
        - Herramienta: Sistema de correlación automática pregunta-concepto (etiquetado semántico)
### 1.2. Análisis de Brechas Residuales
        - Ejecutar algoritmo de detección de vacíos:CopyDownload
            
            python
            
            ```
            if concepto['relevancia'] > 7/10 and concepto['preguntas_asociadas'] == 0:
                generar_alerta_critica()
            ```
            
        - Umbral de aceptación: 0% de conceptos clave sin pregunta asociada
            
    
## 2.Validación Técnica Multinivel
    
### Capa 1 - Coherencia Estructural
    
    - Checklist de 25 puntos incluyendo:
        - Distribución Bloom: 25% Recordar / 35% Comprender / 30% Aplicar / 10% Analizar (±2% tolerancia)
        - Densidad de Términos Técnicos: Mínimo 1.2 por pregunta
        - Índice de Repetición: <0.01% (NLP con embedding BERT)
    
### Capa 2 - Fidelidad Textual
    
    - Sistema de verificación cruzada:
        1. Extraer fragmentos del documento relacionados
        2. Comparar con justificaciones en "Notas" usando:
            - Coincidencia exacta de parámetros (n, p-valor, % exactos)
            - Mapeo página/párrafo en 100% de las afirmaciones
        3. Validar con algoritmo de matching semántico (threshold >95%)
    
### Capa 3 - Análisis de Patrones Avanzado
    
    - **Test de Fisher-Exact** para distribución de respuestas:
        - Hipótesis nula: Las opciones correctas están distribuidas aleatoriamente
        - Requisito: p-value > 0.05 (no patrones detectables)
    - **Análisis de longitud de opciones**:
        - Desviación estándar < 10% de la media
        - Test de Levene para homogeneidad de varianzas (α=0.01)
    
## Fase 3: Certificación de Calidad Total**

### 1. Auditoría de Dificultad Certificada
        - Simular 1000 respuestas de "estudiantes virtuales" (IA con distintos niveles de conocimiento)
        - Índices requeridos:
            - Tasa de acierto promedio: 22-28%
            - Índice de discriminación: >0.40
            - Punto biserial: >0.20
### 2.Sistema Anti-Pistas Ocultas
        - Análisis lingüístico computacional:
            - Detección de patrones léxicos (TF-IDF comparativo pregunta/respuestas)
            - Análisis de posicionamiento de términos clave (n-grams estratégicos)
        - Requisito: 0 coincidencias estadísticamente significativas
### 3.Validación de Explicaciones en Notas
        - Checklist de 12 criterios:CopyDownload
            
            plaintext
            
            ```
            1. Contiene referencia cruzada a ≥2 secciones del documento
            2. Incluye datos cuantitativos originales
            3. Explica claramente por qué cada distractor es incorrecto
            4. Menciona aplicaciones prácticas del concepto
            5. Vincula con el marco teórico general
            ... (hasta 12)
            ```
            
        - Umbral de aprobación: 11/12 criterios cumplidos
            
    
## Fase 4: Informe Final de Calidad
    
    Generar documento certificador con:
    
    - **Heatmap de Cobertura** (por capítulo/concepto)
    - **Curva Característica del Ítem** para cada pregunta
    - **Análisis de Consistencia Interna** (α de Cronbach >0.85)
    - **Reporte de Trazabilidad** (origen documental de cada ítem)
    
    **Mecanismo de Retroalimentación Ajustada**
    
    - Implementar loop de mejora continua:CopyDownload
        
        plaintext
        
        ```
        Errores detectados → Análisis de causa raíz → Actualizar reglas de generación → Re-testear
        ```
        
    - Requisito de salida: 3 iteraciones completas sin nuevos errores