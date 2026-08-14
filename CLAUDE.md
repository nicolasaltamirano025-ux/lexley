# Lexley — sitio web (Del Castillo Pons, S.C.)

Landing page de **Lexley**, despacho legal (nombre fiscal "Del Castillo Pons, S.C.") con sede en Ciudad de México. Sitio informativo de una sola página: presenta a la firma, sus áreas de práctica (cumplimiento normativo, sector privado, litigio, sector público), equipo, clientes y contacto. Sin backend, sin base de datos, sin panel de administración.

## Qué es esto (importante)

Es un **único archivo HTML autocontenido**: [`Landing A.html`](Landing A.html) (~820 líneas: CSS embebido en `<style>`, todo el markup, y JS embebido en `<script>` al final). No hay build step, no hay bundler, no hay `package.json`. Se edita directamente y se despliega tal cual.

Los demás archivos HTML en la raíz (`Landing B.html`, `Landing C.html`, `Landing D.html`, `Propuesta Inicial.html`) son **conceptos de diseño descartados** de la fase de pitch inicial — no están en producción, se conservan solo como referencia histórica. No los toques a menos que te lo pidan explícitamente.

`Landing A - paleta original (oscura) backup 2026-08-06.html` es un **respaldo manual** de una versión de la paleta de colores, guardado antes de una prueba de rediseño que el cliente terminó rechazando. Se conserva por si se quiere retomar esa paleta clara en el futuro.

## Stack

- HTML + CSS + JS vanilla, todo en un solo archivo (`Landing A.html`).
- Sin frameworks, sin dependencias, sin `npm install`.
- Fuentes: system font stacks (Georgia/Palatino para títulos serif, `-apple-system`/Segoe UI para texto sans), no hay webfonts externos.
- Hospedaje: **Vercel** (proyecto `lexley`, team `fully-promoted-qro`).
- Dominio real: **lexley.mx** (DNS apuntando a Vercel vía A record).

## Archivos clave

| Archivo | Qué es |
|---|---|
| `Landing A.html` | **El sitio en producción.** Todo vive aquí: CSS, HTML, JS. |
| `vercel.json` | Rewrite de `/` → `/Landing A.html` (el archivo no se llama `index.html`, así que sin esto el sitio no carga). |
| `assets/brand/lexley-logo.png` | Logo real de Lexley (navy oscuro), se muestra invertido a blanco (`filter:brightness(0) invert(1)`) sobre nav/footer oscuros. |
| `assets/logos/` | ~22 logos de clientes (svg/png/webp) para el carrusel infinito y la sección Clientes. `_originals_backup/` guarda versiones previas de logos que se corrigieron por estar mal (empresa equivocada). |
| `assets/team/` | Fotos de equipo optimizadas (500×500, JPEG). Falta la de Esaú Aguilar Silva (usa iniciales) y semblanza de Carmen Belmont Martínez (pendiente del cliente). |
| `assets/photos/` | Fotografía editorial: edificio (Torre Logar), Ángel de la Independencia, equipo en sala de juntas, y fondos temáticos de Litigio/Sector Público (stock, genéricas). |
| `.env.local` | Solo contiene el `VERCEL_OIDC_TOKEN` que genera la CLI de Vercel automáticamente — no es una variable que la app necesite, no la toques. Está en `.gitignore`. |

## Estructura de secciones (en orden, dentro de `Landing A.html`)

1. **Nav** (`.nav`) — logo + links + CTA
2. **Hero** (`.hero`, `<header>`) — fondo con foto de skyline de CDMX muy tenue (overlay oscuro casi opaco encima)
3. **Logowall** (`.logowall`) — carrusel infinito de logos, se autogenera con JS clonando las imágenes de la sección Clientes (una sola fuente de verdad, no hay que mantener dos listas)
4. **Atmos strip** (`.atmos-strip`) — banner con foto del Ángel de la Independencia
5. `id="firma"` — Nuestra Firma (texto + foto del edificio)
6. `id="cumplimiento"` — Cumplimiento Normativo (incluye la sección "Nuestro Método", un diagrama de 6 pasos hecho en HTML/CSS/SVG puro, no es una imagen)
7. `id="privado"` — Sector Privado
8. `id="litigio"` — Litigio (`.spotlight`, sección oscura con foto de fondo)
9. `id="experiencia"` — Sector Público (estadísticas)
10. `id="equipo"` — Equipo (banner de foto + tarjetas de cada integrante, algunas con modal de semblanza)
11. `id="clientes"` — Clientes (grid de logos, fuente de verdad del logowall)
12. `id="contacto"` — Contacto (mapa de Google embebido, WhatsApp, formulario)
13. **Footer** — logo, redes sociales (pendientes de URL real), dirección
14. **Modales**: Aviso de Privacidad (15 cláusulas LFPDPPP) y semblanzas del equipo (comparten el mismo modal, poblado por JS desde el objeto `bios`)
15. Botón flotante de WhatsApp

