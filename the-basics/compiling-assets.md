Masonite uses [Vite](https://vite.dev) to compile your frontend assets, with [Tailwind CSS](https://tailwindcss.com) preconfigured. You don't need to be an expert in either Vite or NPM to compile assets.

!!! note
    Projects generated with Masonite ≤ 5.2 used `laravel-mix`. Existing projects keep working — the `mix()` template helper is still available — but new projects ship with Vite. To migrate an existing project, copy the `vite.config.js`, `package.json`, `resources/js` and `resources/css` files from a freshly generated project.

## Getting Started

To get started we can simply run NPM install:

```text
$ npm install
```

This will install everything you need to start compiling assets.

## Configuration

The configuration lives in the `vite.config.js` file located in the root of your project:

```javascript
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'
import { fileURLToPath, URL } from 'node:url'

export default defineConfig({
  plugins: [tailwindcss()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./resources/js', import.meta.url)),
    },
  },
  build: {
    outDir: 'storage/compiled',
    // ...
  },
})
```

This compiles `resources/js/app.js` (which imports `resources/css/app.css`) into the `storage/compiled` directory, which Masonite serves under `/static`:

```html
<link href="/static/css/app.css" rel="stylesheet">
<script src="/static/js/app.js"></script>
```

For additional configuration values take a look at the [Vite documentation](https://vite.dev/config/).

### Tailwind CSS

Tailwind CSS 4 comes preconfigured through the `@tailwindcss/vite` plugin. Tailwind 4 is configured directly in CSS — `resources/css/app.css` imports Tailwind and tells it where your templates live:

```css
@import "tailwindcss";

@source "../../templates";
```

Any utility class used in your templates or JavaScript will be included in the compiled CSS automatically. See the [Tailwind CSS documentation](https://tailwindcss.com/docs) for theming and customization.

### Installing Vue

Install the Vue plugin for Vite and register it in `vite.config.js`:

```text
$ npm install --save-dev vue @vitejs/plugin-vue
```

```javascript
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [tailwindcss(), vue()],
  // ...
})
```

## Compiling

Now that we have our compiled assets configured we can now actually compile them.

You can do so by running:

```text
$ npm run production
```

This will compile the assets and put them in the `storage/compiled` directory.

You can also have NPM wait for changes and recompile when changes are detected in frontend files. This is similar to an auto reloading server. To do this just run:

```text
$ npm run watch
```

## Legacy projects (`laravel-mix`)

Projects created before Masonite 5.3 build their assets with the [`laravel-mix`](https://laravel-mix.com) package. The `mix()` template helper resolves hashed filenames from the Mix manifest and remains available:

```html
<script src="{{ mix('resources/js/app.js') }}"></script>
```
