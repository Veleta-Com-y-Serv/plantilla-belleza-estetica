# Plantilla NoExpertise · Peluquería y estética

Web delicada para peluquerías, centros de estética y uñas, con tarifas, bonos y cita por WhatsApp.

- Sectores: peluqueria, estetica, belleza, spa, unas, barberia. Estilo: delicado. Plan mínimo: gratis.
- Secciones: hero, tarifas, equipo, galeria, ofertas, reservas, opiniones, preguntas, contacto (más el diseño `texto` para apartados nuevos).
- Campos propios: `tarifas[]` (agrupadas por `categoria`), `ofertas[]`, `reservas.enlace_reservas`, `hero.galeria` (collage de 3 fotos).
- SEO de base incluido (`src/lib/seo.ts`): canónica, og:image, JSON-LD del negocio y de las preguntas frecuentes, `robots.txt`, `sitemap.xml` y `llms.txt`; `noindex` si `seo.indexable: false`.
- Contenido editable en `site.yaml`; metadatos en `template.yaml`; reglas para el agente en `CLAUDE.md`; fotos de la demo en `fotos.json`.

```
npm ci
npm run dev      # desarrollo
npm run build    # web estática en dist/
```

Basada en [AstroWind](https://github.com/onwidget/astrowind) © onWidget, licencia MIT (`LICENSE-ASTROWIND.md`). Imágenes de relleno (`public/placeholders/`) generadas para esta plantilla, sin licencias de terceros.
