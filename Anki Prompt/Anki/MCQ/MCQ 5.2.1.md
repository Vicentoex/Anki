>[!Warning] A tener en cuenta
>- Solventa problemas dados [[Datos/Anki/MCQ/MCQ 5.2]] , en lo general son iguales pero: 
> 	 - [[Datos/Anki/MCQ/MCQ 5.2.1]] No permite tanta personalización → ↑Automatización
> 	   - ↑Precisión 
> 	   - ↑Fiabilidad
> 	   - Mejor para documentos largos 
> 	   - Elimina Python redundante 


---


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
num_options = 3  # A, B, C
objetivo = 30-70 preguntas (adaptativo según documento)
longitud_opciones = ±10% media
temperature = 0.0
plausibilidad_distractores = 0.48-0.52  # Similitud con respuesta correcta (muy alta dificultad)
```

**Distribución Bloom (configurable por usuario):**
- Recordar: 50% ±5
- Comprender: 30% ±5  
- Aplicar: 20% ±5

**Multi-documento:** Si hay 2 documentos, priorizar doc1 en caso de contradicciones. Documentar contradicción en informe.

---

## PALABRAS PROHIBIDAS EN OPCIONES (anti-sesgos)

**Absolutos:** siempre, nunca, jamás, todos, todas, ninguno, ninguna, únicamente, exclusivamente, solamente

**Cuantificadores:** 100%, 0%, absolutamente, completamente, totalmente

**Conectores:** "todas las anteriores", "ninguna de las anteriores", "todas son correctas", "a y b son correctas"

**Acción:** Reescribir inmediatamente usando términos matizados (ej: "siempre" → "habitualmente", "frecuentemente").

---

## EXTRACCIÓN

Extraer: definiciones, datos numéricos, metodología (si aplica), autores/teorías, relaciones causales, limitaciones. Guardar citas textuales (≥3 palabras) con ubicación aproximada. Identificar conceptos clave del documento.

---

## ESTRATIFICACIÓN Y OBJETIVO

Clasificar conceptos:
- **Esencial** → 3-4 preguntas
- **Ideas troncales** → 2-3 preguntas
- **Subideas** → 1-2 preguntas
- **Autores/teorías** → 1-2 preguntas (si aportan diferenciación)
- **Menor** → 0-1 pregunta

**Objetivo adaptativo:**
```
<2.000 palabras → 30
2.000-5.000 → 50
5.000-10.000 → 70
>10.000 → 70 máx
```
Ajustar: densidad >0.7 (+20%), densidad <0.3 (-20%). Límites: 30-70.

---

## GENERACIÓN DE PREGUNTAS

### Enunciado
Claro, auto-contenido, 15-40 palabras. Evitar dobles negaciones. Una sola respuesta correcta posible. **Basado exclusivamente en documento.**

### Opciones
Formato: `A: [texto]`, `B: [texto]`, `C: [texto]`

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
Byte-a-byte idéntico a opción correcta, con prefijo: `A: [texto completo]` o `B: ...` o `C: ...`

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
- **Opción 1, 2, 3:** Texto SIN prefijo A:/B:/C:
- **Answer:** CON prefijo completo (ej: `A: Inhibidores de la ECA`)
- **Nota:** →
```
  'Cita textual'; No son las otras opciones porque [explicación B incorrecta] y [explicación C incorrecta]
```
  - Cita entre comillas simples
  - Separador: `;` después de cita
  - Explicación máx 50 palabras
  - Solo texto, `:` y `;` (prohibido `|`, `"`, saltos línea)
  
- **Bloom:** Recordar | Comprender | Aplicar
- **Confianza:** 0.00-1.00 (cita válida 30% + distractores plausibles 40% + fidelidad 30%)

**Ejemplo Nota:**
```
'Los IECA bloquean enzima convertidora de angiotensina'; No son las otras opciones porque B describe ARA-II que bloquean receptores downstream y C describe inhibidores de renina upstream
```

---

### Informe Técnico (máx 300 palabras)

Incluir: longitud documento, densidad conceptual, conceptos extraídos, objetivo calculado, preguntas generadas, cobertura esenciales, duplicados detectados/resueltos, confianza promedio, distribución Bloom (% por nivel), incidencias (INCOMPLETAS con razón, palabras prohibidas corregidas, ajustes longitud, distractores adaptados de contexto).

Formato: párrafo continuo, datos separados con `;`

---

## SALIDA

Entregar:
1. Tabla Markdown completa
2. Informe Técnico

No mostrar razonamiento interno.

---

**FIN DEL PROMPT**