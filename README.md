# Práctica 4 — Modelado y control del sistema endócrino

Repositorio académico de la asignatura **Modelado de Sistemas Fisiológicos**, correspondiente al programa de **Ingeniería Biomédica** del Tecnológico Nacional de México / Instituto Tecnológico de Tijuana. Esta práctica integra el modelado fenomenológico mediante analogías eléctricas de segundo orden, análisis temporal y de estabilidad, comparación entre condiciones fisiológicas normales y alteradas, diseño de controladores PID, simulación computacional y comprobación experimental en laboratorio.

El caso de estudio representa los procesos de **secreción, transporte, almacenamiento y respuesta hormonal del sistema endócrino** mediante un circuito eléctrico equivalente **RLC de segundo orden** ( $R_1, C, L, R_2$ ). Se busca contrastar una condición de referencia (**Control / Individuo sano**) frente a una condición alterada (**Caso / Paciente**) ante un estímulo sostenido tipo escalón unitario $V_e(t)=u(t)$, y diseñar un controlador PID que aproxime la respuesta dinámica del caso a la respuesta fisiológica de referencia.

**Docente:** Dr. Paul Antonio Valle Trujillo

**Correo:** paul.valle@tectijuana.edu.mx

**Departamento:** Ingeniería Eléctrica y Electrónica

**Institución:** Tecnológico Nacional de México / Instituto Tecnológico de Tijuana

**Asignatura:** Modelado de Sistemas Fisiológicos

**Programa:** Ingeniería Biomédica

**Estudiante:** Trinidad Diaz Kiara Vianney · 23212240

**Sede:** Blvd. Alberto Limón Padilla s/n, Tijuana, C. P. 22454, B. C., México.

