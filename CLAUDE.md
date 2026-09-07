# Motion Lab — Instagram Stories / Content Pipeline

Contexto para retomar este proyecto en Claude Code. Léelo completo antes de tocar
cualquier `.dc.html` — evita repetir errores/decisiones ya resueltas con el cliente.

## Quién y qué

Motion Lab es una clínica de fisioterapia premium en zona 14, Ciudad de Guatemala
(fisiomotionlab.com). Erick (el dueño) me pidió producir "historias" (Instagram
Stories, 9:16, 24h) para atraer pacientes nuevos y generar citas — no solo
promos/descuentos, que era casi todo su contenido anterior.

Contenido pilar de marca: Escuchar → Medir → Tratar → Progresar.
Slogan: "Recuperación medida, trato cercano." Fisioterapeuta: Johana Juárez.

## Dónde vive el trabajo

- El canvas de diseño (multi-artboard) está publicado como Artifact en:
  `https://claude.ai/code/artifact/3decb23e-0225-4702-9763-829ae48ed3e8`
  (privado, propiedad de Erick). El HTML publicado contiene TODO el estado
  editable — cada artboard `.dc.html`, `canvas.json` y el título — dentro de un
  `<script id="appifact-doc">`. Si necesitas partir de la versión más reciente
  y no de este paquete, léelo primero desde ahí.
- Este repo trae, en la raíz, `motion-lab-claude-code-handoff.zip` — una copia de
  trabajo de los archivos fuente tal como estaban en la última sesión: los
  `.dc.html` de cada artboard (dentro de `serie-equipo/canvas/`), `canvas.json`, y
  las imágenes referenciadas. Las imágenes compartidas (`logo-small.png`, fotos
  `_story.jpg`, cutouts `.png`) viven un nivel arriba de `canvas/` dentro del zip;
  los `.dc.html` las referencian con `../nombre.png` — respeta esa estructura de
  carpetas al descomprimir/reseedear. `motion-lab-historias-artifact.html` en la
  raíz del repo es una copia exportada del Artifact de arriba (mismo contenido,
  verificado byte a byte contra el zip al 2026-09-07).
- Brand assets originales (logo, manual de marca, fotos reales de Johana, del
  equipo, tratamientos) están en Google Drive, carpeta "Creacion Contenido"
  (dueño motionlabsa.gt@gmail.com), subcarpeta "IMAGENES HISTORIAS" para fotos
  ya recortadas listas para usar.

## Skills instaladas para mejorar copy/diseño

Además de la skill `design` (canvas de Claude Design, ver abajo), este repo trae
skills de proyecto en `.claude/skills/` — se cargan solas en cualquier sesión
abierta aquí. Detalle completo y de dónde viene cada una en
`.claude/skills/README.md`; resumen:

