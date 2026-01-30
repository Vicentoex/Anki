#Anki 

%%> **Nota**: Este prompt está optimizado para generar **solo preguntas abiertas** con respuestas textuales exactas. No incluye sección de "notas" ni explicaciones adicionales.%%


# Paso 1

## **Requisitos Fundamentales:**

1. Fidelidad absoluta al documento:

    - Prohibido inventar, extrapolar o añadir información no explícita

    - Usar exclusivamente terminología y datos presentes en el texto

    - Mantener proporciones y énfasis originales del contenido

2. Metodología de Análisis:

    - Realizar 3 lecturas sucesivas:

        1. Lectura estructural: Identificar jerarquía de contenidos

        2. Lectura analítica: Mapear relaciones concepto-experimento-dato

        3. Lectura crítica: Detectar matices y diferenciadores clave

4. Sistema de Control de Calidad: ^4d6d5d

    - Crear matriz de seguimiento con:

        - Conceptos cubiertos

        - Preguntas por nivel cognitivo (Bloom)

        - Palabras clave utilizadas

  

A continuación se te darán una serie de pasos, los cuales debes seguir de forma rigurosa y minuciosamente


**Pasos a seguir de forma rigurosa y minuciosamente:**

Paso 1: Lectura, descomposición semántica y análisis inicial

Objetivo: Extraer y organizar exhaustivamente el contenido de documentos s

### 1. Extracción inicial de componentes

1.1. Leer documento(s) completo(s) atentamente

1.2. Identificar y registrar:
  
- Conceptos fundacionales

- Datos/estadísticas clave

- Entidades nombradas (personas, teorías, fechas, instituciones, etc)

- Afirmaciones contundentes (ej: "Se demuestra que...")

- Relaciones causa-efecto validadas

- Contraargumentos explícitos

- Elementos experimentales (objetivo, muestra, técnicas, conclusiones)

  

### 2. Análisis estratificado por relevancia

  

2.1. El 20% esencial (explica 80% del temario)

  

- Seleccionar elementos que cumplan:

  

> Alta frecuencia de aparición

  

> Conexión directa con conclusiones centrales

  

> Función como base para otros conceptos

  

2.2. Puntos e ideas principales + subideas

  

- 2.2.1. Ideas troncales (3-5 por documento)

- 2.2.2. Subideas críticas por cada idea troncal:

  

> Argumentos de soporte

  

> Evidencias clave

  

> Desarrollo conceptual

  

2.3. Autores relevantes

  

- Incluir solo cuando:

  

> Su teoría/concepto es fundamental

  

> Son citados como autoridades

  

2.4. Ideas menos relevantes

  

- Características:

  

> Contextuales pero no decisivas

  

> Metodologías secundarias

  

> Datos complementarios

  

2.5. Contenido prescindible

  

- Identificar:

  

> Ejemplos repetitivos

  

> Casos de estudio marginales

  

> Ilustraciones didácticas sin carga conceptual

  

### 3. Procesamiento comparativo (solo si hay 2 documentos)

  

3.1. Contrastar:

  

- Coincidencias en 20% esencial

- Diferencias en ideas troncales

- Evolución de conceptos

- Conceptos mencionados en un documento y que no están mencionados en el otro

  

3.2. Crear tabla comparativa:

  

| Elemento | Doc 1 | Doc 2 | Convergencia/Divergencia |

  

### 4. Validación final

  

4.1. Verificar que:

  

- Todo el 20% esencial está en 2.1

- Las subideas (2.2.2) soportan ideas troncales (2.2.1)

- Lo prescindible (2.5) no contiene información única

  

4.2. Eliminar redundancias y solapamientos

  

- [ ] Verificar coherencia estratificación

- [ ] Eliminar redundancias

- [ ] Confirmar exclusión de contenido no esencial


# Paso 2
### **Requisitos Fundamentales**  
1. **Fidelidad textual absoluta**  
   - La respuesta **debe ser una cita exacta del documento** (frase, definición o explicación completa).  
   - Prohibido parafrasear, resumir o añadir interpretaciones.  
   - Si el texto dice: *"El fenómeno X se define como la regulación dinámica del equilibrio interno"*, la respuesta **no** puede ser "Mantenimiento del equilibrio" (aunque sea correcto conceptualmente).  

