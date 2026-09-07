# Skills instaladas en este proyecto

Estas skills viven a nivel de proyecto (`.claude/skills/`) y se cargan automáticamente
en cualquier sesión de Claude Code abierta en este repo. Se agregaron para mejorar
copy, estrategia de contenido y diseño visual de las piezas (historias/posts de
Instagram, sorteos, etc.) — ver `../CLAUDE.md` para el contexto completo del proyecto
Motion Lab.

Todas son de terceros, MIT-licensed (ver `LICENSE-*.txt` en esta misma carpeta), y se
copiaron tal cual (con ajustes menores de rutas donde aplicó) al repo en vez de
instalarse como plugin de marketplace, para que persistan en el repo sin depender de
instalación manual en cada sesión/máquina.

## 1. marketingskills — [coreyhaines31/marketingskills](https://github.com/coreyhaines31/marketingskills)

Las 50 carpetas bajo `.claude/skills/` que NO son `brand`, `banner-design`,
`creative-assets`, `ui-ux-pro-max` ni `stop-slop` vienen de aquí (`ab-testing`,
`ad-creative`, `ads`, `copywriting`, `copy-editing`, `social`, `offers`,
`marketing-plan`, `marketing-ideas`, `co-marketing`, `image`, etc. — lista completa
en el propio repo). Se excluyeron las carpetas `evals/` de cada skill (arnés de
pruebas del propio proyecto, no aportan nada en tiempo de uso).

Más relevantes para el trabajo diario de historias/posts de Motion Lab:
`social`, `ad-creative`, `copywriting`, `copy-editing`, `offers`, `co-marketing`
(justo lo que aplica al sorteo con Fastwell), `marketing-ideas`, `image`.
El resto (SEO, revops, pricing, paywalls, etc.) queda disponible pero es de menor
uso mientras el trabajo sea contenido de Instagram.

`product-marketing` es la skill base que las demás consultan primero — conviene
tenerla presente (posicionamiento, audiencia) antes de invocar las más específicas.

## 2. ui-ux-pro-max-skill — [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)
(el sitio `ui-ux-pro-max-skill.nextlevelbuilder.io` está bloqueado por la política de
red de este entorno remoto — se instaló desde el repo de GitHub que lo respalda).

Se instaló un subconjunto curado (el repo completo trae también `design-system`,
`slides` y `ui-styling`, enfocados en apps web con React/Tailwind/shadcn — no aplican
al formato actual del proyecto, que son artboards HTML estáticos en el canvas de la
skill `design`; se omitieron para no inflar el repo con ~6MB de fuentes/CSS
irrelevantes. Pedir que se agreguen si en algún momento se necesitan):

- **`ui-ux-pro-max/`** — base de datos consultable de 79 estilos, 192 paletas de
  color, 74 combinaciones tipográficas y 119 lineamientos de UX. Se invoca con
  `python .claude/skills/ui-ux-pro-max/scripts/search.py "<query>" --domain <domain>`
  (ruta ajustada para funcionar como skill de proyecto — el SKILL.md original de
  NextLevelBuilder usa `${CLAUDE_PLUGIN_ROOT}`, variable que solo existe si se
  instala como plugin de marketplace, no como skill de proyecto).
- **`brand/`** — checklist de consistencia de marca, uso de logo, paleta, tono.
- **`banner-design/`** — tamaños y estilos de banners.
- **`creative-assets/`** — (carpeta original del repo se llama `design`; se
  renombró aquí para no chocar con la skill `design` de Claude — el canvas
  multi-artboard que este proyecto ya usa intensivamente, ver `CLAUDE.md`).
  Trae guías de logo, iconos, banners y **`references/social-photos-design.md`**,
  justo el tipo de pieza que se está produciendo.

## 3. stop-slop — [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop)

Elimina "tics" de escritura de IA (relleno, voz pasiva, contrastes formulaicos,
em-dashes, etc.). Útil para revisar el copy de los posts/historias antes de
publicarlos — pásalo sobre cualquier texto final (captions, headlines, CTAs).

## Cómo usarlas

Se activan solas cuando la tarea calza con su descripción (p. ej. pedir "mejora el
copy de este post" dispara `copywriting`/`copy-editing`/`stop-slop`; pedir revisar
la paleta de un artboard dispara `ui-ux-pro-max`). También se pueden invocar
explícitamente con `/nombre-skill` si Claude Code las reconoce como comando, o
simplemente mencionando lo que se necesita.
