# Racass Blog

Academic portfolio, teaching materials, and articles built with
[Astro](https://astro.build/) and deployed to GitHub Pages.

## Local development

```sh
npm install
npm run dev
```

The production build is generated with `npm run build`.

## Publishing

Pushes to `main` trigger `.github/workflows/deploy.yml`. The site is published at:

<https://racass.github.io/>

## Analytics with Umami Cloud

1. Add the deployed site to [Umami Cloud](https://cloud.umami.is/).
2. Copy its website ID.
3. In the GitHub repository, open **Settings > Secrets and variables > Actions > Variables**.
4. Create `PUBLIC_UMAMI_WEBSITE_ID` with that website ID.
5. Run the deployment workflow again.

## Comments with Cusdis

1. Add the site in the [Cusdis dashboard](https://cusdis.com/dashboard).
2. Copy its application ID.
3. Create the repository variable `PUBLIC_CUSDIS_APP_ID`.
4. Run the deployment workflow again.

Comments are enabled on article pages and remain hidden until the variable is configured.

## Content

- Add articles as Markdown or MDX files under `src/content/blog/`.
- Edit the landing page in `src/pages/index.astro`.
- Edit the biography in `src/pages/about.astro`.
- Update the site title and description in `src/consts.ts`.
