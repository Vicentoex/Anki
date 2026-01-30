%%
## ARQUITECTURA COMPLETA DE 3 PROMPTS

```
┌─────────────────────────────────────────┐
│  PROMPT 1: GENERADOR PRINCIPAL          │
│  Input: Documento                       │
│  Output: Banco inicial (30-70 Q)        │
│          + Informe técnico              │
│  Características: Cobertura amplia,     │
│                   distribución inicial  │
└──────────────┬──────────────────────────┘
               │
               ├─────────────────────────────┐
               │                             │
               ▼                             ▼
┌──────────────────────────────┐  ┌──────────────────────────────┐
│  PROMPT 2: VALIDACIÓN        │  │  PROMPT 3: COMPLEMENTARIAS   │
│  Input: Banco + Documento    │  │  Input: Banco + Informes +   │
│  Output: Diagnóstico         │  │         Documento            │
│          + Fiabilidad        │  │  Output: +5-30 preguntas     │
│          + Recomendaciones   │  │          + Informe expansión │
│  Características: Sin correc-│  │  Características: Evita dupl,│
│  ción, solo análisis         │  │  complementa cobertura/prof  │
└──────────────┬───────────────┘  └──────────────┬───────────────┘
               │                                  │
               └──────────────┬───────────────────┘
                              │
                              ▼
                    ┌─────────────────────┐
                    │  Iteración opcional │
                    │  (máx 3 ciclos)     │
                    │  Prompt 3 → Prompt 2│
                    └─────────────────────┘
```

%%
# GENERADOR DE PREGUNTAS COMPLEMENTARIAS — EXPANSIÓN Y PROFUNDIZACIÓN

## OBJETIVO

Generar preguntas adicionales sobre documento previamente procesado, utilizando banco existente e informes técnicos para evitar duplicación y complementar cobertura/profundidad.

---

## ENTRADA REQUERIDA

```
1. Documento original
2. Banco de preguntas existente (tabla markdown completa)
3. Informe técnico del generador (conceptos extraídos, NO_CUBIERTOS, distribuciones)
4. (Opcional) Informe de validación-fiabilidad (problemas detectados, fiabilidad por pregunta)
5. Configuración de usuario:
   - N_adicionales: [número de preguntas a generar, 10-30]
   - Modo: [COBERTURA / PROFUNDIZACIÓN / BALANCE / MIXTO]
   - Bloom_target: [si modo BALANCE, especificar nivel a reforzar]
   - Nivel_dificultad: [MANTENER / AUMENTAR]
```

---

## MODO DE OPERACIÓN

Usuario especifica mediante:

```
Modo = COBERTURA         → Priorizar conceptos NO_CUBIERTOS del informe
Modo = PROFUNDIZACIÓN    → Generar preguntas más específicas/difíciles sobre conceptos ya evaluados
Modo = BALANCE          → Equilibrar distribución Bloom o Cuotas según informe
Modo = MIXTO            → Combinación: 50% cobertura + 30% profundización + 20% balance
```

**Por defecto (si no especificado):** MIXTO

---

## FASE 1: ANÁLISIS DE BANCO EXISTENTE

### A. Mapeo de cobertura conceptual

**Del informe técnico extraer:**

```
Conceptos_CUBIERTOS = lista de conceptos con ≥1 pregunta
Conceptos_NO_CUBIERTOS = lista del informe con razones
Conceptos_SUBCUBIERTOS = conceptos ESENCIAL/TRONCAL con <2 preguntas
```

**Del banco existente analizar:**

```
Por cada concepto cubierto:
  - Número de preguntas asignadas
  - Niveles Bloom utilizados
  - Facetas evaluadas (extraer de enunciados)
  - Fiabilidad promedio (si informe validación disponible)
```

### B. Detección de espacios vacíos

**Espacios de contenido:**

