# System Patterns — Bourne Bridge Safety Site

## High-level architecture
- **Hosting**: GitHub Pages serving the root of the `Bourne-bridge-safety` repo.
- **App type**: Single static HTML page (`index.html`) with inline `<script>` blocks.
- **Styling**: Tailwind CSS via CDN (`https://cdn.tailwindcss.com`). No build step; utility classes are written directly in the markup.
- **Fonts**: Google Fonts (Inter) via `<link>` in `<head>`.
- **Images & documents**:
  - Stored in `/images` and `/documents` folders alongside `index.html`.
  - Open Graph / Twitter cards use absolute URLs to `SocialCard.jpg` hosted from GitHub Pages.
- **Embeds**:
  - YouTube iframe for the MassDOT design visualization.
  - Google Forms iframe for coalition sign-on.

## JavaScript patterns

All JS is in two inline `<script>` blocks:
1. **Config script (near top of `<body>`)**
   - Declares constants used by later behavior:
     - `EEA_NUM`, `PROJECT_FILE`, `PORTAL_PROJECT_ID`.
     - `FEEDBACK_EMAIL`, `FEEDBACK_CC`.
     - `PORTAL_URL` (derived from `PORTAL_PROJECT_ID`).
     - `GOOGLE_FORM_URL`, `GOOGLE_FORM_EMBED`.
   - Pattern: keep IDs and external links centralized so the rest of the code can refer to them symbolically.

2. **Behavior script (at bottom of `<body>`)**
   - Helper functions:
     - `trackEvent(name, params)`: thin wrapper around `gtag('event', ...)` for GA4.
     - `buildMailto(...)`: constructs a `mailto:` URL with optional CC.
     - `getCommentWithSignature()`: returns the comment text plus optional name/town.
   - DOM wiring & behaviors:
     - Sets portal link `href` from `PORTAL_URL`.
     - Handles **Share** button behavior using `navigator.share` where supported, otherwise copies URL to clipboard.
     - Handles **Append my name** and **Copy comment** actions.
     - Handles optional **email officials** action (guarded if the element is absent).
     - Handles **Google Form** interactions (open form in new tab, copy form link).

### Event tracking pattern (Google Analytics 4)

- GA4 base tag is loaded in `<head>`:
  ```html
  <script async src="https://www.googletagmanager.com/gtag/js?id=G-026HJCWZTJ"></script>
  <script>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', 'G-026HJCWZTJ');
  </script>
  ```

- Custom events are sent via `trackEvent` from user interaction handlers:
  - `share_click` when the hero **Share** button is clicked.
  - `comment_name_appended` when the user appends their name/town to the comment text.
  - `comment_copied` when the user copies the suggested comment.
  - `portal_opened` when the MassDOT portal link is clicked.
  - `signature_form_opened` when the Google Form is opened in a new tab.
  - `signature_form_link_copied` when the Google Form link is copied.

Each event includes simple context parameters such as `location` or `destination` to make GA4 reports more readable.

## Defensive coding patterns
- DOM event bindings use `getElementById` once and **check for element existence** before attaching listeners where appropriate (e.g., `emailOfficials`).
- Clipboard operations (`navigator.clipboard.writeText`) are wrapped in `try/catch` for the share button, with console logging on failure and no user-facing disruption beyond missing the convenience feature.
- Web Share API usage is feature-detected (`if (navigator.share)`) and falls back gracefully to clipboard copying.

## Content/layout patterns
- All content is organized into semantically named sections:
  - Hero, problem/benefits, cost-effectiveness, how-to-act, signature form, support quotes.
- Tailwind utility classes drive layout and colors; there is a single small `<style>` block setting the base font family.
- CTAs are visually emphasized via background colors and bold typography.

## Future pattern considerations
- If the site grows, consider:
  - Extracting JS into a separate file (e.g. `main.js`) for clarity.
  - Introducing a very light build step to manage Tailwind CSS more efficiently (e.g. purging unused classes), while keeping the site static.
  - Adding a simple event taxonomy/README so future contributors know how to name GA4 events and which parameters to include.