- **Marketing** (de [coreyhaines31/marketingskills](https://github.com/coreyhaines31/marketingskills)):
  50 skills — las más útiles para este proyecto son `social`, `ad-creative`,
  `copywriting`, `copy-editing`, `offers`, `co-marketing`, `marketing-ideas`, `image`.
- **`ui-ux-pro-max`** (de [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)):
  base de datos consultable de estilos/paletas/tipografía/UX. Útil para elegir
  paleta o validar contraste antes de maquetar una pieza nueva.
- **`brand`**, **`banner-design`**, **`creative-assets`** (mismo repo anterior,
  subset curado — `creative-assets` es la carpeta `design` original, renombrada
  para no chocar con la skill `design` de Claude de abajo).
- **`stop-slop`** (de [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop)):
  revisar el copy final (captions, headlines, CTAs) para quitar tics de escritura
  de IA antes de publicar.

## Herramienta: skill "design" (canvas de Claude Design)

Cada historia es un artboard `1080x1920` (`.dc.html`), todos en un mismo
`canvas.json` con `pages` (categorías) y `artboards` (x/y/w/h/page/file).
`seed-canvas.mjs` (parte de la skill `design`) empaqueta todo en un único HTML
publicable. Detalles operativos:

- Comando de seed necesita `--template`, `--title`, `--out`, y al menos un
  `--artboard` (o `--check archivo.html` solo, para validar).
- Ejecutar desde `canvas/` como cwd. Las rutas de `--image` son relativas a ese
  cwd: los assets compartidos un nivel arriba necesitan prefijo `../`
  (`--image ../logo-small.png`); los que ya están copiados dentro de `canvas/`
  van sin prefijo.
- Solo pasar `--image` para los que un `<img src>` realmente use — verificar con
  `grep -o 'src="[^"]*"' canvas/*.dc.html`.
- Preview de un solo artboard sin tocar el archivo de producción: un
  `--canvas archivo.json` mínimo con el w/h real del artboard y
  `"launch":{"view":"focused","file":"X.dc.html"}` (sin `"page"` cuando
  `view` es `"focused"`).
- Export a PNG / preview visual: Playwright con
  `executablePath:'/opt/pw-browsers/chromium'`, viewport 1400x1000,
  `waitForTimeout` de varios segundos antes de interactuar, click en
  `text=Export` → `text=PNG 1×`, capturar el evento `download`.
- Publicar: `Artifact action:"read"` sobre la URL SIEMPRE antes de
  `action:"publish"` (si no, lo rechaza), `contract:"0.1.31"` (o el que reporte
  como vigente).
- Advertencia de tamaño: mantener cada imagen bajo ~70 KB en base64
  (base64 ≈ tamaño del archivo × 4/3). PNG con transparencia (cutouts de
  personas): PIL `.quantize(colors=N, method=Image.MEDIANCUT,
  dither=Image.FLOYDSTEINBERG)` + reaplicar el canal alfa — el dithering evita
  bandas/posterizado visible en tonos de piel. JPG full-bleed (fotos de fondo
  completo): ~720x1280 a calidad 70-75 suele dar 45-65 KB.

## Sistema de marca (fijo, no cambiar sin que Erick lo pida)

- Canvas `1080x1920`, gradiente de fondo
  `linear-gradient(150deg, #1E5490 0%, #123A6B 45%, #0A1F3D 100%)`, overlay de
  grano SVG (`feTurbulence`, opacity .05).
- Logo `logo-small.png`: `top:270px; left:56px; width:320px`.
- Fuentes: Poppins (400–800) para texto, IBM Plex Mono (500/600) para
  eyebrows/labels/números.
- Colores: navy `#0A1F3D`/`#173C69`, cyan `#3ABFDF`, `#1C7FA3`, slate `#33465E`,
  rojo/error `#E14B4B`.
- Zona segura de Instagram: mantener texto/logo/CTA importantes entre
  y:250–1670 (Instagram tapa los 250px de arriba y abajo con su UI).

## Catálogo de estilos ya construidos

1. **Editorial cutout + tarjeta superpuesta** (figura vertical de pie/corriendo):
   truco de dos capas — un div ANTES del `<img>` (fondo blanco+sombra, texto
   duplicado en `color:transparent` solo para que el bloque tenga la altura
   correcta) + el `<img>` del cutout + un div DESPUÉS del `<img>`
   (`pointer-events:none`, el texto real visible) — sin z-index explícito, el
   orden del DOM controla el apilado. Ejemplos: `Johana.dc.html`,
   `MitoDescanso.dc.html`, la serie `carrera`.
2. **Checklist / autoevaluación** (sin foto): tarjeta blanca con filas de
   casillas (`32x32px`, `border:3px solid #3ABFDF`), pregunta de reflexión al
   final, CTA cerca del borde inferior de la tarjeta (ajustar `bottom` según la
   altura real de la tarjeta para no dejar espacio vacío de más).
   Ejemplo: `ChecklistPostura.dc.html`.
3. **"Mito vs. realidad"**: headline en cursiva (la cita/mito) + remate en
   negrita, badge circular rojo con "X" (74x74px, `#E14B4B`) + pill oscuro de
   contexto, tarjeta blanca con dato clave en negritas, banner inferior de
   ancho completo inclinado (`transform:skewY(-1deg)` con contra-giro interno
   `skewY(1deg)`) con el takeaway. Usa `{{accent}}` vía `data-dc-script`/
   `DCLogic` para color editable. Ejemplos: `MitoDescanso.dc.html`,
   `MitoRodillas.dc.html`.
4. **Tarjeta al lado de la foto** (para fotos de composición ancha/sentada, NO
   una figura vertical): antes de maquetar, analiza el bounding box real de
   contenido no-transparente de la imagen (PIL `im.getbbox()`) para encontrar
   dónde cae el espacio vacío genuino dentro del cuadro, y coloca ahí una
   tarjeta simple (sin el truco de dos capas — no hace falta si el área está
   realmente vacía). Ejemplo: `HomeOffice.dc.html`. Regla explícita de Erick:
   **nunca repliques el mismo patrón de foto centrada/superpuesta en toda foto
   nueva — el layout se diseña según la composición real de esa foto
   específica** ("no solo replicar y poner en el centro porque se vera mal").
5. **Foto completa ("ligero")**: foto full-bleed con `object-fit:cover`,
   scrims de gradiente arriba/abajo para legibilidad, logo + eyebrow +
   headline + subline cerca del borde inferior. Usado para fotos ya "resueltas"
   (una escena completa, no un cutout) — ver `Main.dc.html`, y la pieza
   `SorteoStory.dc.html` (foto de producto + señalización de la clínica).

### Regla de texto: wrap por fila, no columna única

No fuerces todo el texto a una sola columna angosta (genera saltos de línea
feos). Dale al bloque de lista/ítems un solo contenedor flexible más ancho y
deja que cada fila envuelva de forma independiente:

```html
<div style="display:flex; flex-direction:column; gap:10px; width:580px;">
  <div style="display:flex; align-items:flex-start; gap:14px;">
    <span style="flex:0 0 auto;">01</span>
    <span style="flex:1 1 auto; min-width:0;">Texto que envuelve naturalmente</span>
  </div>
</div>
```

Antes de fijar el ancho, mide el string más largo candidato con Playwright
(`getBoundingClientRect().width`) al tamaño de fuente real, y elige un ancho
donde los ítems cortos queden en una sola línea y el más largo envuelva en un
punto natural (preposición/conjunción), no a mitad de palabra.

### Principio de diseño (evitar el "look genérico de IA")

Erick pidió explícitamente que dejemos de repetir la fórmula "tarjeta blanca
redondeada + sombra + rotación leve" en automático. Antes de maquetar una pieza
nueva: define paleta (4-6 colores con nombre), tipografía (roles claros) y
concepto de layout ancla dos en el tema real (fisioterapia/Motion Lab, no
genérico), y solo entonces construye. Evitar clichés de diseño-IA: beige+serif+
terracota, negro con un solo acento neón, gradiente morado-a-azul, Inter/Space
Grotesk como fuente "segura", todo centrado, `rounded-lg` en todo, emojis como
separador de sección. Los numerados (01/02/03) solo si el contenido es
realmente una secuencia. La skill `ui-ux-pro-max` (`.claude/skills/ui-ux-pro-max/`)
ayuda a elegir paleta/tipografía con intención en vez de caer en estos clichés; la
skill `stop-slop` ayuda a lo mismo pero en el copy.

## Flujo de fotos con personas reales

Erick genera la foto en Canva (Magic Media) y él mismo remueve el fondo (su
calidad de recorte es mejor que los intentos automáticos) → sube el PNG ya
recortado a la carpeta de Drive "IMAGENES HISTORIAS", o lo pasa directo en el
chat. Yo doy el prompt descriptivo de Canva cuando hace falta una foto nueva.
Prompt de ejemplo que funcionó bien a la primera (para "home office"):
"Fotografía realista de una mujer latina de unos 30 años, ropa casual cómoda...
sentada frente a un escritorio con laptop, haciendo un estiramiento de
cuello... Fondo neutro y liso... para poder recortar la figura fácilmente..."

## Campaña activa: sorteo con Fastwell (EN PROGRESO)

Sorteo de 2 roll-on Fastwell, co-branded. Mecánica confirmada por Erick:
requisito mínimo seguir ambas cuentas de Instagram, + dar like, + comentar
respondiendo una pregunta, + compartir el post a su historia etiquetando a
ambas cuentas. Formato: un post de feed (`SorteoPost.dc.html`, 1080x1080) +
una historia que empuja al post (`SorteoStory.dc.html`, 1080x1920), NO la
secuencia completa de anuncio+recordatorio+ganador.

Pendiente / placeholders que Erick debe corregir (son texto plano, editables
directo en el canvas sin re-publicar todo):
- Handles de Instagram puestos como placeholder: `@motionlab.gt` y
  `@fastwell.gt` — confirmar los reales.
- Fecha de cierre puesta como placeholder: "19 de septiembre".
- Fastwell aún no ha compartido su logo/colores de marca — el chip
  "Motion Lab × Fastwell" es texto plano por ahora; en cuanto Erick lo consiga,
  integrarlo visualmente en vez del texto.

## Otras decisiones/aprendizajes ya resueltos (no re-litigar)

- "Historias" = Instagram Stories (9:16, efímero), no publicaciones de feed.
- Las tarjetas de texto NUNCA deben tocar/superponerse a una persona en la
  foto — ni con halo de sombra de texto (Erick lo rechazó explícitamente,
  "se ve mal"); el texto debe quedar estructuralmente despejado.
- Mantener tamaños de texto grandes: logo 320px, eyebrows ~26-40px, headlines
  ~60-88px, cuerpo de tarjeta ~25-34px — usar `Johana.dc.html` /
  `MitoDescanso.dc.html` como referencia de escala.
- Evitar huecos vacíos grandes de gradiente entre el headline y la
  tarjeta/CTA — ajustar posiciones tras una revisión visual en vez de dejar
  espacio de sobra "por si acaso".
- "Descarga" (en el contexto de la serie carrera) es un tratamiento/servicio
  de la clínica (masaje de recuperación), NO reducción de volumen de
  entrenamiento — confirmado por Erick como dueño de la clínica: agendar la
  descarga pre-carrera al menos 3 días antes de la carrera, y la post-carrera
  24–48h después de terminar.

## Cómo retomar en Claude Code

1. Descomprime `motion-lab-claude-code-handoff.zip` (o lee el Artifact publicado)
   para ver `serie-equipo/canvas/canvas.json` — 6 páginas y 17 artboards actuales
   (incluida la página `sorteo` con `SorteoPost.dc.html` + `SorteoStory.dc.html`).
2. Para editar un artboard existente: abre su `.dc.html`, edita, y re-preview
   en modo focused antes de tocar el archivo de producción completo.
3. Para agregar uno nuevo: decide primero cuál de los 5 estilos del catálogo
   encaja (o si hace falta uno nuevo, documenta el porqué antes de construir).
4. Antes de publicar: `Artifact action:"read"` sobre la URL de arriba, edita
   sobre esa versión, y publica con `contract` igual o el que reporte vigente.
5. Si el Artifact tiene comentarios o fue editado por Erick directamente desde
   el editor visual (guardado interactivo), esa versión manda — vuelve a leer
   antes de publicar encima.
