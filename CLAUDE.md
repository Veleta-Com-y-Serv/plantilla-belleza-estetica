# Plantilla «Peluquería y estética» (NoExpertise)

Web estática Astro 5 + Tailwind 4, derivada de AstroWind (MIT, ver `LICENSE-ASTROWIND.md`). Estilo: **delicado**.
Todo en castellano de España, frases cortas, sin jerga.

## Dónde está cada cosa
- **Textos, contacto, menú, SEO y secciones**: `site.yaml` (raíz). Es la única fuente de contenido. `src/data/site.ts` lo carga y lo tipa; no pongas textos fijos en los componentes.
- **Colores**: `site.yaml → colores` (primario = rosa empolvado, acento = oliva, fondo, texto). `src/layouts/Base.astro` los convierte en variables CSS (`--c-primario`…, y calcula `--c-sobre-primario`/`--c-sobre-acento`, el color de texto legible encima) y Tailwind los expone como `bg-primario`, `text-acento`, `bg-primario/10`… Los tonos nude y los degradados se derivan con `color-mix` en `src/styles/global.css` (`--nude`, `--nude-suave`, `--oliva-suave`, `--linea`).
- **Tipografías y aspecto general**: `src/styles/global.css` (serif fina y espaciada para títulos, sans para texto; botones píldora `.btn`/`.btn-sec`, `.chip`, `.tarjeta`, `.arco`, `.degradado`, cabecera `.cabecera--centrada` y pie `.pie--claro`). Fuentes del sistema: no añadas fuentes externas.
- **Imágenes**: en `public/`. En `site.yaml` se referencian con ruta absoluta (`/fotos/hero-1.jpg`). Las fotos de la demo se generan a partir de `fotos.json`; mientras una foto `/fotos/x.jpg` no exista, `Imagen.astro` usa su respaldo `/placeholders/x.svg`. Las fotos nuevas del cliente van en `public/fotos/`.
- **Personas**: `personas[].foto` usa la silueta `/placeholders/persona.svg` salvo que el cliente aporte su foto. No se generan caras.
- **Logo**: `site.yaml → logo` (ruta en `public/`); vacío = nombre en texto, centrado en la cabecera y en el pie.
- **Pie «Web creada con NoExpertise»**: `site.yaml → marca_pie` (true en plan gratis).

## Secciones
Cada sección es un componente que solo se pinta si `site.yaml → secciones.<id>.activa` es `true`. Se pintan en el orden en que aparecen en `site.yaml`. `hero` y `contacto` son obligatorias.
- `hero` → `src/components/secciones/hero.astro` (portada con collage)
- `tarifas` → `src/components/secciones/tarifas.astro` (lista de precios por categoría)
- `equipo` → `src/components/secciones/equipo.astro`
- `galeria` → `src/components/secciones/galeria.astro` (rejilla 3 × 3)
- `ofertas` → `src/components/secciones/ofertas.astro` (bonos y promociones)
- `reservas` → `src/components/secciones/reservas.astro` (pedir cita)
- `opiniones` → `src/components/secciones/opiniones.astro`
- `contacto` → `src/components/secciones/contacto.astro`
- Diseños extra para apartados nuevos: `texto` (genérico) y `preguntas` (acordeón).

## Reglas para órdenes
1. **Activar o quitar una sección**: cambia `activa: true|false` en `site.yaml`. No borres componentes.
2. **Reordenar secciones**: mueve el bloque dentro de `secciones:` en `site.yaml`.
3. **Cambiar textos, teléfonos, horarios, precios, fotos**: solo en `site.yaml`. Si un texto contiene comas o dos puntos dentro de `{ … }`, ponlo entre comillas. **Precios siempre como cadena** (`precio: "28 €"`, `precio: "desde 75 €"`); precio vacío = no se muestra.
4. **Listas planas**: nunca una lista dentro de un elemento de lista. Para agrupar tarifas usa el campo `categoria` de cada tarifa (no listas anidadas). Máximo 24 elementos por lista.
5. **Página nueva**: crea `src/pages/<slug>.astro` usando `import Base from '../layouts/Base.astro'` y `<Base titulo="…" descripcion="…">…</Base>`; enlázala en `site.yaml → menu` (`{ texto: …, url: "/<slug>" }`). Ejemplo: `src/pages/aviso-legal.astro`.
6. **Apartado nuevo**: añade `secciones.<id-nuevo>` en `site.yaml` con `tipo:` igual al diseño que quieras (`texto`, `tarifas`, `ofertas`, `preguntas`, `galeria`…). Si ninguno sirve, crea `src/components/secciones/<id>.astro` copiando la estructura de uno existente (`seccion(Astro.props.id)`, `Seccion`, `Titular`). `src/pages/index.astro` lo recoge solo: no lo toques.
7. **No tocar** `src/layouts/Base.astro`, `src/pages/index.astro`, `tailwind.config.mjs` ni `src/styles/comun.css` salvo orden explícita de diseño.
8. Mantén: `lang="es-ES"`, imágenes con `Imagen.astro` (carga diferida), sin JavaScript nuevo salvo necesidad real, 16 px de margen lateral (`.contenedor`), sin scroll horizontal en móvil.
9. El formulario no tiene servidor: abre el correo (`mailto`) con el email de `site.yaml`. Para envío real, cambia `action` en `src/components/ui/FormularioContacto.astro`.
10. Nunca inventes precios, opiniones ni datos que el cliente no haya dado: deja el campo vacío o la lista vacía (los componentes lo ocultan).

## Construir
```
npm ci && npm run build      # resultado en dist/
npx astro preview            # ver dist/ en http://localhost:4321
```
Antes de dar una orden por buena: `npm run build` sin errores.

## Campos propios de esta plantilla (además del esquema común)
- `hero.galeria`: hasta 3 fotos del collage (1.ª vertical 3:4, 2.ª cuadrada, 3.ª apaisada 4:3). Sin galería se usa `hero.imagen`. `hero.items`: chips (solo `titulo`). `hero.cta_secundario` opcional.
- `tarifas.tarifas[]`: `{ nombre, precio, duracion, categoria, texto }`. Se agrupan por `categoria` en el orden en que aparece cada una; con más de una categoría salen chips que llevan a cada grupo. `tarifas.nota` y `tarifas.cta` opcionales. Sin tarifas ni texto, la sección no se pinta.
- `ofertas.ofertas[]`: `{ titulo, texto, precio, validez }` (bonos y promociones). `ofertas.nota`. **Con `ofertas: []` la sección no se pinta.**
- `reservas.items`: pasos (3). `reservas.cta`: botón principal (si su `url` está vacía abre WhatsApp con `contacto.whatsapp`). `reservas.enlace_reservas`: url de una agenda externa (opcional; si existe sale su botón con `enlace_reservas_texto`). `reservas.nota`.
- `equipo.personas[]`: sin personas, la sección no se pinta.
- `opiniones.testimonios[]` (`texto`, `autor`) y `opiniones.nota` opcional. Sin testimonios, la sección no se pinta.
- `galeria.galeria`: rutas de fotos cuadradas (múltiplos de 3 quedan mejor). `galeria.cta` opcional (enlace al perfil de Instagram).
- `contacto.imagen`: plano que enlaza a mapas. `contacto.boton`: texto del botón del formulario.
