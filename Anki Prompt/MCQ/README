# Teoría 
- Prompt Alpha
  - Ventajas
    - ↑Velocidad
    - Prguntas más variadas
    - Mayor cantidad de pregutas en menos tiempo 
  - Para máximo partido requiere integrar con 2 prompts del DLC
  - Desventajas 
    - Limitaciones en preguntas → mucha generabilidad o mucha especificidad (no es progresivo), Blueprint casi-aleatorio y sin personalización 
    - No busca tener una estructura similar a un exámen

- Prompts HyperFlow, Expansion y Forecast-Zero 
  - Forman parte de una misma estrategia personalizada de estudio, dividida por sectores 
  - Requiere de más tiempo y elabora menos preguntas, pero permite practicar familias de ítems, permitiendo estudiar más ítems (pero en más tiempo)
  - Ventajas
    - Personalización → Va aumentando la carga cognitiva intrínseca progresivamente (HyperFlow muy fácil y Forecast-Zero es lo más parecido a un exámen complicado) 
    - Realidad → Pretende la imitación de un exámen real (Forecast-Zero)
    - ↓Probabilidad de fallar en un exámen de verdad 
  - Desventajas 
    - ↑Tiempo
    - ↓Automatización

# ARQUITECTURA COMPLETA DE 3 PROMPTS para Alpha

```
┌─────────────────────────────────────────┐
│  PROMPT 1: GENERADOR PRINCIPAL: Alpha   │
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
**Validación** y **Complementarias** en los DLC's

# Arquitectura para Estrategia de Aprendizaje personalizado
```
┌──────────────────────────────────────────────┐
│  HyperFlow: PRECISIÓN Y VELOCIDAD            │
│  Función: Respuesta rápida, control fino     │
│           y ejecución optimizada             │
└───────────────────────┬──────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────┐
│  Expansion: TRANSFERENCIA Y ROBUSTEZ         │                          
│  Función: Generalización, resistencia        │
│           al ruido y adaptación              │
└───────────────────────┬──────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────────┐
│  Forecast-Zero: ESTABILIDAD Y PREDICCIÓN     │
│  Función: Consistencia, anticipación         │
│           y control predictivo               │
└──────────────────────────────────────────────┘
```
