# Proyecto de Innovación y/o Mejora — SENATI (2026)

> **“Desarrollo e implementación de un sistema web con flujos automatizados para la gestión de nóminas y procesos de recursos humanos orientado a clientes empresariales de J&P Periféricos S.A.C.”**

Documento académico de titulación redactado en **LaTeX** bajo lineamientos de **SENATI** (Dirección Zonal Lima – Callao, CFP Luis Cáceres Graziani, Carrera de Desarrollo de Software) y formato **APA 7.ª edición** (`apa7` + `biblatex-apa`).

---

## Autores y Asesoría

| Rol | Nombre |
| :--- | :--- |
| **Autores** | Daniel Enrique Singer Rojas |
| | Rodrigo Alessandro Castillo Del Castillo |
| | Adrian Matias Dueñas Huertas |
| **Asesor** | Mg. José Armando Tiznado Ubillus |
| **Empresa patrocinadora** | J&P Periféricos S.A.C. (Lima, Perú) |

---

## Estructura del Repositorio

```text
tesis-latex/
├── main.tex                  # Documento maestro (preámbulo APA 7, macros e índices)
├── referencias.bib           # Base bibliográfica en formato BibLaTeX
├── portada/                  # Agradecimientos, dedicatorias, resumen e introducción
├── capitulo-1/               # Generalidades de la empresa y entrevistas
├── capitulo-2/               # Problemática, objetivos, antecedentes, justificación y marco teórico
├── capitulo-3/               # Análisis de la situación actual (AS-IS, Ishikawa y Pareto)
├── capitulo-4/               # Propuesta técnica, Scrum, evidencias, requerimientos y arquitectura (TO-BE)
├── capitulo-5/               # Estructura de costos e inversión del proyecto
├── capitulo-6/               # Evaluación técnica y económica (Beneficio/Costo y Payback)
├── capitulo-7/               # Resultados, discusión y conclusiones
├── capitulo-8/               # Recomendaciones operativas y trabajo futuro
├── anexos/                   # Anexos técnicos (depreciación, RF, RNF, Acta, SaaS) y formatos SENATI
└── images/                   # Diagramas BPMN, ERDs, arquitectura, Gantt, evidencias y firmas
```

---

## Requisitos y Compilación

### Requisitos previos
- Distribución LaTeX actualizada: **MiKTeX** (Windows) o **TeX Live** (Linux/macOS).
- **Biber** (para procesar `referencias.bib` con `biblatex-apa`).
- **latexmk** (incluido en MiKTeX/TeX Live) o la extensión **LaTeX Workshop** en Visual Studio Code.

### Compilar el PDF completo
Desde la raíz del repositorio:

```bash
latexmk -pdf -synctex=1 -interaction=nonstopmode main.tex
```

O manualmente paso a paso:

```bash
pdflatex main.tex
biber main
pdflatex main.tex
pdflatex main.tex
```

Para limpiar archivos auxiliares generados durante la compilación:

```bash
latexmk -c
```

---

## Convenciones del Documento (`main.tex`)

- **Estilo de citas:** `biblatex` con estilo `apa` (`\textcite{...}` para citas narrativas y `\parencite{...}` para citas parentéticas).
- **Macros propios del proyecto:**
  - `\portadacapitulo{Título}`: portada de cada capítulo.
  - `\tituloTablaAPA{etiqueta}{Título}` + `\notaTabla{...}` + `\finBloqueAPA`: tablas con formato APA 7.
  - `\figuraEnTexto{etiqueta}{Título}{ruta_imagen}{Nota}`: figuras centradas con pie APA 7.
  - `\anexoItem{Título}{Contenido}`: registro automático en el Índice de Anexos.
- **Nota sobre porcentajes con `babel` (español):** escribir `100 \%` con espacio normal en lugar de `100\,\%`, ya que `spanish.ldf` genera conflicto de unidades con `\,` antes de `\%`.

---

## Síntesis Técnica y Económica del Proyecto

- **Stack del sistema documentado:** React 19 + Vite + TypeScript, NestJS 11 + Fastify 5 + Prisma ORM, PostgreSQL 16, Redis 7 + BullMQ + Worker Threads (Piscina), `n8n`, `Zod` (`@jyp/shared-contracts`) y cifrado `AES-256-GCM`.
- **Resultados operativos (entorno de pruebas):** reducción promedio simple de **91.4 %** en los ocho procedimientos cronometrados y **100 %** de cumplimiento en centralización, parametrización legal y auditoría.
- **Evaluación económica:** inversión total de **S/ 60,906.79**, beneficio anual proyectado de **S/ 73,540.12**, relación **B/C = 1.21** y periodo de recuperación de **9 meses y 28 días**.