## Contenido

 1. [Resumen](#resumen)

 2. [Competencia y objetivos](#objetivos)

 3. [Relación con el temario](#temario)

 4. [Modelo físico y analogía eléctrica](#analogia)

 5. [Formulación matemática](#modelo)

 6. [Análisis temporal y de estabilidad](#analisis)

 7. [Señal de entrada y condiciones de simulación](#senales)

 8. [Implementación en MATLAB, Simulink y Simscape](#matlab)

 9. [Control en lazo cerrado y sintonización PID](#pid)

10. [Reproducción en Python](#python)

11. [Controlador analógico y laboratorio](#laboratorio)

12. [Resultados y verificación cruzada](#resultados)

13. [Flujo de trabajo](#flujo)

14. [Actividades del estudiante](#actividades)

15. [Estructura del repositorio](#estructura)

16. [Requisitos](#requisitos)

## Resumen

La práctica tiene como propósito **formular, analizar, simular, controlar y validar experimentalmente** un modelo fenomenológico de segundo orden del sistema endócrino. A partir de una descripción funcional de los procesos de secreción glandular, transporte circulatorio, acumulación compartimental y respuesta del órgano diana, se propone un circuito eléctrico equivalente compuesto por dos resistores ( $R_1, R_2$ ), un capacitor ( $C$ ) y un inductor ( $L$ ).

Mediante las leyes de Kirchhoff y la transformada de Laplace se obtienen el modelo integro-diferencial, su representación en espacio de estados y las funciones de transferencia correspondientes a dos escenarios fisiológicos: **Control** (individuo sano) y **Caso** (paciente con alteración en las propiedades de almacenamiento $C$ y disipación $R_2$). Posteriormente, se implementa un **controlador PID en lazo cerrado** cuya referencia es la propia respuesta del individuo sano ( $e(t) = V_{s,control}(t) - V_{s,caso}(t)$ ). Los resultados obtenidos en MATLAB/Simulink y Python se contrastan con el montaje físico en protoboard y las mediciones del osciloscopio.

**Palabras clave:** Sistema endócrino; Analogía eléctrica RLC; Control y caso; Controlador PID; Modelado de sistemas fisiológicos; MATLAB; Simulink; Python.

[Volver al contenido](#contenido)

## Competencia y objetivos

### Competencia a desarrollar

Modelizar sistemas fisiológicos mediante analogías eléctricas, ecuaciones diferenciales y funciones de transferencia para analizar su comportamiento dinámico y diseñar estrategias de control clásico. El estudiante interpreta una descripción simplificada de secreción, transporte, almacenamiento y respuesta hormonal para proponer un circuito equivalente de segundo orden, comparar una condición de referencia frente a una condición alterada y validar un controlador PID mediante simulación y experimentación.

### Objetivo general

Proponer, formular y verificar un modelo dinámico de segundo orden del sistema endócrino bajo condiciones normales (**Control**) y alteradas (**Caso**), diseñando un controlador PID analógico y digital que permita compensar la dinámica alterada y contrastando los resultados teóricos, computacionales y experimentales.

[Volver al contenido](#contenido)

## Relación con el temario

La práctica integra contenidos correspondientes a las unidades de la asignatura **Modelado de Sistemas Fisiológicos (IBF-1020)**:

| Unidad | Área | Contenidos aplicados | 
| ----- | ----- | ----- | 
| **I** | Sistemas de control retroalimentados | Diagramas de bloques, señal de error $e(t) = V_{s,control}(t) - V_{s,caso}(t)$, análisis de respuesta transitoria y estabilidad. | 
| **II** | Modelado matemático | Síntesis de topología eléctrica, leyes de Kirchhoff, sistema de segundo orden y funciones de transferencia. | 
| **III** | Controladores | Sintonización de ganancias PID para compensación patológica. | 

[Volver al contenido](#contenido)

## Modelo físico y analogía eléctrica

### Topología del circuito equivalente

A partir de las hipótesis fisiológicas del sistema endócrino, el circuito equivalente consta de una **rama principal** formada por la fuente de voltaje $V_e(t)$ en serie con el resistor $R_1$. En el nodo posterior a $R_1$ (donde se mide la salida $V_s(t)$) se conecta un capacitor $C$ hacia referencia, en paralelo con una **segunda rama** formada por la conexión en serie del inductor $L$ y el resistor $R_2$ hacia referencia.

### Parámetros comerciales: Condición Control vs. Condición Caso

| Componente | Control (Individuo sano) | Caso (Paciente) | 
| ----- | ----- | ----- | 
| $R_1$ | $1\,\mathrm{k}\Omega$ | $1\,\mathrm{k}\Omega$ | 
| $L$ | $100\,\mathrm{mH}$ | $100\,\mathrm{mH}$ | 
| $C$ | $1\,\mu\mathrm{F}$ | $1000\,\mu\mathrm{F}$ | 
| $R_2$ | $100\,\mathrm{k}\Omega$ | $1\,\mathrm{k}\Omega$ | 

[Volver al contenido](#contenido)

## Formulación matemática

### 1. Ecuaciones integro-diferenciales

Con base en el análisis analítico (`AnalisisMatematicoP4.pdf`), aplicando las leyes de Kirchhoff se obtienen las siguientes ecuaciones:

$$
V_e(t) = R_1 F_e(t) + \frac{1}{C}\int (F_e(t) - F_s(t)) dt
$$

$$
V_s(t) = \frac{1}{C}\int (F_e(t) - F_s(t)) dt = L\frac{dF_s(t)}{dt} + R_2 F_s(t)
$$

### 2. Función de transferencia

Aplicando la transformada de Laplace con condiciones iniciales nulas y realizando el procedimiento algebraico:

$$
G(s) = \frac{V_s(s)}{V_e(s)} = \frac{Ls + R_2}{C L R_1 s^2 + (C R_1 R_2 + L)s + (R_1 + R_2)}
$$

[Volver al contenido](#contenido)

## Análisis temporal y de estabilidad

Con base en el desarrollo de la práctica, se realizó el análisis de la ecuación característica para encontrar los polos $\lambda_{1,2}$ y el error en estado estacionario $e(s) = \lim_{s\to0} V_e(s) (1 - G(s))$:

### 1. Condición Control (Individuo Sano)

* **Raíces (Polos):** $\lambda_1 = -1010.01$, $\lambda_2 = -999.989 \times 10^3$.

* **Estabilidad:** Sistema **estable** con respuesta **sobreamortiguada**.

* **Error en estado estacionario:** $e_{ss} = (1 - 0.99) = 0.01$, equivalente a un $1\%$ de error respecto a la entrada.

### 2. Condición Caso (Paciente)

* **Raíces (Polos):** $\lambda_1 = -2$, $\lambda_2 = -9998.99$.

* **Estabilidad:** Sistema **estable** con respuesta **sobreamortiguada** (transitorio más prolongado).

* **Error en estado estacionario:** $e_{ss} = (1 - 0.5) = 0.50$, equivalente a un $50\%$ de error (caída significativa del nivel hormonal).

[Volver al contenido](#contenido)

## Señal de entrada y condiciones de simulación

| Parámetro | Especificación | Descripción | 
| ----- | ----- | ----- | 
| **Señal de entrada** $V_e(t)$ | Escalón unitario $u(t)$ | Estímulo fisiológico sostenido ($0 \to 1\,\mathrm{V}$) | 
| **Intervalo temporal** | $t \in [0, 10]\,\mathrm{s}$ | Ventana de observación | 
| **Condiciones iniciales** | Nulas | Estado basal previo | 

[Volver al contenido](#contenido)

## Implementación en MATLAB, Simulink y Simscape

Se construyeron las representaciones equivalentes para la condición **Control** y **Caso** en el archivo `Practica4.slx`. La ejecución desde el cuaderno interactivo `Trinidad23212240.mlx` extrae las señales y genera las figuras comparativas pertinentes donde se observa gráficamente la diferencia de ganancia y tiempos de establecimiento calculada analíticamente.

[Volver al contenido](#contenido)

## Control en lazo cerrado y sintonización PID

El objetivo terapéutico es lograr que el paciente (**Caso**) reproduzca el comportamiento dinámico del individuo sano (**Control**).

La señal de error se define como la diferencia entre la respuesta del individuo sano y la del paciente:

$$
e(t) = V_{s,control}(t) - V_{s,caso}(t)
$$

### Diagrama de Bloques del Lazo de Control

```
graph LR
    %% Estilos de nodos
    classDef signal fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef system fill:#fff3e0,stroke:#f57c00,stroke-width:2px;
    classDef control fill:#e8f5e9,stroke:#388e3c,stroke-width:2px;
    classDef sum fill:#f3e5f5,stroke:#8e24aa,stroke-width:2px,shape:circle;

    %% Nodos
    Ve["V_e(t) <br> (Estímulo)"]:::signal
    Sano["Individuo Sano <br> (Planta Control)"]:::system
    Paciente["Paciente <br> (Planta Caso)"]:::system
    Sum(("+ / -")):::sum
    PID["Controlador PID <br> (Tratamiento)"]:::control
    Vs["V_s(t) <br> (Respuesta)"]:::signal

    %% Conexiones
    Ve --> Sano
    Sano -->|V_s,control| Sum
    Sum -->|e(t)| PID
    PID --> Paciente
    Paciente -->|V_s,PID| Vs
    Paciente -->|Retroalimentación| Sum

```

[Volver al contenido](#contenido)

## Reproducción en Python

Mediante el script `Trinidad23212240.py`, se reprodujo de forma independiente el comportamiento del sistema endócrino comprobando las funciones de transferencia y la simulación de lazo cerrado alimentado por la salida del sistema Control, generando las gráficas correspondientes.

[Volver al contenido](#contenido)

## Controlador analógico y laboratorio

Se realizó el montaje en protoboard de los circuitos correspondientes a **Control** y **Caso**. La señal fue inyectada con el generador de funciones y se capturó la respuesta mediante osciloscopio, evidenciando los fenómenos de atenuación calculados analíticamente (error del 1% para Control y 50% para Caso).

[Volver al contenido](#contenido)

## Resultados y verificación cruzada

A continuación se listan las evidencias de las distintas implementaciones y simulaciones del sistema:

| Plataforma / Escenario | Evidencia generada | 
| ----- | ----- | 
| **Análisis Matemático (Manual)** | [`AnalisisMatematicoP4.pdf`](AnalisisMatematicoP4.pdf) | 
| **MATLAB / Simulink** | \[`Control y caso.matlab.pdf`\](Control\\ y\\ caso.matlab.pdf) | 
| **Sintonización PID** | [`PID_P4.png`](PID_P4.png) | 
| **Python** | [`Control_caso_python4.pdf`](Control_caso_python4.pdf) | 
| **Laboratorio (Osciloscopio)** | [`ControlCaso_Lab.png`](ControlCaso_Lab.png) | 

[Volver al contenido](#contenido)

## Flujo de trabajo

```
flowchart TD
    %% Clases de estilo para hacer el diagrama bonito
    classDef inicio fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
    classDef proceso fill:#fff8e1,stroke:#ff8f00,stroke-width:2px;
    classDef lab fill:#fce4ec,stroke:#c2185b,stroke-width:2px;
    classDef final fill:#e8f5e9,stroke:#2e7d32,stroke-width:3px;

    A([Descripción fisiológica del sistema endócrino]):::inicio --> B[Propuesta de circuito RLC]:::proceso
    B --> C[Leyes de Kirchhoff y Laplace]:::proceso
    C --> D[G_control y G_caso / Modelo EDO]:::proceso
    D --> E[Análisis de polos, estabilidad y error]:::proceso
    D --> F[Simulink: Ecuaciones y Transfer Fcn]:::proceso
    F --> H[Comparación en lazo abierto]:::proceso
    H --> I[Lazo cerrado: e = Vs,control - Vs,caso]:::proceso
    I --> J[Sintonización PID con Tune]:::proceso
    J --> K[Reproducción en Python]:::proceso
    
    B --> M[Montaje físico Control y Caso]:::lab
    M --> O[Capturas de osciloscopio]:::lab
    
    H --> P{Verificación cruzada y discusión}:::proceso
    J --> P
    K --> P
    O --> P
    
    P --> Q([Documentación final en GitHub]):::final

```

[Volver al contenido](#contenido)

## Actividades del estudiante

* \[x\] **1. Circuito equivalente:** Propuesto en el análisis matemático.

* \[x\] **2. Funciones de transferencia:** Obtenidas algebraicamente.

* \[x\] **3. Error en estado estacionario:** Calculado (1% Control, 50% Caso).

* \[x\] **4. Estabilidad y polos:** Sistema sobreamortiguado en ambos casos.

* \[x\] **5. Bosquejo previo:** Plasmado en reporte de análisis.

* \[x\] **6. Ecuaciones integro-diferenciales:** Formuladas.

* \[x\] **7. Simulación en Simulink:** `Practica4.slx`.

* \[x\] **8-13. Simulación computacional:** Evidenciado en `Trinidad23212240.mlx` y `Trinidad23212240.py`.

* \[x\] **14-15. Sintonización PID:** Ganancias calculadas y evidencia subida (`PID_P4.png`).

* \[x\] **16. Laboratorio lazo abierto:** Evidencia subida en `ControlCaso_Lab.png`.

* \[x\] **19. Repositorio GitHub:** README documentado, organizado y con metadatos.

[Volver al contenido](#contenido)

## Estructura del repositorio

```
.
├── AnalisisMatematicoP4.pdf
├── CITATION.cff
├── Control_caso_python4.pdf
├── Control y caso.matlab.pdf
├── ControlCaso_Lab.png
├── LICENSE
├── PID_P4.png
├── Practica4.slx
├── README.md
├── Trinidad23212240.mlx
├── Trinidad23212240.pdf
└── Trinidad23212240.py

```

[Volver al contenido](#contenido)

## Requisitos

* **MATLAB** y **Simulink** (Con *Control System Toolbox* y *Simscape Electrical*)

* **Python 3.x** (Librerías sugeridas: `control`, `numpy`, `matplotlib`)

* **Git / GitHub**

[Volver al contenido](#contenido)