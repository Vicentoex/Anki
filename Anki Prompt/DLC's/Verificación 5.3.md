
Ejecutar procedimiento de verificación completo sobre banco de preguntas generado. Aplicar fases A→E en orden.

  

---

  

## PARÁMETROS VERIFICACIÓN

```

plausibilidad_distractores = 0.48-0.52  # Muy alta dificultad

longitud_opciones = ±10%

bloom_target = La brindada por el usuario (configurable)

umbral_duplicado = 0.70 (marcar) | 0.85 (eliminar)

umbral_solapamiento = 0.50

```

  

  

**Autorización:** Si usuario incluye `"AUTORIZO EJECUCIÓN COMPLETA"` → aplicar correcciones automáticamente. Si no → detener en Fase B y solicitar confirmación.

  

---

  

## FASE A — DETECCIÓN DE PROBLEMAS

  

### 1. Duplicados semánticos

Comparar cada par de preguntas. Identificar conceptos clave compartidos (≥4 conceptos = posible duplicado).

- Similitud alta estimada (≥70% conceptos compartidos) → marcar DUPLICADO

- Similitud muy alta (≥85% conceptos + mismo Bloom) → marcar ELIMINAR_URGENTE

  

### 2. Solapamiento de entidades

Extraer entidades clave por pregunta (términos técnicos, nombres propios, conceptos). Comparar pares.

- Solapamiento >50% entidades → marcar SOLAPAMIENTO_CRÍTICO

  

### 3. Longitud opciones

Por pregunta: contar palabras de cada opción, calcular media, verificar que todas estén en media ±10%.

- Si violación → marcar LENGTH_VIOLATION

  

### 4. Distribución Bloom

Contar preguntas por nivel. Calcular porcentajes. Comparar con target configurado.

- Delta >5% en cualquier nivel → marcar BLOOM_DESVIADO

  

### 5. Validación distractores rango 0.48-0.52

Por pregunta: evaluar si distractores cumplen criterio de alta similitud (comparten >80% terminología, diferencia solo en matices).

- Si distractor obviamente descartable (opuesto simple, diferencia amplia) → marcar DISTRACTOR_FÁCIL

  

### 6. Verificación excepción fidelidad

Identificar distractores que fueron adaptados/construidos (no literales del documento).

- Verificar que estén basados en contexto: usan terminología documento, conceptos mencionados, relaciones derivables.

- Si distractor inventado sin base contextual → marcar FIDELIDAD_VIOLADA

  

### 7. Cobertura conceptual

Revisar matriz de cobertura (si disponible en informe original). Verificar que todos los conceptos tienen ≥1 pregunta.

- Conceptos sin preguntas → añadir a listado NO_CUBIERTOS_VERIFICADO

  

  

---

  

## FASE B — INFORME TÉCNICO

  

Generar informe estructurado:

  

### Duplicados

Lista: `[Nº i, Nº j, similitud estimada, acción propuesta]`

- Acción: ELIMINAR (menor confianza) | REESCRIBIR (cambiar Bloom o enfoque)

- Si REESCRIBIR: incluir propuesta de texto reescrito manteniendo fidelidad

  

### Solapamientos críticos

Lista: `[Nº i, Nº j, ratio solapamiento, entidades compartidas, acción propuesta]`

- Acción: MATIZAR (enfocar matiz distinto) | CONVERTIR (cambiar tipo pregunta)

- Incluir propuesta de modificación

  

### Violaciones longitud

Lista: `[Nº pregunta, desviación %, acción]`

- Acción: ajustar opción(es) específica(s) manteniendo significado

  

### Distribución Bloom

Comprobar que se cumple con la distribución propuesta

- Si fuera de tolerancia: proponer reescrituras para equilibrar

  

### Distractores problemáticos

Lista: `[Nº pregunta, distractor(es) problemático(s), razón, propuesta]`

- Razones: obviamente descartable, no cumple 0.48-0.52, fidelidad violada

- Propuesta: distractor alternativo basado en documento

  

### Cobertura

```

Conceptos totales: X

Conceptos cubiertos: Y

NO_CUBIERTOS_VERIFICADO: [lista conceptos sin pregunta]

```

- Si hay NO_CUBIERTOS: proponer preguntas adicionales

  

  

