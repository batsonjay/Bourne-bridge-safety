# Tech Context — Bourne Bridge Safety Site

## Stack overview
- **Hosting**: GitHub Pages (static site hosting from the `Bourne-bridge-safety` repo).
- **Runtime**: Plain HTML + CSS + vanilla JavaScript.
- **Styling**: Tailwind CSS via CDN (`https://cdn.tailwindcss.com`).
- **Fonts**: Google Fonts (Inter).
- **Analytics**: Google Analytics 4 (GA4) via gtag.js.
- **Embeds / external services**:
  - YouTube for MassDOT design visualization.
  - Google Forms for coalition sign-on.

## Files & structure
- `index.html`
  - Single-page application entry and only HTML file.
  - Contains:
    - `<head>` with meta tags, OG/Twitter tags, font and Tailwind includes, and GA4 tag.
    - `<body>` with all content sections and inline `<script>` blocks.
- `images/`
  - PNG/JPG assets used for hero imagery, diagrams, and social cards.
- `documents/`
  - PDFs (e.g., draft MassDOT comment, DEIS excerpts) linked from the page.
- `memory-bank/`
  - Project-specific documentation for Cline (context, patterns, progress, etc.).

## Google Analytics 4 setup
- **Measurement ID**: `G-026HJCWZTJ` (as of 2025-12-10).
- **Implementation**:
  - Standard GA4 snippet in `<head>`:
    ```html
    <script async src="https://www.googletagmanager.com/gtag/js?id=G-026HJCWZTJ"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());
      gtag('config', 'G-026HJCWZTJ');
    </script>
    ```
  - Custom event helper in bottom script:
    ```js
    function trackEvent(name, params = {}) {
      if (typeof gtag === 'function') {
        gtag('event', name, params);
      }
    }
    ```
  - Events emitted:
    - `share_click` { location: 'hero' }
    - `comment_name_appended` { location: 'comment_section' }
    - `comment_copied` { destination: 'massdot_portal' }
    - `portal_opened` { destination: 'massdot_portal' }
    - `signature_form_opened` { location: 'coalition_section' }
    - `signature_form_link_copied` { location: 'coalition_section' }

These events appear in GA4 under **Engagement → Events** and can optionally be marked as conversions.

## Development workflow
- Edits are made directly to `index.html` (and assets) and committed to the main branch.
- Pushing to GitHub triggers GitHub Pages to redeploy.
- No local build tooling (Webpack, Vite, etc.) is currently used.

## Constraints & considerations
- Must remain fully static to be compatible with GitHub Pages without extra infra.
- Use of third-party scripts (GA, YouTube, Google Forms) should be limited and purposeful to keep performance acceptable.
- Tailwind is loaded via CDN; there is no purge step, so class usage should be reasonably conservative.

## Potential future tech enhancements
- Introduce a minimal build (e.g. simple npm + Tailwind CLI) to:
  - Generate a purged CSS file.
  - Split JS into separate files for clarity.
- Add basic automated checks:
  - HTML validation and accessibility linting.
  - Link checking (to ensure external portals/forms/docs haven’t changed URLs).
