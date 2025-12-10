# Project Brief — Bourne Bridge Safety Site

This project hosts a static advocacy site for the Bourne Bridge multimodal path, deployed via GitHub Pages from the `Bourne-bridge-safety` repository. The site helps residents and riders quickly understand the safety problem at the south end of the Bourne Bridge path and take specific actions during the public comment period.

## Core Goals
- Explain why the current at-grade Sandwich Rd crossing design is unsafe and short-sighted.
- Advocate for a **grade-separated multimodal overpass** as a safer, long-term alternative.
- Make it easy for visitors to **take action quickly**:
  - Attend the MassDOT public meeting.
  - Submit a public comment via the official MassDOT portal.
  - Add their name to a coalition-style comment via Google Forms.
- Provide sharable content (copyable comment text, link sharing, etc.) that lowers friction for participation.

## Audience
- Local riders and residents.
- Participants and organizers of large events (e.g. Pan-Mass Challenge, Best Buddies).
- Advocates who can help amplify the issue (email, social, word-of-mouth).

## Current State (as of 2025-12-10)
- Single-page static HTML (`index.html`) styled with Tailwind via CDN.
- Deployed via GitHub Pages at `https://batsonjay.github.io/Bourne-bridge-safety/`.
- Content includes:
  - Hero section with key CTAs (attend meeting, submit comment, add name, share).
  - Visuals of the proposed at-grade crossing and an overpass concept.
  - Embedded MassDOT design video (YouTube).
  - Detailed problem/benefits rationale and supporting quote.
  - Pre-drafted comment text and Google Form embed.
- Google Analytics 4 is integrated for basic pageview tracking plus custom click events on key CTAs.

## Constraints
- Simple static hosting (GitHub Pages, no backend).
- Minimal build tooling; currently just a hand-edited `index.html` with inline `<script>` blocks.
- Avoid heavy JS frameworks; keep load fast and accessible for a broad audience.

## Success Criteria
- Visitors understand the safety issue within seconds of landing.
- A meaningful fraction of visitors complete at least one key action:
  - Opening the MassDOT portal.
  - Copying and submitting the suggested comment.
  - Opening or submitting the coalition Google Form.
- Analytics (GA4) provide insight into which CTAs are most used and where visitors drop off.
