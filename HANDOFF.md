# HANDOFF — Demo "Distribuidora del Valle" (Chimichurri)

> Para el agente que continúe (chatgptwork) y para Lucas.
> Objetivo: dejar este demo colgado en los **links oficiales de Chimichurri**
> (meta: `https://chimichurridiseno.com/demoweb`).
> Fecha del handoff: 2026-09-01.

---

## 1) Qué es

Demo 100% funcional de una web para una **distribuidora mayorista de alimentos**.
Tiene 3 piezas que el cliente prueba en vivo, más un tutorial guiado:

- **Tienda** — `index.html` (hero, categorías, catálogo en grilla, carrito → WhatsApp).
- **Catálogo / lista de precios** — `catalogo.html` (lista agrupada por rubro, estilo
  “lista minorista”, con búsqueda, filtro por categoría, orden, botón imprimir/PDF).
- **Panel / CRM** — `admin.html` (login demo, alta/edición/baja de productos, carga
  masiva de fotos, activar/ocultar, export/import JSON). Clave demo: **`demo1234`**.
- **Tutorial guiado** — `tour.js` + `tour.css` (popups secuenciales con spotlight;
  arranca solo la 1ª visita y se reabre con el botón “¿Cómo funciona?”).

**Sin build, sin backend, sin dependencias.** Solo HTML/CSS/JS estático + Google Fonts
(CDN) + emojis. Los datos de cada visitante se guardan en **localStorage** (cada
prospecto juega en su propio sandbox, no pisa a otros). En producción real se puede
enchufar a una base online (ej. Supabase) sin cambiar la UI.

Marca, productos y precios son **genéricos/ficticios**. WhatsApp es un **placeholder**
(`5491100000000`) — se cambia en `config.js`.

---

## 2) Fuente de verdad (código)

- **Repo GitHub:** `https://github.com/lucascasagrande88/demo-distribuidora-del-valle` (rama `main`)
- **Staging actual (GitHub Pages):** `https://lucascasagrande88.github.io/demo-distribuidora-del-valle/`
  - Tienda: `/` · Catálogo: `/catalogo.html` · Panel: `/admin.html`

Árbol de archivos:

```
index.html          Tienda pública
catalogo.html       Catálogo / lista de precios
admin.html          Panel / CRM (login demo: demo1234)
styles.css          Estilos de la tienda + catálogo (paleta verde/crema, Fraunces/Inter)
app.js              Lógica de la tienda (grilla, carrito, tour de la home)
catalog.js          Lógica del catálogo (lista agrupada, carrito, tour del catálogo)
store.js            Guardado local (localStorage) + carrito + sync entre pestañas
config.js           MARCA + categorías + contacto (editar acá)
products-data.js    Catálogo de ejemplo (productos/precios)
tour.js / tour.css  Motor del tutorial guiado
_headers            Cache-Control no-cache (para Netlify)
LEEME.txt           Notas para el cliente
```

Todos los links internos son **relativos** → funciona igual en la raíz o en cualquier
subcarpeta (ej. `/demoweb/`).

---

## 3) Objetivo del deploy: `chimichurridiseno.com/demoweb`

### Contexto de cuentas
- Netlify y GitHub son de **lucas.g.casagrande@gmail.com**.
- El sitio que sirve `chimichurridiseno.com` es el proyecto Netlify **`chimichurridiseno`**
  (site id `cd1ad0e3-64de-4d0a-b9a6-426d46190895`).

### ⚠️ BLOQUEANTE ACTUAL DE NETLIFY
Cualquier deploy nuevo a Netlify (por CLI o API) devuelve:
```
403 — "Account credit usage exceeded - new deploys are blocked until credits are added"
```
Es a **nivel cuenta**. Hay que **sumar crédito / upgradear el plan** en Netlify antes de
poder publicar en el dominio. Esto no depende del código.

### Sitios Netlify vacíos ya creados (de intentos previos — reusar o borrar)
- `distribuidora-del-valle-demo` — id `627e1753-d80e-4517-ba31-84b1ed6ddd22`
- `dv-demo-chimichurri` — id `ba427000-a4a5-46d4-ac62-58f8d1025a70`
(Ninguno tiene deploy exitoso por el bloqueo de crédito.)

### Dos formas de dejarlo en `/demoweb` (elegir una)

**Opción A — Subcarpeta en el repo del sitio oficial (recomendada si chimichurridiseno
se buildea desde GitHub):**
1. Copiar el contenido de este repo dentro del repo de `chimichurridiseno`, en una carpeta
   `demoweb/` (todos los archivos estáticos, sin `.git` ni `HANDOFF.md`/`LEEME.txt` si no
   se quieren públicos).
2. Commit + push → Netlify rebuildea → queda en `chimichurridiseno.com/demoweb/`.
3. No tocar nada más: los paths relativos ya resuelven bajo `/demoweb`.

**Opción B — Deploy propio + rewrite (si NO se quiere mezclar repos):**
1. Publicar este repo como su propio sitio Netlify (ej. reusar `distribuidora-del-valle-demo`).
2. En el sitio `chimichurridiseno`, agregar un rewrite en `netlify.toml` (o `_redirects`):
   ```
   /demoweb/*  https://distribuidora-del-valle-demo.netlify.app/:splat  200
   ```
3. Redeploy de chimichurridiseno → `chimichurridiseno.com/demoweb` sirve el demo sin tocar
   el home.

> Falta el dato de **cómo se deploya hoy chimichurridiseno** (repo GitHub conectado vs
> subida manual). Si es repo conectado → Opción A es directa. Si es manual → Opción B.
> **NO** deployar este demo directo al site `chimichurridiseno` sin subcarpeta: pisaría el
> home (los deploys de Netlify reemplazan todo el sitio).

---

## 4) Personalización rápida (todo en un solo lugar)
- `config.js`:
  - `DV_BRAND` → nombre, WhatsApp (`whatsapp` = solo dígitos, `whatsappPretty` = visible),
    email, dirección, horario, pedido mínimo.
  - `DV_CATS` → categorías (clave, label, emoji, color).
  - `DV_UNITS` → presentaciones sugeridas para el panel.
- `products-data.js` → catálogo de ejemplo (`n` nombre, `p` precio, `cat`, `unit`, `emoji`).
- `styles.css` → paleta y tipografías.

**Caché:** los assets van con `?v=N` en `index.html` / `catalogo.html`. Si editás CSS/JS,
**subí ese número** para invalidar caché en los clientes.

---

## 5) Estado / QA
- ✅ Tienda, catálogo, panel y tutorial funcionando (verificado en vivo).
- ✅ **Responsive mobile arreglado** (Categorías pasó a 1 columna; filas del catálogo se
  apilan). Verificado en viewport de teléfono.
- ✅ Catálogo online publicado en el staging de GitHub Pages.
- ⏳ Falta: publicar en dominio oficial (bloqueado por crédito Netlify).

## 6) Notas
- La “cementera” (`grupodelsurcementera-minorista.netlify.app`) fue **solo referencia de
  formato** para el catálogo (lista agrupada por rubro). No hay que construirla.
- No hay tokens ni secretos en este repo.
```
