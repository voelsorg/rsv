# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Lektor-based static website for "Radstammtisch Völs", a civic organization advocating for safer cycling and pedestrian infrastructure in Völs, Tirol, Austria. The site is published in German and features news articles, focus topics, and event scheduling.

## Development Workflow

### Starting the Development Server

Run the Lektor development server with live editing interface:

```bash
uvx lektor server
```

Then open [http://127.0.0.1:5000/](http://127.0.0.1:5000/) in your browser. Click the pencil icon in the top right to enter edit mode and access the Lektor Admin UI.

### Building the Site

Build the static site locally:

```bash
uvx lektor build
```

The output will be generated in the default build directory.

### CSS/SCSS Development

The project uses Bootstrap 5 with custom SCSS. CSS work happens in the `scss/` directory:

```bash
cd scss
npm install         # Install dependencies
npm run build       # Compile SCSS to CSS
npm run watch       # Watch for changes and auto-compile
npm run css-lint    # Lint SCSS files
npm test            # Run linting and build
```

Compiled CSS is output to `assets/static/styles.css`.

## Architecture

### Lektor CMS Structure

Lektor is a static site generator that uses a file-based content system with a web-based admin interface for content editing.

**Key Directories:**

- `content/` - All page content stored as `.lr` (Lektor Record) files with YAML frontmatter and Markdown bodies. Organized hierarchically matching URL structure.
- `models/` - Data models defined in `.ini` files that define content types and their fields (e.g., `aktuell.ini` for news articles, `startpage.ini` for homepage).
- `templates/` - Jinja2 HTML templates for rendering different content types.
- `flowblocks/` - Reusable content block definitions (e.g., `event.ini`, `fact.ini`, `download.ini`) for structured content insertion.
- `assets/` - Static files (CSS, JS, images, fonts) served directly.

**Content Models:**

- `startpage` - Homepage with schedule events using flowblocks
- `aktuelles` - News listing page
- `aktuell` - Individual news article (with pub_date, teaser, image, body)
- `schwerpunkte` - Focus topics listing page
- `schwerpunkt` - Individual focus topic page
- `page` - Generic page with title and body

**Flowblocks:**

Flowblocks allow editors to add structured content blocks through the Lektor admin UI:
- `event` - Event with title, date/time, location, description
- `fact` - Key fact with label and value
- `download` - Downloadable file attachment

### Frontend Stack

- **CSS Framework**: Bootstrap 5.3.3
- **SCSS Compilation**: Dart Sass with autoprefixer via PostCSS
- **JavaScript**: Bootstrap bundle (includes Popper.js), lightgallery for image galleries
- **Fonts**: Atkinson Hyperlegible Next (accessibility-focused), Fredoka
- **Icons**: Bootstrap Icons (SVG inline)
- **Analytics**: Self-hosted Plausible instance at stats.cloud.kup.tirol

### Deployment

The site is automatically deployed via GitHub Actions when changes are pushed to the `main` branch:

1. GitHub Actions workflow (`.github/workflows/deploy.yml`) triggers on push to `main`
2. Installs `uv`, Node.js, and ImageMagick
3. Runs `uvx lektor build -O public`
4. Deploys to `gh-pages` branch using github-pages-deploy-action
5. GitHub Pages serves the site

**Configured URL**: https://preview.radstammtisch.voels.org

### Content Editing Workflow

Content editors use the Lektor Admin UI rather than editing files directly:

1. Start server with `uvx lektor server`
2. Navigate to http://127.0.0.1:5000/
3. Click pencil icon to enter edit mode
4. Navigate page tree, edit content, add flowblocks
5. Click "Save" (changes are not auto-saved)
6. Commit and push changes to GitHub
7. Site is automatically rebuilt and published

## Important Notes

- **Language**: All content and UI text is in German
- **Content Format**: Uses Lektor's `.lr` format (YAML frontmatter + Markdown body)
- **Images**: Attachments are stored alongside content in `content/` subdirectories
- **RSS Feed**: Generated via lektor-atom plugin at `/aktuelles/feed.xml`
- **Navigation**: Defined in databags (not explicitly seen but referenced in templates as `bag('nav')`)

## Common Tasks

### Adding a New News Article

1. Use Lektor Admin UI to create new child under "Aktuelles"
2. Select "aktuell" model
3. Fill in title, teaser, pub_date, optional image with alt text
4. Write body in Markdown
5. Save and preview

### Modifying Site Styles

1. Edit SCSS files in `scss/src/`
2. Run `npm run watch` from `scss/` directory
3. Changes compile to `assets/static/styles.css`
4. Refresh browser to see changes (Lektor server must be running)

### Adding a New Content Model

1. Create new `.ini` file in `models/`
2. Define fields with types (string, text, markdown, date, datetime, select, etc.)
3. Create corresponding template in `templates/`
4. Restart Lektor server to load new model

### Modifying Templates

Edit Jinja2 templates in `templates/`. The base template is `layout.html`. Changes are reflected immediately when Lektor server is running.

## Accessibility Guidelines

This site voluntarily follows WCAG 2.1 AA accessibility standards for inclusive access.

**Legal Context (Austria):** While the Austrian Barrierefreiheitsgesetz (BaFG, effective June 28, 2025) primarily applies to commercial websites, this civic organization site implements accessibility best practices voluntarily.

When editing content or templates:

### Content Editing Guidelines

**Headings:**
- Use Markdown headings (`##`, `###`) instead of HTML tags (`<h2>`, `<h3>`)
- **Correct:** `### Wie funktioniert's?`
- **Incorrect:** `<h3>Wie funktioniert's?</h3>`
- Heading hierarchy: h1 (page title) → h2 (main sections) → h3 (subsections)
- Never skip heading levels (e.g., don't jump from h2 to h4)

**Images:**
- Always provide descriptive `image_alt` text in Lektor content models
- Alt text should describe the image content, not say "image of..."
- Decorative images can have empty alt text

**Links:**
- Use descriptive link text (not "click here" or "more")
- Links to external sites should indicate they open in new context if applicable

**Iframes:**
- Always include `title` attribute describing the iframe content
- Add `lang` attribute if content is in different language
- Example: `<iframe title="Interaktive Karte..." lang="de"></iframe>`

### Template Development Guidelines

**Semantic HTML:**
- Use `<header>`, `<main>`, `<footer>`, `<nav>` for page structure
- One `<h1>` per page (page title)
- Use `<article>` for blog posts/news items
- Use `<section>` for distinct page sections

**Focus Indicators:**
- All interactive elements have visible 3px focus outlines
- Brand colors: rsv-gelb (#e3dc1e), rsv-lila (#9962eb)
- Focus uses yellow or purple for 3:1 contrast

**Color Contrast:**
- White on purple: 8.5:1 ✓
- Black on yellow: 14:1 ✓
- Avoid yellow text on purple background (1.6:1 ✗)
- Test all color combinations meet 4.5:1 for normal text, 3:1 for large text

**ARIA Labels:**
- Add `aria-label` to icon-only links
- Mark decorative elements with `aria-hidden="true"`
- Don't duplicate visible text in aria-label