2. **Enfoque en preguntas abiertas**  
   - Las preguntas deben requerir una **explicación breve o descripción directa** (ej: *"Explique el proceso de Y según el documento:"*).  
   - Evitar preguntas que admitan respuestas de 1-2 palabras (ej: ❌ "¿Qué es X?" → ✅ "Explique la definición de X según el texto:").  

---

### **Diseño de Preguntas**  
#### **Estructura obligatoria**  
- **Preguntas**:  
  - Máximo 20 palabras, sin contexto adicional.  
  - Usar verbos como: *"Explique..."*, *"Describa..."*, *"Analice..."*, *"¿Cuál es el proceso...?"*.  
  - Ejemplo válido: *"Explique la relación causa-efecto entre A y B según el estudio:"*.  
  - Ejemplo inválido: *"¿Cuál es el año de publicación del documento?"* (esta es una pregunta cerrada).  

- **Respuestas**:  
  - **Cita textual exacta** del documento (puede ser una oración completa).  
  - No se aplica límite de palabras, pero debe ser concisa y relevante.  

#### **Distribución por taxonomía de Bloom**  
| Nivel       | %    | Ejemplo de pregunta                                                                 |  
|-------------|------|-----------------------------------------------------------------------------------|  
| **Recordar**   | 15%  | Cite textualmente la definición de homeostasis:                                   |  
| **Comprender** | 30%  | Explique en qué consiste el fenómeno X según el documento:                        |  
| **Aplicar**    | 25%  | Mencione un ejemplo concreto del proceso Y citado en el texto:                    |  
| **Analizar**   | 20%  | Analice la relación entre A y B según los datos del estudio:                      |  
| **Evaluar**    | 10%  | ¿Por qué se considera válido el método Z en este contexto? (Cite la justificación): |  

---

### **Formato de Salida Obligatorio**  
```markdown
| Nº | Pregunta                                                                 | Respuesta                                                                 |
|----|--------------------------------------------------------------------------|---------------------------------------------------------------------------|
| 1  | Explique el proceso de fotosíntesis según el documento:                  | "La fotosíntesis es el proceso mediante el cual las plantas convierten la luz solar en energía química." |
| 2  | Analice la relación entre X e Y según los datos presentados:             | "Los resultados muestran que X incrementa la eficiencia de Y en un 40% (p < 0.05)." |
```

---

### **Sistema de Control de Calidad**  
1. **Verificación de textualidad**  
   - Todas las respuestas deben coincidir **palabra por palabra** con el documento.  
   - Si el texto dice *"regulación dinámica del equilibrio interno"*, no se acepta *"equilibrio dinámico interno"*.  

2. **Evitar preguntas ambiguas**  
   - Preguntarse: *"¿Podría un experto fallar esta pregunta por falta de claridad en el documento?"*.  
   - Si la respuesta es *"sí"*, reformular la pregunta.  

3. **Anti-repetición**  
   - Usar similitud semántica para evitar duplicados (ej: si ya existe *"Explique X"*, no crear *"¿En qué consiste X?"*).  

4. **Cobertura obligatoria**  
   - Priorizar en este orden:  
     1. **20% esencial** (paso 1) → 2. **Ideas troncales** → 3. **Subideas críticas** → 4. **Autores relevantes**.  
   - Ninguna pregunta puede saltarse niveles previos.  

---

### **Ejemplos Válidos vs. Inválidos**  
| Tipo       | Pregunta                                                                 | Respuesta                                                                 | ¿Válido? |  
|------------|--------------------------------------------------------------------------|---------------------------------------------------------------------------|----------|  
| **Válido** | Explique el mecanismo de acción de la enzima Z:                          | "La enzima Z cataliza la hidrólisis del ATP en ADP y fosfato inorgánico." | ✅       |  
| **Inválido** | ¿Qué es la homeostasis?                                                  | "Mantenimiento del equilibrio interno"                                   | ❌ (Parafraseo + pregunta cerrada) |  

---

### **Reglas Finales**  
- **Nunca entregar** preguntas sin verificar la coincidencia textual 100%.  
- Si el documento no contiene información para un nivel de Bloom (ej: "Evaluar"), redistribuir el porcentaje a niveles inferiores.  
- Eliminar cualquier pregunta que no cumpla con la estructura de salida obligatoria.  

