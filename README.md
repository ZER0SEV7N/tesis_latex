Revisando en detalle tanto la redacción de tu compañero como los diagramas BPMN y el diagrama de Gantt generados, el trabajo está **muy bien encaminado y tiene una coherencia técnica sólida**, pero existen **errores ortográficos, inconsistencias en los diagramas BPMN que un jurado observaría y discrepancias numéricas en el cronograma**.

A continuación tienes el desglose punto por punto de lo que debe corregirse y el código LaTeX pulido listo para reemplazar.

---

### 1. Hallazgos críticos en los Diagramas BPMN (Sección 4.4)

#### A. Diagrama de Solicitudes, Asistencia e Incidencias (`solicitudesAsistenciaIncidencias.png`

)

1. **Flujo roto tras la validación de sesión (Grave):**
* En el inicio, el *Empleado* realiza `Iniciar sesión` $\rightarrow$ pasa a `Validar el Access Token` en el carril de *Sistema Web*.


* De esa tarea de validación sale una flecha hacia `Registrar solicitud con sustento` en el carril del *Empleado*.


* **El error:** De esa misma tarea `Registrar solicitud con sustento`, la flecha sube hacia `Notificar al responsable de Recursos Humanos`. Pero la tarea `Importar Excel biométrico o registrar marcación manual` que hace RR.HH. viene como una secuencia después de `Evaluar solicitud y dictaminar efecto en asistencia`. En la realidad operativa, la importación de asistencia no depende de que alguien pida vacaciones; RR.HH. importa la asistencia de todos los días independientemente.




2. **Ortografía en cajas y anotaciones de Bizagi:**
* En el carril de Empleado dice: `vacaiones` (debe decir **vacaciones**).


* En la compuerta de decisión dice: `¿Existen alertas o días pendientes?` y el camino del NO va a `Aprobar y cerrar asistencia del perioco`. Dice **perioco**, debe decir **periodo**.


* En las notas grises dice: `Registrar marcacion manual requiere motivo y evidencia`. Le falta tilde a **marcación**.





#### B. Diagrama de Elaboración y Cierre de Nóminas (`elaboracion-cierre-nomina.png`

)

* **Línea punteada errónea en el retorno de ajuste:**
* Arriba a la derecha, de la tarea `Ajustar novedad monetaria o parámetro legal vigente`, sale una línea punteada hacia un texto gris (`Vuelve a solicitar cálculo con motivo auditable`), y de ahí sale una línea negra hacia `Solicitar cálculo masivo o individual de planilla`.


* En BPMN, las **líneas punteadas son asociaciones de datos/notas**, no flujo de secuencia. La flecha de retorno debe ser una **línea continua normal de flujo de secuencia** que conecte directamente la salida de `Ajustar novedad monetaria` de vuelta a la entrada de `Solicitar cálculo masivo o individual de planilla`.





#### C. Diagrama de Automatización con n8n (`subproceso-automatizacion-doc-entregas.png`

)

* Este diagrama está impecable. La bifurcación paralela (Compuerta `+` tipo Fork) y la sincronización (Compuerta `+` tipo Join) con sus tres ramas paralelas respetan la especificación estándar de BPMN 2.0.



---

### 2. Hallazgos en la Redacción y el Cronograma (Sección 4.5)

1. **Título de la subsección:** En el código LaTeX tu compañero escribió:
`\subsection{Diagrama del proceso, mapa del flujo de valor y/o diagrama de operación de la situación mejora}`
Debe decir **situación mejorada** (con «da» al final).
2. **Inconsistencia de días en el Cronograma:**
* En el texto del 4.5 dice: *«La implementación abarca 156 días laborables, del 12 de mayo al 28 de diciembre de 2026...»*.
* Sin embargo, en el Capítulo 5 (Sección 5.2), computamos la mano de obra para los 4 desarrolladores principales en **145 días laborables** (de mayo a noviembre de 2026), ya que en diciembre solo se ejecuta el despliegue del MVP y la entrega final (fase 5). Conviene aclararlo en el texto para que el jurado no piense que hay una contradicción entre los 145 días de desarrollo activo y los 156 días de calendario total del proyecto.




