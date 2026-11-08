#### DIAGRAMA 1: Gestión de Solicitudes, Asistencia y Cierre de Incidencias 

* **Nombre del Pool:** `Gestión de asistencia e incidencias en el sistema web`
* **Carriles (Lanes):**
1. `Colaborador / Empleado`
2. `Responsable de Recursos Humanos`
3. `Sistema Web (Backend NestJS + PostgreSQL)`



**Secuencia exacta de elementos para dibujar en Bizagi:**

1. **(Carril Empleado) Evento de Inicio** $\rightarrow$ **Tarea de Usuario:** `Iniciar sesión y registrar solicitud con sustento` *(Vacaciones, licencia o permiso)*.


2. **(Carril Sistema Web) Tarea de Servicio:** `Validar sesión JWT y registrar en bitácora (audit_log)`.


3. **(Carril RR.HH.) Tarea de Usuario:** `Evaluar solicitud y dictaminar efecto en asistencia` *(Aprueba/Rechaza y define Con Goce / Sin Goce)*.


4. **(Carril RR.HH.) Tarea de Usuario:** `Importar Excel biométrico o registrar marcación manual` *(Con motivo y referencia de evidencia)*.


5. **(Carril Sistema Web) Tarea de Servicio:** `Cruzar jornadas, marcaciones y solicitudes aprobadas` *(Genera resumen mensual e identifica alertas)*.


6. **(Carril RR.HH.) Compuerta Exclusiva (Rombo):** `¿Existen alertas o días pendientes?`


* **Ruta SÍ** $\rightarrow$ **(Carril RR.HH.) Tarea de Usuario:** `Registrar decisión operativa con sustento` *(Condonar/descontar tardanza, clasificar ausencia o autorizar sobretiempo)* $\rightarrow$ *(Vuelve a la tarea 5 del Sistema Web para regenerar resumen)*.


* **Ruta NO** $\rightarrow$ **(Carril RR.HH.) Tarea de Usuario:** `Aprobar y cerrar asistencia del periodo`.


7. **(Carril Sistema Web) Tarea de Servicio:** `Cambiar estado a APROBADA, auditar y habilitar planilla` $\rightarrow$ **Evento de Fin**.

---

#### DIAGRAMA 2: Proceso General de Cálculo y Congelamiento de Nómina (Espejo de Figura 4)

* **Nombre del Pool:** `Proceso mejorado de elaboración y cierre de nóminas`
* **Carriles (Lanes):**
1. `Contador / Asistente Contable`
2. `Sistema Web (Motor BullMQ + Piscina + Prisma ORM)`



**Secuencia exacta de elementos para dibujar en Bizagi:**

1. **(Carril Contador) Evento de Inicio** (`Asistencia aprobada`) $\rightarrow$ **Tarea de Usuario:** `Autenticarse, verificar cierre de asistencia y registrar novedades` *(Bonos, adelantos o descuentos autorizados)*.


2. **(Carril Contador) Tarea de Usuario:** `Solicitar cálculo masivo o individual de planilla`.


3. **(Carril Sistema Web) Tarea de Servicio:** `Encolar trabajo (BullMQ) y calcular en paralelo (Worker Threads)` *(Aplica RMV, AF, HE 25%/35%, AFP/ONP, 5ta Cat. y EsSalud con Decimal.js)*.


4. **(Carril Sistema Web) Tarea de Servicio:** `Persistir snapshot de cálculo y registrar en audit_log`.


5. **(Carril Contador) Tarea de Usuario:** `Revisar pre-planilla y montos calculados`.


6. **(Carril Contador) Compuerta Exclusiva (Rombo):** `¿Requiere ajuste de novedades o parámetros?`


* **Ruta SÍ** $\rightarrow$ **(Carril Contador) Tarea de Usuario:** `Ajustar novedad monetaria o parámetro legal vigente` *(Con motivo auditable)* $\rightarrow$ *(Vuelve a la tarea 2 para recalcular)*.


* **Ruta NO** $\rightarrow$ **(Carril Contador) Tarea de Usuario:** `Aprobar y congelar planilla del periodo`.


7. **(Carril Sistema Web) Tarea de Servicio:** `Bloquear periodo (Trigger SQL inmutable) y guardar auditoría`.


8. **(Carril Contador) Subproceso Colapsado (Caja con signo `[+]`):** `Ejecutar exportaciones y distribución automatizada (n8n)` $\rightarrow$ **Evento de Fin**.

---

#### DIAGRAMA 3: Subproceso Automatizado de Exportación y Entregas con n8n (Espejo de Figura 5)

* **Nombre del Pool:** `Subproceso automatizado de preparación de documentos y entregas`
* **Carriles (Lanes):**
1. `Contador`
2. `Sistema Web (Backend NestJS)`
3. `Orquestador de Flujos (n8n Engine)`



**Secuencia exacta de elementos para dibujar en Bizagi:**

1. **(Carril Contador) Evento de Inicio** (`Planilla congelada`) $\rightarrow$ **Tarea de Usuario:** `Solicitar exportación regulatoria, bancaria y envío de boletas`.


2. **(Carril Sistema Web) Tarea de Servicio:** `Crear trabajo de integración y emitir Webhook firmado (HMAC-SHA256)`.


3. **(Carril n8n) Tarea de Servicio:** `Validar firma HMAC y extraer datos paginados de planilla`.


4. **(Carril n8n) Compuerta Paralela (Rombo con signo `+` — División):** Se abre en **3 ramas simultáneas**:
* **Rama 1 (Tarea de Servicio):** `Compilar archivos SUNAT PLAME (.rem/.jor) y AFPnet`.


* **Rama 2 (Tarea de Servicio):** `Generar TXT Telecrédito BCP y reporte CSV de costos`.


* **Rama 3 (Tarea de Servicio):** `Renderizar boletas PDF cifradas con DNI y enviar por SMTP`.


5. **(Carril n8n) Compuerta Paralela (Rombo con signo `+` — Sincronización):** Une las 3 ramas $\rightarrow$ **Tarea de Servicio:** `Reportar manifiesto de archivos, hashes SHA-256 y estado SMTP` *(Callback M2M)*.


6. **(Carril Sistema Web) Tarea de Servicio:** `Actualizar estado del trabajo y registrar en auditoría`.


7. **(Carril Contador) Tarea de Usuario:** `Descargar archivos generados y registrar desembolso (PAGADA)` $\rightarrow$ **Evento de Fin**.

