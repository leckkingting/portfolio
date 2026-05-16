# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A static resume website with no build step, no package manager, and no framework. Open `index.html` directly in a browser to preview.

## File structure

```
index.html        ← main resume page (hero, experience, skills, education, contact)
projects.html     ← separate projects page; same nav/footer/theme structure
css/style.css     ← all styles for both pages
js/main.js        ← all JavaScript for both pages
```

## Architecture

**Theming** — All colors are CSS custom properties defined in `:root` inside `css/style.css`. A `html[data-theme="light"]` block overrides those tokens for light mode. `js/main.js` toggles the `data-theme` attribute on `<html>` and persists the choice in `localStorage`.

**Animations** — Elements that animate on scroll (`.timeline-item`, `.skill-group`, `.edu-card`, `.project-card`) start with `opacity: 0; transform: translateY(...)` in CSS and gain a `.visible` class via `IntersectionObserver` in `js/main.js`. Do not add `opacity` or `transform` to these selectors without accounting for the animation state.

**Responsive nav** — Desktop shows `.nav-links` + `.btn-contact`. Below 640 px those are hidden and a `.hamburger` button reveals a `.mobile-menu` drawer. Both are wired in `js/main.js`.

**Cross-page navigation** — `projects.html` links back to resume sections using `index.html#section` hrefs. The active nav link on `projects.html` is highlighted inline via `style="color: var(--text);"` since the IntersectionObserver nav highlighter only runs on `section[id]` elements.

## Conventions

- All resume content (name, experience, skills, education, contact) lives only in `index.html`. All project content lives only in `projects.html`.
- CSS variables in `:root` are the single source of truth for colors — never hardcode color values elsewhere.
- The nav background uses `color-mix(in srgb, var(--bg) 85%, transparent)` instead of a hardcoded `rgba` so it adapts to both themes.
- JavaScript is vanilla ES6 — no libraries or bundlers.
- Projects on `projects.html` are AirAsia work projects only; GBG Malaysia and Hitachi eBworx are excluded as no end-to-end projects were built there.
