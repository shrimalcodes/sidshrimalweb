# CLAUDE.md

## Project Overview

Static personal portfolio and academic website for **Siddharth Shrimal**, an Economics and Finance undergraduate at Ashoka University. The site showcases research projects, a CV, blog posts, and non-research (finance/investing) projects.

**Live site structure:** Pure static HTML — no build step, no bundler, no package manager.

## Tech Stack

- **HTML5** with inline `<style>` blocks per page
- **Bootstrap 5.3.3** via CDN (CSS + JS with Popper)
- **Vanilla JavaScript** (ES6+ async/await, Fetch API, DOM manipulation)
- **Georgia serif** as the primary font across all pages
- No external CSS file in use (`style.css` is referenced but does not exist)

## Directory Structure

```
/
├── index.html              # Home/landing page (profile intro, navbar)
├── cv.html                 # CV display (embeds PDF via iframe)
├── blogs.html              # Blog listing with dynamic JS loading
├── projectspage.html       # Research projects (PDF embeds, GitHub cards)
├── nonresearchprojpage.html# Finance/sector analysis projects
├── profilephoto.jpeg       # Profile image
├── blogs/
│   ├── blog1.txt           # Raw text content for blog posts
│   └── blog2.txt           # Placeholder
├── blog_subpages/
│   ├── blog1.html          # Full blog post page (loads from ../blogs/blog1.txt)
│   └── blog2.html          # Placeholder
├── pdfs/                   # PDFs and Excel files (CVs, research papers, analyses)
└── CLAUDE.md               # This file
```

## Site Navigation Map

```
Home (index.html)
├── CV (cv.html)
├── Blog (blogs.html)
│   └── Individual posts (blog_subpages/blog*.html)
└── Projects (dropdown)
    ├── Research Projects (projectspage.html)
    │   ├── #project1 – Proxy Selection Frameworks
    │   ├── #project2 – India's Credit Post IBC
    │   ├── #project3 – Risk Aversion of Banks
    │   ├── #project4 – Taiwan's Energy Crisis
    │   └── #project5 – System 1&2 Thinking Model
    └── Non-Research Projects (nonresearchprojpage.html)
        ├── Climate Tech Sector Analysis
        ├── Regeneron Pharma Valuation
        └── Asian Paints Analysis
```

## Key Conventions

### HTML Structure
- Each page is a standalone HTML file with its own inline `<style>` block
- Pages that have site-wide navigation use a **Bootstrap fixed-top dark navbar** (`navbar-expand-sm bg-dark navbar-dark fixed-top`) with centered nav items
- Body needs `padding-top: 70-80px` on pages with the fixed navbar to prevent content overlap
- Simpler pages (blogs.html, blog subpages) use a plain "Back to Home" link instead of the full navbar

### CSS Patterns
- All styles are inline within each HTML file's `<style>` tag — there is no shared external stylesheet
- Consistent base styles across pages: `font-family: "Georgia", serif`, `line-height: 1.6`, `max-width: 700-1000px`, centered with `margin-left/right: auto`
- Links: `color: blue`, no underline, underline on hover
- Footer: `margin-top: 50px`, `font-size: 0.9em`, `color: #555`, centered
- Responsive: media queries at `max-width: 600px` (index) and `max-width: 480px` (projects)

### Reusable Component Patterns
- **`.project`** — White card with padding, rounded corners, box-shadow (for project entries)
- **`.toolbar`** — Gray action bar with flex layout for links (back/download)
- **`.github-card`** — Styled card linking to a GitHub repository with icon, description, and button
- **`.blog`** — Blog preview card with bottom border separator

### Blog System
Blog content is managed through a simple text-file-based system:
1. Write blog content in `blogs/blogN.txt` (plain text)
2. Create a corresponding `blog_subpages/blogN.html` wrapper page
3. Register the blog in the `blogs` JavaScript array in `blogs.html`:
   ```js
   { title: "Title", file: "blogs/blogN.txt", page: "blog_subpages/blogN.html", date: "Month DD, YYYY" }
   ```
4. The blog listing page auto-loads previews (first 75 words) via Fetch API
5. Blog subpages load full content from the text file and render paragraphs by splitting on newlines

### Adding Research Projects
1. Place the PDF in `pdfs/`
2. Add a new `<div class="project" id="projectN">` section in `projectspage.html`
3. Embed the PDF with `<iframe src="pdfs/filename.pdf"></iframe>`
4. Add a `.toolbar` div with back and download links
5. Update the navbar dropdown to include the new project anchor

### Adding Non-Research Projects
Same pattern as research projects but in `nonresearchprojpage.html`.

## Local Development

No build step is required. Serve the directory with any static HTTP server:

```bash
# Python
python3 -m http.server 8000

# Node.js (npx)
npx serve .
```

A local server is needed because the blog system uses `fetch()` to load `.txt` files, which won't work with `file://` protocol due to CORS restrictions.

## Deployment

Copy all files to any static hosting provider (GitHub Pages, Netlify, Vercel, etc.) as-is. No compilation or build step needed.

## Known Issues

- `style.css` is referenced in `index.html` and `projectspage.html` but does not exist (no effect currently since all styles are inline)
- `nonresearchprojpage.html` has two `<body>` tags (invalid HTML)
- Some footer text still reads "Your Name" instead of "Siddharth Shrimal" (`blogs.html`, `projectspage.html`)
- No `.gitignore` file — large PDFs are tracked in git
- Blog metadata is hardcoded in JavaScript (not in a separate data file)

## Important Reminders for AI Assistants

- **No build tools or package manager** — do not introduce npm, webpack, or similar unless explicitly asked
- **Inline styles only** — each page manages its own CSS in a `<style>` tag; do not create external CSS files unless asked
- When editing navigation, update it on **all pages** that have the navbar (index.html, projectspage.html, nonresearchprojpage.html)
- PDF paths may contain spaces — handle them correctly in HTML `src` and `href` attributes
- The blog system relies on relative paths from the root — maintain the `blogs/` and `blog_subpages/` directory structure
- Keep the Georgia serif font and the overall minimal academic aesthetic
