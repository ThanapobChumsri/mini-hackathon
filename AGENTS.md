# Repository Guidelines

## Project Structure & Module Organization
This Nuxt 3 app keeps entry points in `pages/` (routes like `index.vue` and `login.vue`). Shared UI lives inside `components/` (e.g., `dnd.vue`, `sideBar.vue`), while layout scaffolding goes in `layouts/`. Static assets and global styles sit under `assets/` (`css/main.css`) and `public/` for files served as-is. Nuxt module and runtime configuration is centralized in `nuxt.config.ts`, and any server routes or API handlers belong in `server/`. Use `middleware/` for navigation guards and `app.vue` for root-level composition.

## Build, Test, and Development Commands
- `npm install` installs dependencies and also runs `nuxt prepare`.
- `npm run dev` starts the local development server with hot reload.
- `npm run build` generates the production build output in `.output/`.
- `npm run preview` serves the built bundle for final smoke tests.
- `npm run generate` produces a static site when applicable.

## Coding Style & Naming Conventions
Use Vue single-file components with two-space indentation, script blocks at the top and scoped styles when necessary. Follow the existing pattern of lower camel component filenames (`sideBar.vue`, `markerArray.vue`) and favour descriptive prop names. Keep template markup class-first to leverage Tailwind-style utilities and avoid inline style blocks unless unavoidable. Run `@nuxt/ui` primitives for consistent styling, and factor shared logic into composables once they emerge.

## Testing Guidelines
Automated tests are not configured yet; add Vitest + @nuxt/test-utils when introducing complex logic. Until then, supply manual test notes in pull requests and smoke test via `npm run dev` and `npm run preview`. When adding tests, mirror the feature structure (`components/__tests__/dnd.spec.ts`) and name cases using `should` statements.

## Commit & Pull Request Guidelines
Commits in this repo use short, lowercase imperatives (`add drag and drop component`). Bundle related work into focused commits and avoid mixing refactors with features. Pull requests should describe the change impact, list manual checks, and link any design references or issues. Include screenshots or GIFs for UI adjustments and call out new environment variables.

## Security & Configuration Notes
Do not commit secrets; store runtime configuration in untracked `.env` files. Review `nuxt.config.ts` changes for module side effects and ensure static assets are optimized before adding them to `public/`.
