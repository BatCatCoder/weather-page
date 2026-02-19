# UX Design Specification: Weather Page

**Version:** 1.0
**Date:** 2026-02-19
**Author:** UX Designer (HANS)

## 1. Design Principles

1. **Speed first** — Użytkownik widzi pogodę w < 2s
2. **Minimalism** — Tylko niezbędne informacje
3. **Mobile-first** — Zaprojektowane dla telefonów
4. **No friction** — Zero konfiguracji, zero kont

## 2. Design Tokens

### Colors

```css
:root {
  /* Primary */
  --color-primary: #2563eb;      /* Blue for accents */
  --color-primary-light: #3b82f6;
  
  /* Backgrounds */
  --color-bg: #ffffff;
  --color-bg-secondary: #f8fafc;
  
  /* Text */
  --color-text: #1e293b;
  --color-text-secondary: #64748b;
  
  /* Weather States */
  --color-sunny: #fbbf24;        /* Yellow */
  --color-cloudy: #94a3b8;       /* Gray */
  --color-rainy: #3b82f6;        /* Blue */
  --color-stormy: #6366f1;       /* Indigo */
  
  /* Semantic */
  --color-error: #ef4444;
  --color-success: #22c55e;
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  :root {
    --color-bg: #0f172a;
    --color-bg-secondary: #1e293b;
    --color-text: #f1f5f9;
    --color-text-secondary: #94a3b8;
  }
}
```

### Typography

```css
:root {
  --font-family: system-ui, -apple-system, sans-serif;
  
  /* Sizes */
  --text-xs: 0.75rem;    /* 12px */
  --text-sm: 0.875rem;   /* 14px */
  --text-base: 1rem;     /* 16px */
  --text-lg: 1.125rem;   /* 18px */
  --text-xl: 1.25rem;    /* 20px */
  --text-2xl: 1.5rem;    /* 24px */
  --text-4xl: 2.25rem;   /* 36px - temperature */
  --text-6xl: 3.5rem;    /* 56px - main temp */
  
  /* Weights */
  --font-normal: 400;
  --font-medium: 500;
  --font-bold: 700;
}
```

### Spacing

```css
:root {
  --space-1: 0.25rem;   /* 4px */
  --space-2: 0.5rem;    /* 8px */
  --space-3: 0.75rem;   /* 12px */
  --space-4: 1rem;      /* 16px */
  --space-6: 1.5rem;    /* 24px */
  --space-8: 2rem;      /* 32px */
}
```

### Border Radius

```css
:root {
  --radius-sm: 0.25rem;
  --radius-md: 0.5rem;
  --radius-lg: 1rem;
  --radius-full: 9999px;
}
```

## 3. Component Library

### Weather Icon Component

```
┌──────────────┐
│     ☀️       │  48x48px
│    48px      │
└──────────────┘

States: sunny, cloudy, rainy, stormy, snowy, foggy
```

### Temperature Display

```
┌─────────────────┐
│      12°        │  --text-6xl, bold
│     /24°        │  --text-sm, secondary color
└─────────────────┘
     Main   Low/High
```

### Hourly Forecast Card

```
┌─────────┐
│  14:00  │  --text-sm
│   ☁️    │  24x24 icon
│   8°    │  --text-base
└─────────┘
   60x80px
```

### Location Badge

```
┌─────────────────────┐
│  📍 Warszawa        │  --text-sm
└─────────────────────┘
```

## 4. Page Layouts

### Mobile (< 768px)

```
┌────────────────────────┐
│      ☰  Weather Page   │  Header (56px)
├────────────────────────┤
│                        │
│       📍 Warszawa      │  Location badge
│                        │
│          ☀️            │  Weather icon (80x80)
│                        │
│         12°            │  Main temp (--text-6xl)
│        Słonecznie      │  Condition
│                        │
│    💨 15 km/h  💧 45%   │  Wind + Humidity
│                        │
├────────────────────────┤
│  Dziś                  │  Section header
│  ─────────────────────  │
│  ☀️  ☁️  ☁️  🌧️  🌧️   │  Hourly scroll
│  14  15  16  17  18    │
│                        │
└────────────────────────┘
```

### Desktop (≥ 768px)

```
┌──────────────────────────────────────────────┐
│         Weather Page                    ☰    │  Header (64px)
├──────────────────────────────────────────────┤
│                                              │
│              📍 Warszawa                     │
│                                              │
│                  ☀️                          │  Weather icon (120x120)
│                                              │
│                 12°                          │  Main temp
│              Słonecznie                      │
│                                              │
│          💨 15 km/h  💧 45%                   │
│                                              │
├──────────────────────────────────────────────┤
│  Dziś                                        │
│  ──────────────────────────────────────────  │
│  14:00  15:00  16:00  17:00  18:00  19:00   │  Hourly cards
│   ☀️     ☁️    ☁️    🌧️    🌧️    🌧️        │  (horizontal)
│   12°    13°   14°    12°   10°    9°       │
│                                              │
└──────────────────────────────────────────────┘
         Max-width: 768px, centered
```

## 5. Interaction Patterns

### Loading State

```
┌────────────────────────┐
│                        │
│    ████████████        │  Skeleton loader
│    ████████████        │
│    ████████████        │
│                        │
└────────────────────────┘
```

### Error State

```
┌────────────────────────┐
│                        │
│         ⚠️            │
│   Nie można pobrać     │
│      pogody            │
│                        │
│   [ Spróbuj ponownie ] │
│                        │
└────────────────────────┘
```

### Geolocation Permission

```
┌────────────────────────┐
│                        │
│   📍 Strona chce       │
│   poznać Twoją         │
│   lokalizację          │
│                        │
│  [Zezwól] [Odrzuć]     │
│                        │
└────────────────────────┘
```

## 6. Accessibility

### Focus States
- Visible focus ring (2px solid --color-primary)
- Tab order: Location → Main temp → Hourly forecast

### Color Contrast
- Text on background: 7:1 (AAA)
- Secondary text: 4.5:1 (AA)
- Icons: Accompanied by text labels

### Screen Reader
- Semantic HTML (`<main>`, `<section>`, `<h1>`)
- ARIA labels for icons
- Live region for weather updates

## 7. Responsive Breakpoints

```css
/* Mobile first */
@media (min-width: 768px) {
  /* Tablet/Desktop */
}

@media (min-width: 1024px) {
  /* Large desktop */
}
```

## 8. Animation Guidelines

- **Purpose:** Enhance understanding, not decoration
- **Duration:** 150-300ms for UI transitions
- **Easing:** `ease-out` for entering, `ease-in` for exiting

```css
/* Example: Temperature fade in */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.weather-main {
  animation: fadeIn 300ms ease-out;
}
```
