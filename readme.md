# Taller 4 — Tarea Go/No-Go (Solución)

- **Asignatura:** Investigación e Intervención desde las Neurociencias Aplicadas  
- **Ciclo:** 2026-1  
- **Autor:** Marcelo Landa

---

## Descripción

Este repositorio contiene la **solución completa** del Taller 4 del curso, centrado en el diseño e implementación de la tarea Go/No-Go en PsychoPy Builder. La tarea Go/No-Go es un paradigma de control inhibitorio en el que el participante debe responder ante un estímulo frecuente (Go) e inhibir su respuesta ante un estímulo infrecuente (No-Go). La alta proporción de trials Go genera una tendencia prepotente a responder, lo que hace que la inhibición en los trials No-Go sea cognitivamente demandante. El control inhibitorio es una de las tres funciones ejecutivas centrales revisadas por Diamond (2013), y constituye la función directamente medida por este paradigma. Wessel (2018) es la referencia metodológica utilizada en el taller.

> **Nota:** Este repositorio se publica al finalizar el taller como material de referencia.

### Actualización para la clase

Se incluyó una modificación del experimento con **bucle anidado** a manera de repaso para el examen. Esta versión permite revisar el uso de un bucle externo de bloques y un bucle interno de condiciones/trials dentro de PsychoPy Builder.

Además, se agregó un caso redactado tipo simulación en el archivo `SOLUCION_GO_NO_GO_TASK.pdf`, ya solucionado e incluido con capturas de pantalla del proceso. Para esta modificación se añadieron los estímulos `O.png` y `Q.png` en formato PNG, junto con dos archivos nuevos en `conditions/`: `bloques.xlsx` para el bucle externo y `condiciones_experimental_2.xlsx` como hoja de condiciones para los estímulos O y Q.

---

## Cómo descargar los materiales

1. Hacer clic en el botón verde **`< > Code`** en la parte superior de esta página
2. Seleccionar **`Download ZIP`**
3. Descomprimir el archivo en tu computadora
4. Abrir PsychoPy Builder y cargar el archivo `gonogo_task.psyexp`

> **Importante:** No mover ni renombrar ninguna carpeta. PsychoPy necesita encontrar los archivos en las rutas `media/stim/` e `media/instructions/` exactamente como están configurados.

---

## Estructura del repositorio

```
solucion_taller_4/
│
├── gonogo_task.psyexp                  # Archivo del experimento completo
├── SOLUCION_GO_NO_GO_TASK.pdf          # Caso tipo simulación resuelto con capturas
│
├── conditions/
│   ├── bloques.xlsx                    # Archivo para el bucle externo del repaso
│   ├── condiciones_practica.xlsx       # Trials de práctica
│   ├── condiciones_experimental.xlsx   # Condiciones experimentales originales
│   └── condiciones_experimental_2.xlsx # Condiciones para estímulos O y Q
│
├── media/
│   ├── instructions/
│   │   ├── 1_instrucciones.png
│   │   ├── 2_instrucciones_practica.png
│   │   ├── 3_fin_practica.png
│   │   ├── 4_descanso.png
│   │   ├── 5_instrucciones_experimental.png
│   │   └── 6_fin_experimento.png
│   └── stim/
│       ├── go.png                     # Círculo verde (#27AE60) — 500×500 px
│       ├── nogo.png                   # Cuadrado rojo (#C0392B) — 500×500 px
│       ├── O.png                      # Estímulo agregado para bucle anidado
│       └── Q.png                      # Estímulo agregado para bucle anidado
│
├── docs/
│   ├── instrucciones_gonogo.docx      # Instrucciones por pantalla con notas técnicas
│   └── guia_materiales_taller4.docx  # Guía completa de materiales del taller
│
└── README.md
```

---

## Diseño de la tarea

