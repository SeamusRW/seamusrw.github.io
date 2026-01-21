# Copilot Instructions for seamusrw.github.io

## Project Overview
This is a **multi-project monorepo** hosted on GitHub Pages containing:
- **solar-series**: An Astro 5.16.11 static site (primary active project)
- **brewmuse**: AI Agent project (planned)
- **coffeepal**: AI Agent project (planned)

The repository uses GitHub Actions for CI/CD with security scanning (Fortify AST, dependency review).

## Architecture: Astro File-Based Routing

### Key Directories
- `solar-series/src/pages/` – File-based routing (each `.astro` file → URL route)
- `solar-series/src/components/` – Reusable Astro components
- `solar-series/src/layouts/` – Page wrapper components (e.g., `Layout.astro` provides HTML shell)
- `solar-series/src/assets/` – Static images/SVGs imported into components
- `solar-series/public/` – Copied verbatim to build output (favicon, etc.)

### Component Structure Pattern
Astro uses **frontmatter** (code fence `---`) for server-side logic:
```astro
---
import Logo from '../assets/logo.svg';  // Static imports at build time
---
<div>
  <img src={Logo.src} alt="Logo" />
</div>
<style>
  /* Scoped CSS automatically */
</style>
```

## Critical Development Workflows

### Local Development
```bash
cd solar-series
npm install
npm run dev        # Starts server at http://localhost:4321
npm run build      # Outputs to dist/
npm run preview    # Preview production build locally
```

### TypeScript & Type Safety
- Uses `astro/tsconfigs/strict` (strict mode enabled)
- Type definitions auto-generated in `.astro/types.d.ts` (git-ignored)
- VSCode extension recommended: `astro-build.astro-vscode`

### Deployment
- GitHub Pages auto-deploys from `main` branch via `.github/workflows/static.yml`
- **Note**: `static.yml` is legacy from old static HTML site; will be repurposed for Atom site deployment
- Planned: Atom build workflow will be added to support AI Agent projects

## Project-Specific Patterns

### File Naming Conventions
- Astro components: `.astro` extension (server-rendered)
- Config files: `astro.config.mjs` (ES module)
- No TypeScript config in `solar-series/` root beyond `tsconfig.json`

### Data Import Pattern
Assets are **imported and accessed via `.src` property**:
```astro
---
import bgImage from '../assets/background.svg';
---
<img src={bgImage.src} alt="" />  <!-- Use .src, not direct path -->
```

### Layout Composition
Layouts use `<slot />` for content injection:
```astro
<!-- layouts/Layout.astro -->
<html>
  <body>
    <slot />  <!-- Page content renders here -->
  </body>
</html>
```

## Integration Points

### GitHub Actions Security Scanning
Three workflows in `.github/workflows/`:
1. **static.yml** – Deploys to GitHub Pages (runs on main push)
2. **fortify.yml** – SAST security scanning (requires FOD/SSC credentials)
3. **dependency-review.yml** – Checks for vulnerable dependencies (runs on PRs)

### Astro Build System
- Uses Astro CLI for all operations (not bare Node)
- Build output: `dist/` directory (git-ignored via `.astro/` cache)
- Production output format: `directory` (default; creates index.html per page)

## Common AI Tasks in This Repo

- **Adding pages**: Create new `.astro` files in `src/pages/`
- **Creating components**: Build in `src/components/`, import via relative paths
- **Styling**: Use scoped `<style>` blocks (CSS scoped to component)
- **Adding integrations**: Use `npm run astro add <integration>` (updates config automatically)

## Repository Structure Notes

- **brewmuse/** & **coffeepal/** – Reserved for AI Agent projects (Atom-based)
- Root `.github/workflows/` – Applies to entire monorepo
- Each project is self-contained with its own `package.json` & `node_modules/`
- `solar-series/` uses Astro; AI Agent projects will use Atom framework
