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

## Accessibility Testing Procedures

Regular accessibility testing ensures the site remains compliant and usable for all visitors.

### Quick Pre-Commit Checklist

Before committing content or template changes:

- [ ] All images have descriptive alt text
- [ ] Headings use proper hierarchy (h1 → h2 → h3)
- [ ] Links have descriptive text or aria-labels
- [ ] No HTML heading tags in content (use Markdown `##` instead)
- [ ] Interactive elements are keyboard accessible
- [ ] Color contrast meets 4.5:1 for normal text, 3:1 for large text

### Manual Testing Workflow

**1. Keyboard Navigation (5 minutes)**
```
- Tab through entire page
- Verify skip link works (Tab → Enter)
- Check all interactive elements are reachable
- Ensure focus indicators are visible
- Test with Shift+Tab for reverse navigation
```

**2. Visual Inspection (5 minutes)**
```
- Check focus indicators on links, buttons, inputs
- Verify text is readable at 200% zoom
- Test responsive breakpoints (mobile, tablet, desktop)
- Review color contrast for any new colored elements
```

**3. Screen Reader Spot Check (10 minutes - Optional)**

Using NVDA (Windows), Orca (Linux), or VoiceOver (macOS):
```
- Navigate with heading shortcuts (H key)
- Jump through landmarks (D key)
- Listen to image alt text announcements
- Verify link context is clear
```

### Automated Testing Tools

**Browser Extensions:**
- **[axe DevTools](https://www.deque.com/axe/devtools/)** - Install in Chrome/Firefox
  - Run on homepage, sample article, sample schwerpunkt
  - Fix any "Critical" or "Serious" issues

- **[WAVE](https://wave.webaim.org/extension/)** - Browser extension
  - Visual feedback overlay
  - Useful for quick checks

**Command Line:**
```bash
# Lighthouse accessibility audit (Chrome DevTools)
# Open DevTools → Lighthouse → Accessibility → Run
```

### Annual Review Checklist

**Scheduled: October each year**

1. **Update Documentation**
   - [ ] Review [ACCESSIBILITY_TESTING.md](ACCESSIBILITY_TESTING.md)
   - [ ] Update "Last Updated" date in `/barrierefreiheit` page
   - [ ] Check for WCAG updates (currently 2.1 AA, watch for 2.2 adoption)

2. **Run Automated Scans**
   - [ ] axe DevTools on 5 representative pages
   - [ ] WAVE on homepage and main navigation
   - [ ] Lighthouse accessibility audit

3. **Manual Testing**
   - [ ] Keyboard navigation on new pages
   - [ ] Screen reader test on updated sections
   - [ ] Mobile touch target verification
   - [ ] 200% zoom test

4. **Content Audit**
   - [ ] Check new images for alt text
   - [ ] Verify new content uses proper heading hierarchy
   - [ ] Review any new interactive elements (forms, modals, etc.)

5. **Report Findings**
   - [ ] Document any issues found
   - [ ] Create GitHub issues for fixes
   - [ ] Update ACCESSIBILITY_TESTING.md with results

### Common Issues to Watch For

**During Development:**
- Missing alt text on new images
- Raw HTML headings (`<h3>`) instead of Markdown (`###`)
- Links with vague text ("click here", "more")
- New color combinations without contrast check
- Interactive elements without keyboard access

**During Content Editing:**
- Images without `image_alt` field filled
- Skipping heading levels (h1 → h3)
- Embedded content (iframes) without title attribute
- Complex tables without proper markup

### Resources

**Documentation:**
- Full testing report: [ACCESSIBILITY_TESTING.md](ACCESSIBILITY_TESTING.md)
- Austrian law info: [Barrierefreiheitsgesetz (BaFG)](https://www.sozialministerium.gv.at)
- WCAG Guidelines: [WCAG 2.1 Quick Reference](https://www.w3.org/WAI/WCAG21/quickref/)

**Testing Tools:**
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- [WAVE Web Accessibility Evaluation Tool](https://wave.webaim.org/)
- [axe DevTools Browser Extension](https://www.deque.com/axe/devtools/)

**Learning:**
- [WebAIM Articles](https://webaim.org/articles/)
- [A11y Project Checklist](https://www.a11yproject.com/checklist/)
- [MDN Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)

### Contact for Accessibility Issues

If users report accessibility issues:

1. Document the issue in detail (browser, AT used, specific problem)
2. Reproduce if possible
3. Check ACCESSIBILITY_TESTING.md for known issues
4. Test fix with relevant assistive technology
5. Update documentation if it's a new category of issue

Accessibility is an ongoing commitment, not a one-time achievement.
