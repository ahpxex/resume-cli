

# resumy

README en inglés / [中文文档](https://github.com/ahpxex/resume-cli/blob/main/README.zh-CN.md).

`resumy` es una CLI priorizada para Bun para generar currículums profesionales a partir de flags estructurados y plantillas integradas. Está diseñada para permanecer scriptable, determinista y amigable para agentes o usuarios avanzados que prefieren la entrada explícita de comandos sobre flujos interactivos.

## Instalación

Instala `resumy` globalmente con Bun:

```bash
bun add -g resumy
bun install -g resumy
```

Tras la instalación, el comando global es:

```bash
resumy --help
```

## Publicar en npm

Publica el paquete públicamente en npm:

```bash
npm login
npm publish
```

Una vez publicado, otros usuarios pueden instalarlo globalmente con Bun y ejecutar `resumy` directamente.

## Instalar la Skill del Agente

Este repositorio también distribuye la skill `agent-resume` para el instalador de skills.

Instálala desde GitHub:

```bash
bunx skills add https://github.com/ahpxex/resume-cli --skill agent-resume
```

Inspeciónala localmente durante el desarrollo:

```bash
bunx skills add . --list
bunx skills add . --skill agent-resume
```

## Inicio Rápido

Lista las plantillas integradas:

```bash
resumy templates
```

Genera un currículum en PDF:

```bash
resumy generate pdf \
  --theme professional \
  --name "Jordan Lee" \
  --title "Product Engineer" \
  --email "jordan@example.com" \
  --phone "+1 (555) 123-4567" \
  --location "San Francisco, CA" \
  --website "https://jordanlee.dev" \
  --link "GitHub|https://github.com/jordanlee" \
  --link "LinkedIn|https://linkedin.com/in/jordanlee" \
  --summary "Product-minded engineer with a track record of shipping polished user experiences." \
  --experience "role=Senior Product Engineer;company=Northstar Labs;start=2022;end=Present;location=Remote;summary=Led the frontend architecture for customer-facing workflows." \
  --experience-bullet "0|Built a design-system-based UI platform across three product teams." \
  --experience-bullet "0|Improved onboarding completion by 18% through a guided setup flow." \
  --experience-tech "0|TypeScript, React, Bun, Design systems" \
  --project "name=Resume Studio;role=Creator;url=https://github.com/jordanlee/resume-studio;summary=A template-driven resume renderer for structured content." \
  --project-bullet "0|Designed a normalized resume schema for multiple layouts." \
  --project-tech "0|TypeScript, Bun, HTML, CSS" \
  --education "institution=University of Washington;degree=B.S. in Computer Science;start=2015;end=2019;location=Seattle, WA" \
  --education-highlight "0|Focused on human-computer interaction and distributed systems." \
  --skill-group "Languages|TypeScript, JavaScript, SQL, HTML, CSS" \
  --skill-group "Frameworks|React, Next.js, Bun, Node.js" \
  --extra "Certifications|AWS Certified Cloud Practitioner" \
  --output ./dist/resume.pdf
```

También genera el HTML intermedio:

```bash
resumy generate pdf ... --html-output ./dist/resume.html
```

## Comandos

- `resumy templates`: lista los diseños integrados
- `resumy generate pdf`: genera un PDF de currículum a partir de flags explícitos

## Cómo Funciona la Exportación a PDF

`resumy` primero renderiza tus datos estructurados del currículum a HTML, luego lanza un navegador sin interfaz (headless) a través de Playwright y le solicita al navegador que imprima ese HTML en un PDF. Esto mantiene el desarrollo de plantillas simple mientras preserva la calidad del diseño del navegador, tipografías, colores y estilos de impresión.

## Playwright y Chromium

- `resumy` depende del paquete JavaScript Playwright para la automatización del navegador.
- El paquete publicado de `resumy` no incluye un binario de Chromium dentro del tarball del paquete.
- En tiempo de ejecución, `resumy` primero intenta usar el Chromium de Playwright. Si no está disponible, utiliza como respaldo un Google Chrome instalado localmente.
- Si ninguno de los navegadores está disponible, instala Chromium con `bunx playwright install chromium`.

## Modelo de Entrada

La CLI es intencionalmente explícita. Las entradas repetidas se pasan con flags repetidos:

- `--experience "role=...;company=...;start=...;end=...;location=...;summary=..."`
- `--experience-bullet "0|Built something"`
- `--experience-tech "0|TypeScript, React, Bun"`
- `--project "name=...;role=...;url=...;summary=..."`
- `--project-bullet "0|Shipped something"`
- `--project-tech "0|TypeScript, Bun, HTML, CSS"`
- `--education "institution=...;degree=...;start=...;end=...;location=..."`
- `--education-highlight "0|Focused on ..."`
- `--skill-group "Languages|TypeScript, JavaScript, SQL"`
- `--extra "Certifications|AWS Certified Cloud Practitioner"`

Se utilizan índices basados en cero para adjuntar viñetas y pilos de tecnología a las entradas correspondientes.

## Opciones de Tipografía

- `--density`: `standard` o `compact`
- `--theme-color`: color de acento para enlaces, encabezados y detalles visuales
- `--font-family`: pila de fuentes para el cuerpo
- `--heading-font-family`: pila de fuentes para encabezados
- `--font-face`: incorpora archivos locales `.ttf`, `.otf`, `.woff` o `.woff2`

## Desarrollo

```bash
bun install
bun run check
bun test
bun run build
npm publish --dry-run
```