### Estadísticas finales

```

Preguntas totales: X

Duplicados: X (Y eliminar, Z reescribir)

Solapamientos: X

Violaciones longitud: X

Confianza promedio: 0.XX

```

  

**SI NO HAY `"AUTORIZO EJECUCIÓN COMPLETA"` → DETENER AQUÍ Y SOLICITAR CONFIRMACIÓN**

  

---

  

## FASE C — APLICACIÓN DE CORRECCIONES (solo si autorizado)

  

### 1. Eliminar duplicados urgentes

Eliminar preguntas marcadas ELIMINAR_URGENTE (similitud >0.85). Priorizar mantener pregunta con mayor confianza.

  

### 2. Reescribir duplicados moderados

Aplicar propuestas de reescritura de Fase B. Mantener fidelidad al documento, cambiar enfoque o nivel Bloom.

  

### 3. Resolver solapamientos

Aplicar propuestas MATIZAR o CONVERTIR de Fase B.

  

### 4. Ajustar longitudes

Homogeneizar opciones a media ±10%. Conservar significado y fidelidad.

  

### 5. Corregir distractores

Reemplazar distractores problemáticos con propuestas de Fase B. Asegurar cumplimiento 0.48-0.52 y base contextual.

  

### 6. Cubrir huecos conceptuales

Generar preguntas para conceptos en NO_CUBIERTOS_VERIFICADO (si aplica).

  

### 7. Corregir formato Notas

Aplicar formato correcto: `'Cita'; No son las otras opciones porque [explicación]`

  

### 8. Equilibrar Bloom

Si necesario, reescribir preguntas para alcanzar distribución target. Cambiar nivel sin perder fidelidad documental.

  

---

  

## FASE D — DIFF DE CAMBIOS

  

Por cada ítem modificado:

```

Nº X | TIPO_CAMBIO | Descripción breve

```

  

Tipos:

- ELIMINADO: duplicado con Nº Y (sim=0.XX)

- REESCRITO: [razón] → [cambio aplicado]

- DISTRACTOR_CORREGIDO: [razón]

- LONGITUD_AJUSTADA: opciones homogeneizadas

- NOTA_CORREGIDA: formato ajustado

- BLOOM_AJUSTADO: nivel cambiado de X a Y

- NUEVO: cubre concepto Z no cubierto

  

Máximo 2 líneas por cambio. Mostrar primeros 10 cambios, resumir resto.

  

---

  

## FASE E — VERIFICACIÓN FINAL

  

### Recalcular métricas

  

1. **Duplicados:** No quedar pares con similitud >70%

2. **Solapamientos:** No quedar pares con ratio >50%

3. **Bloom:** Dentro de tolerancia (±deltas configurados)

4. **Longitud:** Todas opciones ±10%

5. **Distractores:** Todos cumplen 0.48-0.52 y base contextual

6. **Formato:** Todas Notas con formato correcto

7. **Confianza:** Promedio ≥0.70

  

### Resultado

- Si todas condiciones OK → `STATUS: APROBADO`

- Si falla alguna → `STATUS: REQUIERE_REVISIÓN` + lista incumplimientos

  

**Si REQUIERE_REVISIÓN y autorizado:** volver a Fase C (máx 1 ciclo adicional)

  

---

  

## SALIDA FINAL

  

Entregar:

1. **Tabla Markdown actualizada** (todas las preguntas finales)

2. **Informe técnico resumido** (máx 300 palabras):

   - Duplicados resueltos: X eliminados, Y reescritos

   - Solapamientos resueltos: X

   - Violaciones longitud corregidas: X

   - Distractores corregidos: X

   - Bloom final: Recordar X%, Comprender Y%, Aplicar Z%

   - Confianza promedio final: 0.XX

   - Cobertura conceptual: X/Y conceptos cubiertos

   - Preguntas añadidas para cobertura: X

   - Formato Notas corregido: X

3. **Diff conciso** (cambios aplicados, máx 2 líneas por ítem)

  

---

  

**FORMATO RESPUESTA USUARIO:**

  

  

`"AUTORIZO EJECUCIÓN COMPLETA"`

  

  

---

  

**FIN PROMPT VERIFICACIÓN**
