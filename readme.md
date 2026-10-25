# Taller 4 - Tarea Go/No-Go (Solución)

- **Asignatura:** Investigación e Intervención desde las Neurociencias Aplicadas
- **Ciclo:** 2026-2
- **Autor:** Marcelo Landa

---

## Descripción

Este repositorio contiene el **solucionario del Taller 4**, centrado en el diseño e implementación de una tarea Go/No-Go en PsychoPy Builder. El participante debe presionar la **barra espaciadora ante un círculo verde (Go)** y **no presionar ninguna tecla ante un cuadrado rojo (No-Go)**. La mayor frecuencia de ensayos Go favorece una tendencia a responder que debe inhibirse ante el estímulo No-Go.

La tarea incluye una fase de práctica con feedback y una fase experimental sin feedback. El control inhibitorio se aborda a partir de Diamond (2013), y Wessel (2018) se utiliza como referencia metodológica.

Young et al. (2018) examinan cómo la proporción Go/No-Go y el intervalo entre ensayos influyen en los errores de comisión. Hasegawa et al. (2021) analizan cómo los intervalos entre estímulos afectan la interpretación de esos errores como indicador de inhibición de respuestas. Los parámetros que se describen aquí corresponden a la adaptación didáctica del curso.

### Secuencia del ensayo de práctica

![Secuencia del ensayo de práctica: estímulo Go y feedback](media/SECUENCIA_ENSAYO_PRACTICA.png)

**Estímulo y respuesta (hasta 1000 ms) → Feedback (1000 ms).**

### Secuencia del ensayo experimental

![Secuencia del ensayo experimental: estímulo No-Go e intervalo en negro](media/SECUENCIA_ENSAYO_EXPERIMENTAL.png)

**Estímulo y respuesta (hasta 1000 ms) → ITI en negro (500 ms).**

La primera pulsación de la barra espaciadora termina la presentación del estímulo y da paso al feedback o al ITI. Si no hay respuesta, el estímulo permanece hasta completar los 1000 ms. Esto también se aplica a los ensayos No-Go: no responder es correcto; pulsar espacio constituye un error y termina la presentación. La tarea no utiliza cruz de fijación.

---

## Cómo descargar los materiales

1. Hacer clic en el botón verde **`< > Code`** en la parte superior de esta página.
2. Seleccionar **`Download ZIP`**.
3. Descomprimir el archivo en tu computadora.
4. Abrir PsychoPy Builder y cargar `gonogo_task.psyexp`.
5. Consultar `docs/instrucciones_gonogo.docx` para revisar los textos, tiempos y forma de avance de las pantallas.

> **Importante:** Conservar los nombres de los archivos y la estructura de carpetas. PsychoPy utiliza rutas relativas para encontrar las hojas de condiciones y las imágenes de los estímulos.

---

## Estructura del repositorio

```text
solucion_taller_4/
├── gonogo_task.psyexp                       # Experimento en PsychoPy Builder
├── conditions/
│   ├── condiciones_practica.xlsx            # 20 ensayos: 16 Go y 4 No-Go
│   └── condiciones_experimental.xlsx        # 80 ensayos: 64 Go y 16 No-Go
├── docs/
│   └── instrucciones_gonogo.docx            # Textos y tiempos de las pantallas
├── media/
│   ├── FLUJO_TAREA_GO_NO_GO.png             # Diagrama del flujo completo
│   ├── SECUENCIA_ENSAYO_PRACTICA.png
│   ├── SECUENCIA_ENSAYO_EXPERIMENTAL.png
│   └── stim/
│       ├── go.png                          # Círculo verde, 500 × 500 px
│       └── nogo.png                        # Cuadrado rojo, 500 × 500 px
├── .gitignore
└── readme.md
```

Al ejecutar la tarea, PsychoPy genera la carpeta `data/` con los archivos de resultados. Las instrucciones y demás pantallas se presentan mediante componentes Text.

---

## Diseño de la tarea

| Parámetro | Valor |
|---|---|
| Paradigma | Go/No-Go con figuras |
| Fondo | Negro |
| Instrucciones, feedback y demás textos | Color blanco; tamaño de letra 0.05 |
| Estímulo Go | Círculo verde, centrado; imagen en su tamaño original |
| Estímulo No-Go | Cuadrado rojo, centrado; imagen en su tamaño original |
| Respuesta Go | Presionar la barra espaciadora lo más rápido posible |
| Respuesta No-Go | No presionar ninguna tecla |
| Ensayos de práctica | 20: 16 Go y 4 No-Go, con feedback |
| Ensayos experimentales | 80: 64 Go y 16 No-Go, sin feedback |
| Proporción | 80 % Go y 20 % No-Go en ambas fases |
| Estímulo y ventana de respuesta | Comienzan al mismo tiempo y duran hasta 1000 ms; una pulsación termina la rutina |
| Feedback | 1000 ms, únicamente en práctica |
| ITI | 500 ms de pantalla negra, únicamente en la fase experimental |
| Cruz de fijación | No se utiliza |
| Fin de práctica | 5 s |
| Descanso | 60 s entre la práctica y la fase experimental |
| Cierre | 5 s |

> **Orden de las condiciones:** `loop_practica` y `loop_experimental` presentan las filas en orden **aleatorio**, con `nReps = 1`. Cada fila de la hoja de condiciones corresponde a un ensayo.

### Feedback de práctica