```
Conceptos sin preguntas: [lista NO_CUBIERTOS]
Conceptos con 1 sola pregunta: [lista con clasificación ESENCIAL/TRONCAL]
Matices no evaluados: [identificar en documento aspectos de conceptos cubiertos no preguntados]
```

**Espacios de profundidad:**

```
Conceptos evaluados solo con Recordar: [lista candidatos para Comprender/Aplicar]
Conceptos sin casos aplicados: [lista candidatos para Aplicar]
Datos específicos no evaluados: [dosis, timings, criterios numéricos mencionados en documento]
```

**Espacios de distribución:**

```
Déficit Bloom: [nivel con % real < target - tolerancia]
Déficit Cuota: [nivel ESENCIAL/TRONCAL/SUBIDEA con % < esperado]
```

---

## FASE 2: ESTRATEGIA DE GENERACIÓN POR MODO

### MODO COBERTURA

**Prioridad 1:** Conceptos NO_CUBIERTOS con clasificación ESENCIAL/TRONCAL

```
Asignar: 
  - ESENCIAL no cubierto → 2-3 preguntas
  - TRONCAL no cubierto → 1-2 preguntas
  - SUBIDEA no cubierto → 1 pregunta (si espacio)
```

**Prioridad 2:** Conceptos SUBCUBIERTOS (ESENCIAL/TRONCAL con 1 pregunta)

```
Asignar +1 pregunta con Bloom distinto al usado
```

**Distribución de N_adicionales:**

```
60% → NO_CUBIERTOS
30% → SUBCUBIERTOS
10% → Matices no evaluados de conceptos cubiertos
```

---

### MODO PROFUNDIZACIÓN

**Criterio:** Generar preguntas más específicas/difíciles sobre conceptos YA evaluados

**Estrategias de profundización:**

1. **Escalado Bloom:**

```
Si concepto tiene pregunta Recordar → generar Comprender o Aplicar
Si concepto tiene Comprender → generar Aplicar con caso clínico
Si concepto tiene Aplicar → generar Analizar con causas, consecuencias, diferenciar, organizar, inferir, atribuir
```

2. **Incremento de especificidad:**

```
Si pregunta existente es general (ej: "mecanismo de X") 
→ Generar específica (ej: "mecanismo de X en contexto Y específico")

Si pregunta existente sobre concepto amplio
→ Generar sobre subproceso o fase específica
```

3. **Aumento de dificultad de distractores:**

```
Si pregunta existente tiene similitud_distractores <0.50
→ Generar nueva con similitud 0.48-0.52 sobre mismo concepto pero distinta faceta
```

4. **Integración conceptual:**

```
Si conceptos A y B evaluados por separado
→ Generar pregunta que integre relación A-B
```

**Distribución de N_adicionales:**

```
40% → Escalado Bloom
30% → Especificidad aumentada
20% → Distractores más difíciles
10% → Integración conceptual
```

---

### MODO BALANCE

**Objetivo:** Corregir desbalances detectados en informe

**Balance Bloom:**

```
Si déficit en nivel X (ej: Comprender real 25%, target 30%±5):
  Calcular: preguntas_necesarias = (target% - real%) × total_preguntas_existentes / 100
  Generar: preguntas_necesarias de nivel X sobre conceptos ya cubiertos
```

**Balance Cuota:**

```
Si déficit ESENCIAL (ej: real 40%, esperado 45%±3):
  Identificar conceptos ESENCIAL con ≤2 preguntas
  Generar preguntas adicionales hasta alcanzar ~45%
```

**Distribución de N_adicionales:**

```
70% → Nivel/Cuota con mayor déficit
30% → Nivel/Cuota con segundo mayor déficit
```

---

### MODO MIXTO (predeterminado)