3. **Formato de Figuras APA 7:**
* En la sección 4.4 usó el entorno estándar `\begin{figure}[H]`, mientras que en todo el resto de la tesis y en la sección 4.5 estás usando el comando estandarizado del documento (`\noindent\begin{minipage}{\linewidth} ... \tituloTablaAPA / \figuraMedida`). Mantener el mismo formato visual evita saltos de página extraños y unifica la tipografía de notas de figura.



---

### 3. Código LaTeX corregido y unificado para las Secciones 4.4 y 4.5

Reemplaza el bloque de tu compañero por el siguiente código, el cual ya tiene corregidas las tildes, las referencias cruzadas de Jira (`BEL-*`), los requerimientos (`RF-*` y `RNF-*`) y la consistencia de días con el Capítulo 5:

```latex
\clearpage
\subsection{Diagrama del proceso, mapa del flujo de valor y/o diagrama de operación de la situación mejorada}

En este apartado se describe el funcionamiento de los procesos operativos bajo la intervención del sistema web propuesto (escenario \textit{TO-BE}). A diferencia del flujo manual tradicional analizado en el Capítulo~3, la propuesta centraliza los registros en una única base de datos relacional (PostgreSQL 16), segrega los permisos mediante control de acceso basado en roles (RBAC: Recursos Humanos, Asistente Contable, Contador y Administrador) y delega la compilación documental masiva y el despacho de notificaciones al orquestador asíncrono \textbf{n8n}.

La mejora operativa se estructura en dos flujos principales y un subproceso automatizado que atienden de forma directa las causas raíz identificadas en el diagnóstico:
(1) gestión de asistencia, incidencias y solicitudes,
(2) elaboración y cierre de nóminas, y
(3) subproceso automatizado de preparación de documentos y entregas, el cual se dispara una vez congelada la planilla contable.

\subsubsection{Flujo de gestión de asistencia, incidencias y solicitudes}

Este flujo sustituye la transcripción manual de tardanzas, inasistencias y horas extras hacia hojas de cálculo dispersas, y estandariza la tramitación entre los colaboradores y el área de Recursos Humanos, eliminando el traslado informal de documentos físicos o correos electrónicos aislados (atendiendo las causas \textbf{CA1}, \textbf{CA2}, \textbf{CA3} y \textbf{CA5}). Las solicitudes y el control de asistencia se integran en un mismo flujo debido a que toda solicitud aprobada (vacaciones, licencias o permisos) impacta directamente en el cómputo final de la jornada laboral del periodo.

\begin{enumerate}[leftmargin=*]
    \item \textbf{Registro de la solicitud con sustento digital:} El colaborador inicia sesión validando su token de acceso JWT. Al gestionar vacaciones, licencias médicas o permisos, registra la solicitud en el portal adjuntando obligatoriamente la evidencia probatoria en formato digital (\textbf{RF-06}, \textbf{RF-08}, \textbf{BEL-81}).
    
    \item \textbf{Evaluación y dictamen de Recursos Humanos:} El sistema notifica en tiempo real al responsable de Recursos Humanos, quien accede al módulo de evaluación (\textbf{BEL-107}) para verificar la autenticidad de los adjuntos y la coherencia del rango de fechas frente al periodo laboral activo (\textbf{BEL-58}). Al evaluar la solicitud, dictamina su aprobación o rechazo estableciendo su efecto en la asistencia (con goce o sin goce de haber), incorporándose únicamente las solicitudes aprobadas al legajo del trabajador (\textbf{BEL-50}, \textbf{BEL-105}).
    
    \item \textbf{Captura e ingesta de marcaciones biométricas:} De forma periódica, Recursos Humanos importa el archivo consolidado del reloj biométrico mediante validación transaccional (\textbf{BEL-57}, \textbf{BEL-78}) o efectúa marcaciones manuales excepcionales (\textbf{BEL-77}), las cuales exigen registrar de manera obligatoria el motivo y la referencia documental del sustento.
    
    \item \textbf{Cruce automático de jornadas, marcaciones y solicitudes:} El motor del sistema contrasta cada marcación frente al catálogo de jornadas asignadas, su margen de tolerancia (\textbf{BEL-73}, \textbf{BEL-74}) y el calendario laboral (\textbf{BEL-75}), determinando con exactitud los minutos de tardanza o tipificando la inasistencia (\textbf{BEL-79}). En el mismo proceso cruza las ausencias con las solicitudes aprobadas (\textbf{RF-11}), previniendo descuentos salariales indebidos por causas justificadas.
    
    \item \textbf{Resolución de alertas operativas:} Si el cruce algorítmico detecta inconsistencias o marcaciones incompletas, Recursos Humanos registra la decisión operativa correspondiente debidamente justificada, recalculando el resumen hasta que no existan observaciones bloqueantes.
    
    \item \textbf{Cierre y aprobación de la asistencia mensual:} Una vez subsanadas las alertas, Recursos Humanos aprueba y cierra la asistencia del periodo (\textbf{BEL-90}). El sistema actualiza su estado a \textit{Aprobada}, consolida las incidencias en la vista mensual (\textbf{BEL-56}, \textbf{BEL-80}) y habilita formalmente la elaboración de la nómina, dejando la información disponible en modo de consulta protegida para el área contable (\textbf{BEL-89}, \textbf{BEL-91}, \textbf{BEL-100}).
\end{enumerate}

\par\medskip
\noindent
\begin{minipage}{\linewidth}
    \linespread{1}\footnotesize
    \refstepcounter{figure}\label{fig:flujo_asistencia_mejorado}
    \addcontentsline{lof}{figure}{Figura~\thefigure. Proceso mejorado de gestión de asistencia, incidencias y solicitudes en el sistema web}
    {\bfseries Figura~\thefigure}\par
    {\itshape Proceso mejorado de gestión de asistencia, incidencias y solicitudes en el sistema web}\par
    \vspace{4pt}
    \makebox[\linewidth][c]{\includegraphics[
        width=\linewidth,
        height=0.62\textheight,
        keepaspectratio
    ]{images/diagramas-procesos-mejorados/solicitudesAsistenciaIncidencias.png}}\par
    \vspace{4pt}
    \textit{Nota.} Registro y evaluación de solicitudes con sustento, captura de marcaciones, cruce automático con jornadas y cierre de la asistencia del periodo. Elaboración propia.\par
\end{minipage}
\finBloqueAPA

\subsubsection{Flujo de elaboración y cierre de nóminas}

Este flujo automatiza el cálculo de las remuneraciones de ley y garantiza la inmutabilidad física de los resultados contables aprobados (atendiendo las causas \textbf{CA4}, \textbf{CA7} y \textbf{CA8}). Se activa formalmente cuando la asistencia mensual ha sido aprobada por Recursos Humanos.

\begin{enumerate}[leftmargin=*]
    \item \textbf{Verificación de parámetros y novedades monetarias:} El Contador o Asistente Contable se autentica en su módulo (\textbf{BEL-87}), examina en modo lectura el legajo maestro (\textbf{BEL-88}), verifica el cierre de la asistencia (\textbf{BEL-90}, \textbf{BEL-91}), inscribe novedades monetarias excepcionales (bonos o adelantos) y comprueba la vigencia de los parámetros legales del periodo (RMV, UIT, tasas de AFP/ONP y factores de quinta categoría) (\textbf{RF-09}, \textbf{BEL-99}).
    
    \item \textbf{Ejecución del cálculo masivo en segundo plano:} Al solicitar la liquidación (\textbf{RF-12}), el backend encola el trabajo en BullMQ sobre Redis y lo delega a un grupo de hilos secundarios (\textit{Worker Threads} con Piscina) (\textbf{RNF-03}). El motor procesa las fórmulas salariales, retenciones previsionales, impuesto a la renta de quinta categoría y aportaciones patronales con precisión decimal exacta (\texttt{decimal.js}), persistiendo un snapshot inmutable de auditoría.
    
    \item \textbf{Auditoría y revisión de la pre-planilla:} El Contador audita los importes calculados en la pre-planilla. En caso de requerir modificaciones en novedades o parámetros, actualiza la información indicando un motivo auditable y vuelve a solicitar el cálculo del periodo.
    
    \item \textbf{Aprobación y congelamiento contable del periodo:} Validados los montos, el Contador ejecuta el congelamiento definitivo de la planilla (\textbf{RF-13}). Los disparadores (\textit{triggers}) en PostgreSQL bloquean físicamente cualquier mutación posterior sobre las tablas de cálculo y registran la acción en la bitácora inmutable.
    
    \item \textbf{Disparo de entregables masivos:} Con la planilla en estado congelado, el sistema habilita el subproceso automatizado de generación de planos regulatorios, transferencias bancarias y distribución de boletas mediante n8n.
\end{enumerate}

\par\medskip
\noindent
\begin{minipage}{\linewidth}
    \linespread{1}\footnotesize
    \refstepcounter{figure}\label{fig:flujo_planilla_mejorado}
    \addcontentsline{lof}{figure}{Figura~\thefigure. Proceso mejorado de elaboración y cierre de nóminas}
    {\bfseries Figura~\thefigure}\par
    {\itshape Proceso mejorado de elaboración y cierre de nóminas}\par
    \vspace{4pt}
    \makebox[\linewidth][c]{\includegraphics[
        width=\linewidth,
        height=0.62\textheight,
        keepaspectratio
    ]{images/diagramas-procesos-mejorados/elaboracion-cierre-nomina.png}}\par
    \vspace{4pt}
    \textit{Nota.} Secuencia de cálculo asíncrono, revisión de la pre-planilla, congelamiento del periodo y disparo del subproceso de exportaciones. Elaboración propia.\par
\end{minipage}
\finBloqueAPA

\subsubsection{Subproceso automatizado de preparación de documentos y entregas}

Este subproceso delega en el motor de flujos \textbf{n8n} la compilación de archivos de declaración y transferencias bancarias, así como el renderizado masivo y despacho de boletas digitales, liberando al área contable de la elaboración manual (\textbf{RF-10}, \textbf{RF-14}, \textbf{RF-15}).

\begin{enumerate}[leftmargin=*]
    \item \textbf{Solicitud de exportación:} A partir de una planilla congelada, el Contador solicita la generación de los planos regulatorios, transferencias bancarias o el despacho masivo de boletas.
    
    \item \textbf{Despacho del trabajo de integración:} El backend crea un trabajo en base de datos y remite una petición webhook firmada mediante \textbf{HMAC-SHA256} hacia el contenedor de n8n, garantizando que el orquestador solo ejecute llamadas auténticas y no manipuladas.
    
    \item \textbf{Compilación paralela de entregables:} Tras validar la firma criptográfica, n8n extrae de forma paginada la información de la planilla y ejecuta tres ramas paralelas: (a) compila las estructuras para el PDT PLAME de SUNAT (\texttt{.rem} y \texttt{.jor}) y AFPnet; (b) genera el archivo TXT para Telecrédito BCP y el consolidado de costos en CSV; y (c) renderiza las boletas de pago en PDF cifradas individualmente con el DNI del trabajador, distribuyéndolas vía SMTP.
    
    \item \textbf{Reporte y confirmación de resultados:} El orquestador envía un callback seguro al backend reportando el manifiesto de artefactos generados con sus hashes SHA-256 y el estado de recepción de los correos, actualizando la auditoría del sistema.
    
    \item \textbf{Descarga y registro del desembolso:} El Contador descarga los archivos listos para su carga en los portales bancarios y tributarios, y registra la constancia de pago, transicionando la nómina al estado final \textit{Pagada}.
\end{enumerate}

\par\medskip
\noindent
\begin{minipage}{\linewidth}
    \linespread{1}\footnotesize
    \refstepcounter{figure}\label{fig:flujo_exportacion_n8n}
    \addcontentsline{lof}{figure}{Figura~\thefigure. Subproceso automatizado de preparación de documentos y entregas}
    {\bfseries Figura~\thefigure}\par
    {\itshape Subproceso automatizado de preparación de documentos y entregas}\par
    \vspace{4pt}
    \makebox[\linewidth][c]{\includegraphics[
        width=\linewidth,
        height=0.62\textheight,
        keepaspectratio
    ]{images/diagramas-procesos-mejorados/subproceso-automatizacion-doc-entregas.png}}\par
    \vspace{4pt}
    \textit{Nota.} Generación de archivos PLAME, AFPnet y Telecrédito BCP, y emisión y envío de boletas cifradas mediante n8n a partir de la planilla congelada. Elaboración propia.\par
\end{minipage}
\finBloqueAPA

\subsection{Cronograma de ejecución de la mejora}

El cronograma del proyecto se estructuró en el software de gestión ProjectLibre, permitiendo diagramar la secuencia lógica de actividades, dependencias técnicas y la asignación de recursos humanos y computacionales bajo un diagrama de Gantt. 

El ciclo integral comprende 156 días laborables de calendario técnico, desarrollados entre el 12 de mayo y el 28 de diciembre de 2026. Esta programación se desglosa en cinco fases secuenciales y superpuestas: 
(1) Fase de Análisis y Levantamiento de Requisitos (mayo y junio), 
(2) Fase de Backend de RR.HH. y Núcleo Contable (julio a octubre), 
(3) Fase de Frontend React SPA (septiembre a diciembre), 
(4) Fase de Integraciones Automatizadas con n8n (octubre a diciembre), y 
(5) Fase de Despliegue del MVP y Entrega (diciembre). 

Cabe precisar que las fases centrales de codificación técnica comprendieron 145 días laborables de desarrollo activo por parte del equipo núcleo (tal como se computó en la valorización de mano de obra del Capítulo~5), mientras que el tiempo restante corresponde a las etapas previas de levantamiento y a la transferencia final. La Figura~\ref{fig:cronograma_gantt} ilustra las 50 actividades programadas, destacando la ruta crítica del proyecto y los porcentajes de avance a la fecha de corte.

\par\vspace{6pt}
\noindent
\begin{minipage}{\linewidth}
    \linespread{1}\footnotesize
    \refstepcounter{figure}%
    \label{fig:cronograma_gantt}%
    \addcontentsline{lof}{figure}{Figura~\thefigure. Diagrama de Gantt de la implementación de la mejora}%
    {\bfseries Figura~\thefigure}\par
    {\itshape Diagrama de Gantt de la implementación de la mejora}\par
    \vspace{4pt}
    \makebox[\linewidth][c]{\includegraphics[
        width=\linewidth,
        height=0.42\textheight,
        keepaspectratio
    ]{images/cronograma-gantt.png}}\par
    \vspace{4pt}
    \textit{Nota.} Planificación elaborada en ProjectLibre. Las barras rojas identifican las actividades pertenecientes a la ruta crítica del proyecto. Convención de iniciales: DS, Daniel Singer; RC, Rodrigo Castillo; AD, Adrian Dueñas; DF, Dylan Florez; JV, Julio Vargas; LS, Luis Diego Sipion; JF, Julio Flores; DR, Daniel Román; JP, Juan Pablo Alcalá. Elaboración propia.\par
\end{minipage}
\finBloqueAPA

```

---

### 4. Qué decirle a tu compañero para ajustar los diagramas en Bizagi

Dile puntualmente estos dos cambios sencillos en Bizagi para que los vuelva a exportar a PNG:

1. En `solicitudesAsistenciaIncidencias.png`:


* Corregir el texto de la caja gris a la derecha que dice `"perioco"` por **`periodo`**.


* Corregir `"vacaiones"` por **`vacaciones`**.




2. En `elaboracion-cierre-nomina.png`:


* La flecha que sale de `Ajustar novedad monetaria` hacia atrás (hacia `Solicitar cálculo masivo`) debe ser una **flecha negra continua normal**, no una línea punteada gris.