## Diseño / paleta de colores

Todo vive en variables CSS dentro de `:root` (arriba del `<style>`):

```css
--ink:#1b262e;      /* fondo oscuro (nav, hero, footer, litigio) */
--ink-2:#24333c;     /* variante del oscuro */
--bone:#efe8d8;      /* texto claro sobre fondo oscuro */
--paper:#f2eee1;     /* fondo principal (beige cálido) */
--paper-2:#e8e0c7;   /* fondo alterno (secciones .alt) */
--line:#cdc2a0;       /* bordes */
--oxide:#8c3b2e;      /* acento (botones, links, CTAs) — terracota */
--oxide-dark:#6d2e24; /* hover del acento */
--soft:#57544a;        /* texto secundario/muted */
```

Estética: editorial/institucional, serif para títulos, paleta cálida oscura (ink navy + beige + terracota). El cliente probó una paleta más clara/fría (grises-azules) y pidió regresar a esta — **no cambiar la paleta sin que el cliente lo pida explícitamente de nuevo** (ver el archivo de respaldo mencionado arriba).

**Patrón de fotos con esquinas redondeadas + sombra** (usado en Firma, Cumplimiento y anteriormente Sector Público): `border-radius:10px; box-shadow:0 16px 32px rgba(27,38,46,.16);` — es el tratamiento estándar para fotos "de acompañamiento" junto a texto. Si agregas una foto nueva junto a un bloque de texto, sigue este patrón.

**Regla de oro del proyecto: cero em-dashes (—).** Antes de cualquier commit, correr:
```bash
grep -c "—\|–" "Landing A.html"
```
Debe dar `0`. El cliente es muy sensible a esto por estilo editorial.

## Reglas de contenido (importante, pedidas explícitamente por el cliente)

- **Nunca inventar contenido**: no fabricar bios, fotos, testimonios ni URLs de redes sociales. Si falta algo, se deja marcado con un comentario `<!-- PENDIENTE: ... -->` o se pregunta.
- Los íconos de redes sociales en el footer están sin URL (`href="#"`) a propósito — están pendientes de que el cliente las proporcione.
- El aviso de privacidad se reprodujo **fielmente** del documento que dio el cliente — no editorializar su contenido aunque se detecte una inconsistencia (se avisa, no se corrige unilateralmente).

## Cómo hacer deploy

El proyecto ya está enlazado a Vercel (carpeta `.vercel/` local, no versionada). Para desplegar a producción:

```bash
cd "Del Castillo Pons"
npx vercel --prod --yes
```

Esto sube `Landing A.html` y todo `assets/` tal cual están en disco — no hay paso de build. Verificar después del deploy:

```bash
curl -s https://lexley.mx/ | grep -c "—\|–"   # debe dar 0
```

No hace falta autenticación adicional si la CLI de Vercel ya tiene sesión iniciada en esta máquina.

## Variables de entorno

**Ninguna es necesaria para que el sitio funcione** — es HTML estático sin backend. El único archivo `.env.local` que existe localmente solo trae el token OIDC que genera Vercel CLI automáticamente para uso interno de la herramienta; no lo necesita la app y no hay que configurarlo en ningún lado nuevo.

## DNS / dominio

`lexley.mx` apunta a Vercel vía registro A (`76.76.21.21`), configurado en Hospedando.mx (cPanel → Zone Editor). El subdominio `mail.lexley.mx` se dejó como registro A fijo apuntando al host anterior para no romper el correo del cliente — **nunca tocar esos registros MX ni el A de `mail.lexley.mx`** al hacer cambios de DNS.

## Pendientes conocidos (no bloqueantes)

- Semblanza de Carmen Belmont Martínez (el cliente la va a conseguir con León).
- Foto profesional de Esaú Aguilar Silva (usa iniciales por ahora, decisión explícita del cliente).
- URLs reales de redes sociales para el footer.
- Posible discrepancia de dominio de correo (`@lexley.mx` vs `@lexley.com`) — ya resuelta: se confirmó que el correcto es `contacto@lexley.mx` en todo el sitio.
