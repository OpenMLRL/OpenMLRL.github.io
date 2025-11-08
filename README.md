# OpenMLRL.github.io

Static site for the OpenMLRL collective built with [Hugo](https://gohugo.io/) and
the [`hugo-theme-gallery`](https://themes.gohugo.io/themes/hugo-theme-gallery/)
template. The repository mirrors the layout that powers the `docs/` site inside
[`OpenMLRL/CoMLRL`](https://github.com/OpenMLRL/CoMLRL/tree/main/docs): only the
source files live in Git, while the rendered HTML is produced in CI.

## Local development

1. Install the Hugo **extended** binary (already available via Homebrew).
2. Pull the theme if needed: `git submodule update --init --recursive`.
3. Start the hot-reload server:

   ```bash
   hugo server --buildDrafts
   ```

   The site becomes available at <http://localhost:1313>. Hugo keeps the build
   in memory, so no files inside `public/` are written to disk.

To create a production build locally, run `hugo --gc --minify`. The output still
lands inside `public/`, which is ignored by Git so it will not be committed by
mistake.

## Deploy pipeline

GitHub Pages publishes the site via `.github/workflows/pages.yml`:

- triggers on pushes to `main` (and manual dispatches)
- checks out this repo plus the gallery theme submodule
- installs Hugo Extended and builds the site with the base URL supplied by the
  Pages runtime
- uploads the generated `public/` folder as an artifact
- deploys the artifact to the `github-pages` environment using
  `actions/deploy-pages@v4`

After merging to `main`, ensure the repository settings pick **GitHub Actions**
as the Pages source and point to the `github-pages` environment once the first
run succeeds.

## Customizing content

- Albums live inside `content/<album>/index.md` as [page bundles](https://gohugo.io/content-management/page-bundles/).
  Each bundle carries its own PNG placeholders so you can swap them for real
  lab photos or diagrams without touching layout code.
- Category landing pages stay under `content/categories/*`. They include cover
  images so the `/categories` route renders properly.
- Global parameters (title, description, footer links, gallery layout) are set
  in `hugo.toml`.

Replacing the placeholder artwork is as simple as dropping your own `.jpg` or
`.png` files inside the relevant bundle directories and referencing them in the
`resources` list.
