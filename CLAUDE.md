# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev          # Start dev server (Vite)
npm run build        # Type-check + build for production
npm run build-only   # Build without type-check
npm run type-check   # Run vue-tsc type checking
npm run lint         # ESLint with auto-fix
npm run format       # Prettier format src/
npm run test:unit    # Run unit tests (Vitest)
npm run preview      # Preview production build
```

To run a single test file:
```bash
npx vitest run src/__tests__/App.spec.ts
```

Node version requirement: `^20.19.0 || >=22.12.0`

## Architecture

**Single-page portfolio site.** `App.vue` renders only `HomePage.vue`, which composes the five section components: `AboutMe`, `TechStack`, `Experience`, `Contact`. Vue Router is installed but has no routes — the site is entirely one page. Pinia is installed but only a placeholder counter store exists; it is not used by any component.

### CSS — ITCSS

Styles follow the [ITCSS](https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/) (Inverted Triangle CSS) layering inside `src/assets/itcss/`:

| Layer | Folder | Purpose |
|---|---|---|
| Settings | `settings/` | SCSS variables (colors, fonts) |
| Generic | `generic/` | CSS reset, font-face imports |
| Elements | `elements/` | Bare element defaults |
| Components | `components/` | Per-component style partials |

The main entry point `src/assets/index.scss` imports these layers in order. Component-level partials live in `itcss/components/` and are imported globally (not scoped inside `.vue` files).

**Color palette:** GMK Oblivion keycap theme — grays, greens, purples, reds, yellows, cyan, and blue are all defined in `settings/_colors.scss`.

**Fonts:** JetBrains Mono and Fira Mono, loaded via `generic/_reset.scss`.

### Path Alias

`@` resolves to `src/` (configured in both `vite.config.ts` and `tsconfig.app.json`).

### Code Style

- No semicolons, single quotes, 100-character print width (`.prettierrc.json`)
- 2-space indentation, LF line endings (`.editorconfig`)
- ESLint flat config (`eslint.config.ts`) with Vue 3 + TypeScript rules; Prettier formatting conflicts disabled
- Vitest tests go in `src/__tests__/`, environment is jsdom