| Estímulo | Acción del participante | Feedback |
|---|---|---|
| Círculo verde (Go) | Presiona espacio | ¡Correcto! |
| Círculo verde (Go) | No presiona | Incorrecto |
| Cuadrado rojo (No-Go) | No presiona | ¡Correcto! |
| Cuadrado rojo (No-Go) | Presiona espacio | Incorrecto |

### Flujo de la tarea

```mermaid
flowchart LR
    subgraph SB["BIENVENIDA"]
        A["instrucciones"]
    end

    subgraph SP["PRÁCTICA"]
        direction LR
        B["instrucciones_practica"]
        subgraph LP["loop_practica · 20 ensayos"]
            direction LR
            P1["estimulo_practica<br/>Hasta 1000 ms"]
            P2["feedback_practica<br/>1000 ms"]
            P1 --> P2
        end
        C["fin_practica<br/>5 s"]
        B --> LP
        LP --> C
    end

    subgraph SD["DESCANSO"]
        D["descanso<br/>60 s"]
    end

    subgraph SE["EXPERIMENTAL"]
        direction LR
        E["instrucciones_experimental"]
        subgraph LE["loop_experimental · 80 ensayos"]
            direction LR
            E1["estimulo_experimental<br/>Hasta 1000 ms"]
            E2["ITI<br/>500 ms"]
            E1 --> E2
        end
        E --> LE
    end

    subgraph SC["CIERRE"]
        F["fin_experimento<br/>5 s"]
    end

    A --> B
    C --> D
    D --> E
    LE --> F

    classDef bienvenida fill:#6BA3D6,stroke:#2E6DA4,color:#0D2E4A
    classDef practica fill:#AFA9EC,stroke:#534AB7,color:#26215C
    classDef experimental fill:#5DCAA5,stroke:#0F6E56,color:#04342C
    classDef descanso fill:#FAC775,stroke:#BA7517,color:#412402
    classDef cierre fill:#E07B6A,stroke:#A83D2B,color:#3D0E07

    class A bienvenida
    class B,P1,P2,C practica
    class E,E1,E2 experimental
    class D descanso
    class F cierre

    style SB fill:#EBF3FA,stroke:#2E6DA4,color:#0D2E4A
    style SP fill:#F8F7FF,stroke:#534AB7,color:#26215C
    style LP fill:#EEEDFE,stroke:#534AB7,color:#26215C
    style SD fill:#FEF9EE,stroke:#BA7517,color:#412402
    style SE fill:#F0FAF6,stroke:#0F6E56,color:#04342C
    style LE fill:#E1F5EE,stroke:#0F6E56,color:#04342C
    style SC fill:#FAEAE7,stroke:#A83D2B,color:#3D0E07
```

La tarea se organiza en cinco fases: **bienvenida, práctica, descanso, experimental y cierre**. La práctica y la fase experimental utilizan un loop cada una.

El diagrama también está disponible como [imagen del flujo completo](media/FLUJO_TAREA_GO_NO_GO.png).

---

## Análisis de resultados

Una vez completada la tarea, el CSV generado por PsychoPy en `data/` puede procesarse en la aplicación web del curso:

**https://mlandaf.github.io/taller_4_resultados**

Carga tu archivo CSV en el navegador y confirma las columnas de la fase experimental: `es_go`, `tipo`, `key_resp_experimental.corr`, `key_resp_experimental.keys` y `key_resp_experimental.rt`.

| Medida | Qué describe |
|---|---|
| Precisión global (%) | Proporción de ensayos experimentales correctos, tanto Go como No-Go |
| RT promedio Go (ms) | Tiempo de respuesta promedio de los ensayos Go correctos |
| Errores de comisión (%) | Proporción de ensayos No-Go en los que se presionó espacio |
| Errores de omisión (%) | Proporción de ensayos Go en los que no se respondió |

Selecciona las columnas experimentales, sin el sufijo `_p` que identifica las condiciones de práctica. La ausencia de respuesta es correcta en No-Go y constituye una omisión en Go. El RT se interpreta **junto con la precisión y los errores**.

---

## Referencias

Diamond, A. (2013). Executive functions. *Annual Review of Psychology, 64*, 135–168. https://doi.org/10.1146/annurev-psych-113011-143750

Hasegawa, A., Matsumoto, N., Yamashita, Y., Tanaka, K., Kawaguchi, J., & Yamamoto, T. (2021). Do shorter inter-stimulus intervals in the go/no-go task enable better assessment of response inhibition? *Scandinavian Journal of Psychology, 62*(2), 118–124. https://doi.org/10.1111/sjop.12679

Peirce, J., Gray, J. R., Simpson, S., MacAskill, M., Höchenberger, R., Sogo, H., Kastman, E., & Lindeløv, J. K. (2019). PsychoPy2: Experiments in behavior made easy. *Behavior Research Methods, 51*(1), 195–203. https://doi.org/10.3758/s13428-018-01193-y

Wessel, J. R. (2018). Prepotent motor activity and inhibitory control demands in different variants of the go/no-go paradigm. *Psychophysiology, 55*(3), e12871. https://doi.org/10.1111/psyp.12871

Young, M. E., Sutherland, S. C., & McCoy, A. W. (2018). Optimal go/no-go ratios to maximize false alarms. *Behavior Research Methods, 50*(3), 1020–1029. https://doi.org/10.3758/s13428-017-0923-5

---

## Contacto

**Marcelo Landa**

Asistente de cátedra - Investigación e Intervención desde las Neurociencias Aplicadas

Universidad de Lima
