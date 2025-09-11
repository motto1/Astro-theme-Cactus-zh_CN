# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

This is **Astro 仙人掌** (Astro Cactus), a Chinese localized version of the Astro Cactus blog theme. It's an Astro-based static site generator with integrated Decap CMS for web-based content management, using TailwindCSS for styling and supporting both blog posts and notes.

## Essential Commands

### Development Workflow
```bash
pnpm install          # Install dependencies
pnpm dev              # Start dev server at localhost:4321 (Astro 5+ default)
pnpm build            # Build for production
pnpm postbuild        # Generate search index with Pagefind
pnpm preview          # Preview production build locally
```

### Code Quality
```bash
pnpm lint             # Lint with Biome
pnpm format           # Format code and organize imports
pnpm check            # Type check with Astro
```

### Content Management
```bash
pnpm sync             # Generate types from content config
```

## Core Architecture

### Content Collections
- **Posts**: Located in `src/content/post/` - full blog articles with tags, dates, and rich metadata
- **Notes**: Located in `src/content/note/` - simpler content with title, description, and publish date
- Both collections support Markdown/MDX with frontmatter validation via Zod schemas

### Key Configuration Files
- `src/site.config.ts` - Site metadata, navigation menu, and code theme settings
- `src/content.config.ts` - Content collection schemas and validation rules  
- `astro.config.ts` - Astro framework config, integrations, and plugins
- `public/admin/config.yml` - Decap CMS configuration for web editing

### Routing Structure
- Dynamic routes use Astro's file-based routing with `[...slug].astro` and `[...page].astro` patterns
- Posts: `/posts/[slug]` and paginated `/posts/page/[n]`
- Notes: `/notes/[slug]` and paginated `/notes/page/[n]`  
- Tags: `/tags/[tag]/page/[n]` for tag-filtered content
- RSS feeds generated for both content types

### Styling Architecture
- TailwindCSS with custom theme extending base colors via CSS custom properties
- Custom typography plugin with comprehensive admonition support (note, tip, warning, caution, important)
- Dark/light theme toggle with data attributes (`[data-theme="dark"]`)
- Responsive design with mobile-first approach

### Markdown Processing Pipeline
- **Remark plugins**: 
  - Custom reading time calculator
  - Directive processing for admonitions (`::: note`, `::: warning`, etc.)
  - Math support (LaTeX via remark-math)  
  - Emoji support (remark-gemoji)
- **Rehype plugins**:
  - External link processing with security attributes
  - Image unwrapping for better layout
  - KaTeX rendering for math expressions

### Decap CMS Integration
- Web-based content editor at `/admin` route
- OAuth integration with GitHub for authentication
- Configured for both posts and notes with Chinese UI labels
- Media uploads stored in `public/assets/images/`

## Development Notes

### Content Creation
- Posts require: title, description, publishDate, tags, and optional coverImage
- Notes require: title, publishDate, and optional description  
- Draft posts (with `draft: true`) only visible in development mode
- Custom date format handling for notes supports both ISO and "YYYY-MM-DD HH:mm" formats

### Search Functionality  
- Pagefind generates static search index during build (`postbuild` script)
- Search UI component integrates with generated index
- Index built from `/dist/client` and output to Vercel-compatible path

### Deployment Configuration
- Configured for Vercel with server-side rendering (`output: 'server'`)
- Environment variables for OAuth and webmention integration
- Social card generation via Satori for dynamic OG images

### Code Organization Patterns
- Components in `src/components/` with logical grouping (layout, UI, etc.)
- Layouts in `src/layouts/` for page structure (Base.astro, BlogPost.astro)
- Utilities in `src/utils/` for shared functionality (date formatting, DOM helpers, etc.)
- Custom plugins in `src/plugins/` for Markdown processing extensions

### Theming and Customization
- Colors defined via CSS custom properties in global.css
- Icon system using astro-icon with custom icon directory
- Font loading for Roboto Mono via Vite plugin
- Expressive Code integration for syntax highlighting with dual theme support