| Parámetro | Valor |
|---|---|
| Paradigma | Go/No-Go — control inhibitorio |
| Versión | Versión 4 de Wessel (2018): rare No-Go + ritmo rápido |
| Estímulo Go | Círculo verde (#27AE60) — 500×500 px, fondo negro |
| Estímulo No-Go | Cuadrado rojo (#C0392B) — 500×500 px, fondo negro |
| Estímulos agregados para repaso | `O.png` y `Q.png`, usados en la modificación con bucle anidado |
| Tecla de respuesta (Go) | Barra espaciadora (solo en trials Go) |
| Proporción | 80% Go / 20% No-Go |
| Trials de práctica | 20 (16 Go + 4 No-Go) con feedback |
| Trials experimentales | 80 (64 Go + 16 No-Go) sin feedback |
| Bucle anidado de repaso | Bucle externo controlado por `bloques.xlsx` y bucle interno controlado por las hojas de condiciones |
| Duración del estímulo | Hasta 1000 ms o hasta respuesta |
| Duración del trial (ITI incluido) | 1500 ms |
| Variables de la tarea | RT en Go (ms), errores de comisión (%), errores de omisión (%) |

### Flujo de la tarea

```mermaid
flowchart TD
    subgraph SB[BIENVENIDA]
        A[instrucciones_generales]
    end

    subgraph SP[PRÁCTICA]
        B[instrucciones_practica]
        B --> LP
        subgraph LP["loop_practica x 20"]
            P1[estimulo] --> PITI[ITI] --> P2[feedback]
        end
        LP --> C[fin_practica]
    end

    subgraph SD[DESCANSO]
        D[descanso 10 s]
    end

    subgraph SE[EXPERIMENTAL]
        E[instrucciones_experimental]
        E --> LB
        subgraph LB["loop_bloques"]
            direction TB
            subgraph LE["loop_experimental / loop_condiciones"]
                E1[estimulo] --> EITI[ITI]
            end
        end
    end

    subgraph SC[CIERRE]
        F[fin_experimento]
    end

    A --> B
    C --> D
    D --> E
    LB --> F

    style A    fill:#6BA3D6,stroke:#2E6DA4,color:#0D2E4A
    style F    fill:#E07B6A,stroke:#A83D2B,color:#3D0E07
    style D    fill:#FAC775,stroke:#BA7517,color:#412402

    style B    fill:#AFA9EC,stroke:#534AB7,color:#26215C
    style C    fill:#AFA9EC,stroke:#534AB7,color:#26215C
    style P1   fill:#AFA9EC,stroke:#534AB7,color:#26215C
    style PITI fill:#D8D5FA,stroke:#534AB7,color:#26215C
    style P2   fill:#AFA9EC,stroke:#534AB7,color:#26215C
    style LP   fill:#EEEDFE,stroke:#534AB7,color:#26215C

    style E    fill:#5DCAA5,stroke:#0F6E56,color:#04342C
    style E1   fill:#5DCAA5,stroke:#0F6E56,color:#04342C
    style EITI fill:#C9EFE3,stroke:#0F6E56,color:#04342C
    style LE   fill:#E1F5EE,stroke:#0F6E56,color:#04342C
    style LB   fill:#D7F2EA,stroke:#0F6E56,color:#04342C

    style SB   fill:#EBF3FA,stroke:#2E6DA4,color:#0D2E4A
    style SP   fill:#F8F7FF,stroke:#534AB7,color:#26215C
    style SD   fill:#FEF9EE,stroke:#BA7517,color:#412402
    style SE   fill:#F0FAF6,stroke:#0F6E56,color:#04342C
    style SC   fill:#FAEAE7,stroke:#A83D2B,color:#3D0E07
```

---

## Análisis de resultados

Una vez completada la tarea, el CSV generado por PsychoPy puede procesarse en la aplicación web del curso:

**🔗 [mlandaf.github.io/taller_4_resultados](https://mlandaf.github.io/taller_4_resultados)**

No se requiere instalar ningún software adicional. Solo arrastra tu archivo CSV al navegador.

---

## Referencias

Diamond, A. (2013). Executive functions. *Annual Review of Psychology, 64*, 135–168. https://doi.org/10.1146/annurev-psych-113011-143750

Wessel, J. R. (2018). Prepotent motor activity and inhibitory control demands in different variants of the go/no-go paradigm. *Psychophysiology, 55*(3), e12871. https://doi.org/10.1111/psyp.12871

Peirce, J. W., Gray, J. R., Simpson, S., MacAskill, M. R., Hochenberger, R., Sogo, H., Kastman, E., & Lindelov, J. (2019). PsychoPy2: Experiments in behavior made easy. *Behavior Research Methods*. https://doi.org/10.3758/s13428-018-01193-y

---

## Contacto

**Marcelo Landa**  
Asistente de cátedra — Investigación e Intervención desde las Neurociencias Aplicadas  
Universidad de Lima