```
50% N_adicionales → COBERTURA (NO_CUBIERTOS prioritarios)
30% N_adicionales → PROFUNDIZACIÓN (escalado Bloom + especificidad)
20% N_adicionales → BALANCE (nivel Bloom o Cuota con mayor déficit)

Distribución automática según informe:
  Si NO_CUBIERTOS >10 conceptos → aumentar COBERTURA a 60%, reducir PROFUNDIZACIÓN a 20%
  Si fiabilidad_banco <0.75 → aumentar BALANCE a 30% (generar preguntas de calidad superior)
  Si distribución Bloom delta >10% → aumentar BALANCE a 35%, reducir PROFUNDIZACIÓN a 15%
```

---

## FASE 3: GENERACIÓN DE PREGUNTAS COMPLEMENTARIAS

### Parámetros heredados del generador principal

```
Usar TODOS los parámetros del prompt principal:
- num_options = 3
- plausibilidad_distractores = 0.48-0.52 (o 0.45-0.55 si nivel_dificultad=MANTENER y fiabilidad<0.75)
- longitud_opciones = ±10%
- Anti-sesgos completo
- Sistema de confianza (Relevancia + Cita + Distractores + Fidelidad)
- Formato tabla markdown idéntico
```

### Restricciones adicionales para evitar duplicación

**CRÍTICO — Verificar contra banco existente:**

```
Por cada pregunta en borrador:
  1. Extraer conceptos clave evaluados
  2. Comparar con conceptos de preguntas existentes
  3. Si similitud_conceptual >0.70:
     → Verificar si evalúa faceta distinta
     → Si evalúa misma faceta → DESCARTAR, generar alternativa
  4. Comparar respuesta correcta con respuestas existentes
  5. Si similitud_respuesta >0.60:
     → DESCARTAR, reformular enfocando matiz distinto
```

**Registro de no-duplicación:** Documentar en informe: "Preguntas descartadas por duplicación [N], regeneradas enfocando [facetas alternativas especificadas]"

---

### Ajuste de nivel de dificultad

**Si nivel_dificultad = AUMENTAR:**

```
- Forzar similitud_distractores a 0.48-0.52 (sin tolerancia ampliada)
- Priorizar preguntas Comprender/Aplicar sobre Recordar
- Incluir datos numéricos específicos (dosis, porcentajes, timings) si presentes en documento
- Generar preguntas que integren 2+ conceptos relacionados
```

**Si nivel_dificultad = MANTENER:**

```
- Usar similitud_distractores según fiabilidad banco existente
- Respetar distribución Bloom del banco original
- Mantener nivel de especificidad promedio del banco
```

---

## FASE 4: INTEGRACIÓN Y VALIDACIÓN

### Numeración continua

```
Primera pregunta complementaria = max(Nº_banco_existente) + 1
Mantener numeración secuencial
```

### Verificación de objetivos cumplidos

**Según modo, validar:**

COBERTURA:

```
✓ Conceptos NO_CUBIERTOS residuales ≤30% de NO_CUBIERTOS iniciales
✓ Conceptos ESENCIAL todos con ≥2 preguntas
✓ Conceptos TRONCAL prioritarios con ≥1 pregunta
```

PROFUNDIZACIÓN:

```
✓ ≥40% preguntas nuevas tienen Bloom superior a preguntas existentes sobre mismo concepto
✓ ≥30% preguntas nuevas tienen especificidad aumentada verificable
✓ Similitud_distractores promedio ≥ similitud_distractores banco existente
```

BALANCE:

```
✓ Distribución Bloom combinada (existente + nuevas) dentro de tolerancia target±5%
✓ Distribución Cuota combinada dentro de rangos esperados
```

MIXTO:

```
✓ Combinación de criterios anteriores según proporciones
```

### Recálculo de métricas globales

```
Banco_expandido = banco_existente + preguntas_complementarias

Calcular:
- Total preguntas: [N_existentes + N_complementarias]
- Distribución Bloom nueva: [R%, C%, A%, A%,]
- Distribución Cuota nueva: [E%, T%, S%, M%]
- Confianza promedio nueva: [0.XX]
- Conceptos NO_CUBIERTOS residuales: [lista reducida]
- Cobertura ESENCIAL: [%] con ≥2 preguntas
```

