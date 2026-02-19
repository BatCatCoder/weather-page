# Architecture: Weather Page

**Version:** 1.0
**Date:** 2026-02-19
**Author:** Architect (HANS)

## 1. Tech Stack

| Layer | Technology | Rationale |
|-------|------------|-----------|
| Frontend | Vanilla HTML/CSS/JS | Zero build, minimal size, simple |
| Hosting | GitHub Pages | Free, static, CDN |
| API | Open-Meteo | Free, no key, reliable |
| Build | None | Not needed for single HTML |

## 2. Architecture Overview

```
┌─────────────────────────────────────────┐
│           GitHub Pages (CDN)            │
│  ┌─────────────────────────────────┐    │
│  │     index.html (single file)    │    │
│  │  - HTML structure               │    │
│  │  - CSS (<style>)                │    │
│  │  - JS (<script>)                │    │
│  └─────────────────────────────────┘    │
└─────────────────┬───────────────────────┘
                  │ HTTPS
                  ▼
┌─────────────────────────────────────────┐
│           Browser Runtime               │
│  ┌─────────────────────────────────┐    │
│  │  Geolocation API                │    │
│  │  ↓                              │    │
│  │  fetch(Open-Meteo API)          │    │
│  │  ↓                              │    │
│  │  Render weather                 │    │
│  └─────────────────────────────────┘    │
└─────────────────────────────────────────┘
```

## 3. Project Structure

```
weather-page/
├── index.html          # Single file with embedded CSS/JS
├── _bmad-output/       # Planning artifacts
│   └── planning-artifacts/
│       ├── product-brief.md
│       ├── prd.md
│       ├── architecture.md
│       ├── ux-design-specification.md
│       └── epics.md
├── README.md           # For GitHub
└── .nojekyll          # Disable Jekyll for GH Pages
```

## 4. Key Decisions

### ADR-001: Single HTML File
**Decision:** Cała aplikacja w jednym pliku HTML z inline CSS/JS

**Rationale:**
- Minimalna ilość requestów HTTP (1)
- Najprostszy deployment (push to GitHub)
- Rozmiar < 50KB łatwy do osiągnięcia

**Alternatives Considered:**
- Multi-file: Więcej requestów, ale cache'owanie
- Build process (Vite/Webpack): Overkill dla tego projektu

### ADR-002: Vanilla JS (No Framework)
**Decision:** Czysty JavaScript bez frameworków

**Rationale:**
- Zero bundle overhead
- Prostota dla małego projektu
- Szybkie ładowanie

**Alternatives Considered:**
- React/Vue/Svelte: Za dużo overhead dla 100 linii kodu
- Alpine.js/HTMX: Może być overkill, ale HTMX ciekawy

### ADR-003: CSS Custom Properties
**Decision:** Użycie CSS variables dla theming

**Rationale:**
- Native browser support
- Easy dark mode toggle
- No preprocessors needed

## 5. Data Flow

```
User opens page
       │
       ▼
Browser requests geolocation
       │
       ├─► Granted ──► Get coordinates
       │                    │
       │                    ▼
       │              Fetch Open-Meteo
       │                    │
       └─► Denied ──► Use fallback (Warszawa)
                            │
                            ▼
                      Fetch Open-Meteo
                            │
                            ▼
                      Parse response
                            │
                            ▼
                      Render DOM
                            │
                            ▼
                      Show weather
```

## 6. Error Handling

| Error | Strategy |
|-------|----------|
| Geolocation denied | Fallback to Warsaw, show notice |
| Open-Meteo timeout | Show "Service unavailable, try later" |
| Invalid coordinates | Show error message |
| Network offline | Cache last response (localStorage) |

## 7. Security Considerations

- ✅ HTTPS only (GitHub Pages default)
- ✅ No user input (no XSS risk)
- ✅ No cookies/tracking
- ✅ CSP-friendly (no external scripts)

## 8. Performance Targets

| Metric | Target | Strategy |
|--------|--------|----------|
| LCP | < 1.5s | Single file, no blocking |
| FCP | < 0.5s | Inline critical CSS |
| TTI | < 2s | Minimal JS |
| Bundle size | < 50KB | No dependencies |

## 9. Testing Strategy

- **Manual testing:** Cross-browser (Chrome, Firefox, Safari)
- **Lighthouse:** Performance audit
- **Real devices:** Mobile testing (iOS Safari, Chrome Android)
- **API mock:** Test error scenarios locally
