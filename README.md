# The Wave Principle — Lector en voz alta

Aplicación web (Next.js) para escuchar el libro *"The Wave Principle"* de R. N. Elliott (1938) de forma auditiva, mostrando la página escaneada original y desplazándose automáticamente mientras se lee.

Basada en [elliot-wave-app](https://github.com/rod773/elliot-wave-app) (lector del libro de Frost & Prechter), adaptada a este libro en inglés.

## Características

- **Lectura en voz alta** con la API nativa `speechSynthesis` del navegador (sin servicios externos).
- **Voz en inglés**: prefiere voces `en-US`, con fallback a otras voces en inglés.
- **Selección de voz**, **control de volumen**, **auto-avance** a la siguiente página.
- **Auto-scroll**: la vista se desplaza en proporción al progreso de la lectura.
- **Navegación manual** (Anterior / Siguiente) y selector de página.
- **Imágenes originales**: cada página se muestra como imagen escaneada (`public/pages/page-NNN.png`).

## Cómo funciona

- `src/app/page.tsx` contiene toda la lógica de la aplicación.
- El texto de cada página proviene de `src/pages.json` (205 páginas con `{ page, text }`), generado por OCR del PDF (`npm run generate`).
- El texto se divide en fragmentos; cada uno se envuelve en un `SpeechSynthesisUtterance` y se encadenan por `onend`.
- Se estima la duración de cada fragmento (`max(6000, longitud * 150)`) para calcular el progreso y animar el scroll.

## Ejecutar en local

```bash
npm install
npm run dev
```

Abre [http://localhost:3000](http://localhost:3000).

## Scripts

| Comando | Descripción |
| --- | --- |
| `npm run dev` | Servidor de desarrollo |
| `npm run build` | Build de producción |
| `npm start` | Servir el build de producción |
| `npm run lint` | Linter (ESLint) |
| `npm run generate` | Regenera `src/pages.json` desde `../the-wave-principle/texts` |

## Estructura

```
the-wave-app/
├── public/
│   └── pages/
│       ├── page-001.png ... page-205.png   # Páginas escaneadas
├── scripts/
│   └── generate-pages.cjs                  # Generador de pages.json
├── src/
│   ├── app/
│   │   ├── layout.tsx      # Layout raíz (fuentes Geist, metadata)
│   │   ├── page.tsx        # Toda la lógica de lectura y UI
│   │   └── globals.css     # Estilos globales (Tailwind + tema claro/oscuro)
│   └── pages.json          # Texto de las 205 páginas
├── package.json
└── README.md
```

## Notas

- El contenido escaneado e imágenes pertenecen a la edición original del libro y se usan aquí únicamente con fines personales/educativos.
- La lectura en voz alta depende de las voces instaladas en el navegador/sistema.