---

## FORMATO DE SALIDA

### 1. Informe de Generación Complementaria (máx 300 palabras, separado `;`)

**Entrada:** banco previo [N preguntas]; conceptos NO_CUBIERTOS [N]; conceptos SUBCUBIERTOS [N]; déficit Bloom [nivel, delta%]; déficit Cuota [nivel, delta%]; fiabilidad banco [0.XX si disponible]

**Configuración:** N_adicionales [N]; modo [COBERTURA/PROFUNDIZACIÓN/BALANCE/MIXTO]; nivel_dificultad [MANTENER/AUMENTAR]; Bloom_target [si aplica]

**Estrategia aplicada:** [según modo, detallar % asignado a cada tipo]; distribución real [especificar N preguntas por tipo]

**Generadas:** total [N]; numeración [desde X hasta Y]

**Por tipo de complemento:**

- NO_CUBIERTOS: [N preguntas]; conceptos cubiertos [lista nombres]
- SUBCUBIERTOS: [N preguntas]; conceptos reforzados [lista]
- Profundización: [N]; escalado Bloom [N], especificidad [N], integración [N]
- Balance: [N]; nivel reforzado [Bloom o Cuota especificado]

**Anti-duplicación:** borradores descartados [N]; razón [similitud conceptual >0.70]; regenerados enfocando [facetas alternativas]

**Métricas banco expandido:** total [N_prev + N_nuevas]; Bloom [R%, C%, A%, A%] (delta vs target [máx%]); Cuota [E%, T%, S%, M%]; confianza promedio [0.XX] (vs prev [0.XX]); NO_CUBIERTOS residuales [N, lista nombres]

**Cobertura ESENCIAL expandida:** [N/N_total] conceptos con ≥2 preguntas ([%]); mejora vs previo [+X preguntas en Y conceptos]

**Objetivo cumplido:** [SÍ/PARCIAL/NO]; justificación [si PARCIAL/NO explicar razón]

**Recomendación:** [siguiente iteración necesaria si NO_CUBIERTOS >30% residuales / banco completo si cobertura satisfactoria / balance adicional si déficit Bloom/Cuota persiste]

---

### 2. Tabla Markdown de Preguntas Complementarias

**Formato idéntico al generador principal**


**Numeración:** Continua desde banco existente (si banco tenía 42 preguntas, primera complementaria es Nº 43)

---

### 3. Resumen de Integración

```
**BANCO ORIGINAL:**
Preguntas: [N]
Bloom: Recordar [N, %], Comprender [N, %], Aplicar [N, %], Analizar [N, %]
Cuota: ESENCIAL [%], TRONCAL [%], SUBIDEA [%], MENOR [%]
NO_CUBIERTOS: [N conceptos]

**PREGUNTAS COMPLEMENTARIAS:**
Añadidas: [N]
Bloom: Recordar [N, %], Comprender [N, %], Aplicar [N, %], Analizar [N, %]
Tipo: NO_CUBIERTOS [N], SUBCUBIERTOS [N], Profundización [N], Balance [N]

**BANCO EXPANDIDO (ORIGINAL + COMPLEMENTARIAS):**
Total: [N]
Bloom: Recordar [N, %], Comprender [N, %], Aplicar [N, %], Analizar [N, %]
  → Delta vs target: [máx %] [DENTRO/FUERA] de tolerancia
Cuota: ESENCIAL [%], TRONCAL [%], SUBIDEA [%], MENOR [%]
  → Evaluación: [CUMPLE/INCUMPLE] rangos esperados
Confianza promedio: [0.XX] (cambio [+/-0.XX])
NO_CUBIERTOS residuales: [N] (reducción [%] vs original)
Cobertura ESENCIAL: [%] conceptos con ≥2 preguntas (mejora [+X%])

**ESTADO FINAL:**
[COMPLETO - cobertura satisfactoria, distribuciones balanceadas]
[MEJORADO - cobertura aumentada, considerar iteración adicional para [especificar qué]]
[INSUFICIENTE - persisten [especificar problemas], requiere nueva iteración con ajuste [especificar]]
```

