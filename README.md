# Vite, Svelte, tailwind static website scaffolding

## Alternative steps to reproduce to get a working homepage:

1. Create basic vite site

```bash
npm create vite@latest . -- --template svelte-ts
```

2. Remove unneeded stuff like:
   - `.vscode/`
   - `public/`
   - `src/assets`
   - `src/lib`
3. Replace content of `App.svelte` with:

```ts
<main>
  <h1>Hello World</h1>
  <p>Welcome to my website</p>
</main>
```

4. Install tailwind

```bash
npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
```

5. Initialize tailwind

```bash
npx tailwindcss init -p
```

6. Update content of `tailwind.config.cjs` with:

```cjs
module.exports = {
  content: ["./index.html", "./src/**/*.{html,js,ts,svelte}"],
```

7. Replace the content of `app.css` with:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

8. Feel free to add tailwind classes e.g. in `App.svelte`

```ts
<main>
  <h1 class="font-bold">Hello World</h1>
  <p>Welcome to my website</p>
</main>
```

9. Start development server

```bash
npm run dev
```

## Building and deploying to github pages

Navigate to your repository's settings page under `pages` set source to `GitHub Actions` and define your own action something like so:

```yaml
name: Github Pages build and deployment

on:
  push:
    branches:
      - main

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: 19

      - name: Install dependencies
        run: npm install

      - name: Build project
        run: npm run build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: ./dist

  deploy:
    needs: build

    permissions:
      pages: write
      id-token: write

    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1
```