---

## CONFIGURACIÓN TÍPICA POR ESCENARIO

### Escenario 1: Banco inicial con baja cobertura

```
Usuario ve informe con NO_CUBIERTOS: 15 conceptos (8 ESENCIAL, 7 TRONCAL)

Configuración recomendada:
  N_adicionales = 20
  Modo = COBERTURA
  Nivel_dificultad = MANTENER
  
Resultado esperado: NO_CUBIERTOS reducidos a ≤5 conceptos
```

### Escenario 2: Banco inicial completo pero superficial

```
Usuario ve informe con cobertura 100% pero todas preguntas Recordar

Configuración recomendada:
  N_adicionales = 15
  Modo = PROFUNDIZACIÓN
  Nivel_dificultad = AUMENTAR
  
Resultado esperado: 50% preguntas banco expandido con Comprender/Aplicar/Analizar
```

### Escenario 3: Banco desbalanceado en Bloom

```
Usuario ve informe con Bloom desbalanceado (target **Distribución Bloom configurable por usuario**)

Configuración recomendada:
  N_adicionales = 10
  Modo = BALANCE
  Bloom_target = Comprender
  Nivel_dificultad = MANTENER
  
Resultado esperado: Bloom dentro de tolerancia en banco expandido
```

### Escenario 4: Primera expansión general

```
Usuario quiere ampliar banco sin diagnóstico previo específico

Configuración recomendada:
  N_adicionales = 20
  Modo = MIXTO
  Nivel_dificultad = MANTENER
  
Resultado esperado: Balance entre cobertura, profundidad y distribuciones
```

---

## LÍMITES Y RESTRICCIONES

**Iteraciones:**

```
Máximo 3 iteraciones de expansión sobre mismo documento
Razón: a partir de 3ª iteración, probabilidad de duplicación aumenta exponencialmente
Si tras 3 iteraciones persisten NO_CUBIERTOS críticos → problema en documento (info insuficiente)
```

**N_adicionales:**

```
Mínimo: 5 preguntas
Máximo: 30 preguntas por iteración
Total banco (original + todas expansiones): máximo 100 preguntas
Razón: >100 preguntas sobre un documento sugiere sobre-explotación, generar banco nuevo con distinto material
```

**Confianza mínima:**

```
Preguntas complementarias deben cumplir umbral confianza ≥0.60
Si imposible generar con confianza ≥0.60 → marcar INCOMPLETO, documentar razón, no incluir en banco
```

---
%%
## INSTRUCCIONES DE USO

**Comando básico:**

```
Generar 15 preguntas complementarias en modo MIXTO
```

**Comando avanzado:**

```
Generar 20 preguntas en modo COBERTURA, nivel_dificultad AUMENTAR
Priorizar conceptos NO_CUBIERTOS ESENCIAL del informe
```

**Comando específico de balance:**

```
Generar 10 preguntas en modo BALANCE
Bloom_target = Aplicar (reforzar hasta alcanzar 45%)
```

**Workflow completo recomendado:**

```
1. Generar banco inicial con prompt principal → banco_v1 + informe_v1
2. Ejecutar validación-fiabilidad → diagnóstico_v1
3. Si NO_CUBIERTOS críticos → generar complementarias modo COBERTURA → banco_v2
4. (Opcional) Ejecutar validación-fiabilidad sobre banco_v2 → diagnóstico_v2
5. Si profundización deseada → generar complementarias modo PROFUNDIZACIÓN → banco_v3
6. Banco final para uso/exportación a Anki
```
%%

---

**FIN PROMPT GENERACIÓN COMPLEMENTARIA**

