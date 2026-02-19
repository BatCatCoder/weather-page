# Epics & Stories: Weather Page

**Version:** 1.0
**Date:** 2026-02-19
**Author:** Scrum Master (HANS)

## Overview

| Epic | Stories | Priority | Dependencies |
|------|---------|----------|--------------|
| Epic 1: Foundation | 3 | P0 | None |
| Epic 2: Core Weather | 3 | P0 | Epic 1 |
| Epic 3: Enhancements | 2 | P1 | Epic 2 |

---

## Epic 1: Foundation

**Goal:** Postawić podstawową stronę HTML na GitHub Pages
**Value:** Działająca infrastruktura dla aplikacji
**Dependencies:** None

### Story 1-1: Setup GitHub Pages

**As a** developer
**I want** a GitHub repository with Pages enabled
**So that** the app is accessible via public URL

**Acceptance Criteria:**

```gherkin
Given a GitHub repository
When I push to main branch
Then the site is deployed to github.io
And the URL returns HTTP 200
```

**Technical Notes:**
- Create repo: `weather-page`
- Enable Pages in Settings → Pages → Source: main branch
- Add `index.html` with "Hello World"
- Add `.nojekyll` file

**Dependencies:** None

**Estimated Effort:** S (30 min)

---

### Story 1-2: Create Base HTML Structure

**As a** developer
**I want** a semantic HTML structure
**So that** the page is accessible and SEO-friendly

**Acceptance Criteria:**

```gherkin
Given the index.html file
When I load the page
Then I see a header, main section, and footer
And HTML validates without errors
```

**Technical Notes:**
- Use semantic HTML5 (`<header>`, `<main>`, `<section>`)
- Add meta tags for viewport, charset
- Include placeholder content

**Dependencies:** 1-1

**Estimated Effort:** S (30 min)

---

### Story 1-3: Implement Basic CSS

**As a** user
**I want** a visually appealing page
**So that** I can easily read the weather information

**Acceptance Criteria:**

```gherkin
Given the page loads
When I view it on mobile or desktop
Then the layout is responsive
And colors match the design tokens
```

**Technical Notes:**
- Embed CSS in `<style>` tag
- Use CSS custom properties from UX spec
- Mobile-first approach

**Dependencies:** 1-2

**Estimated Effort:** S (30 min)

---

## Epic 2: Core Weather Features

**Goal:** Wyświetlić aktualną pogodę dla użytkownika
**Value:** Główna funkcjonalność produktu
**Dependencies:** Epic 1

### Story 2-1: Implement Geolocation

**As a** user
**I want** the page to automatically detect my location
**So that** I see weather for my current position

**Acceptance Criteria:**

```gherkin
Given I visit the page
When I grant geolocation permission
Then my coordinates are captured
And displayed as a location name
```

```gherkin
Given I deny geolocation permission
When the page loads
Then Warsaw is used as fallback
And a notice shows "Using default location"
```

**Technical Notes:**
- Use `navigator.geolocation.getCurrentPosition()`
- Handle permission denied, unavailable, timeout errors
- Fallback coords: 52.2297, 21.0122 (Warsaw)

**Dependencies:** 1-3

**Estimated Effort:** M (1 hour)

---

### Story 2-2: Fetch Weather Data

**As a** user
**I want** current weather for my location
**So that** I know what to wear

**Acceptance Criteria:**

```gherkin
Given my location is known
When the page fetches weather data
Then I see current temperature
And weather conditions (sunny, cloudy, etc.)
And wind speed
```

**Technical Notes:**
- Endpoint: `https://api.open-meteo.com/v1/forecast`
- Parameters: `latitude`, `longitude`, `current_weather=true`
- Parse `weathercode` to condition string
- Handle API errors with user-friendly message

**Dependencies:** 2-1

**Estimated Effort:** M (1 hour)

---

### Story 2-3: Display Weather UI

**As a** user
**I want** a clear visual display of weather
**So that** I can understand it at a glance

**Acceptance Criteria:**

```gherkin
Given weather data is fetched
When the page renders
Then I see a large temperature display
And a weather icon
And location name
And wind speed
```

**Technical Notes:**
- Main temp: `--text-6xl`
- Icon: emoji or inline SVG
- Animate entrance with CSS keyframes

**Dependencies:** 2-2

**Estimated Effort:** M (1 hour)

---

## Epic 3: Enhancements

**Goal:** Dodać prognozę godzinową i obsłużyć błędy
**Value:** Lepsze UX i stabilność
**Dependencies:** Epic 2

### Story 3-1: Hourly Forecast

**As a** user
**I want** to see weather for the next 24 hours
**So that** I can plan my day

**Acceptance Criteria:**

```gherkin
Given weather data includes hourly forecast
When I view the page
Then I see a scrollable list of 24 hourly cards
Each showing time, icon, and temperature
```

**Technical Notes:**
- Fetch hourly data: `hourly=temperature_2m,weathercode`
- Horizontal scroll on mobile, grid on desktop
- Show only next 24h from current time

**Dependencies:** 2-3

**Estimated Effort:** M (1-2 hours)

---

### Story 3-2: Error Handling & Loading States

**As a** user
**I want** clear feedback when things go wrong
**So that** I'm not confused by empty screens

**Acceptance Criteria:**

```gherkin
Given the API is unavailable
When I load the page
Then I see "Service unavailable, please try later"
And a retry button
```

```gherkin
Given weather is loading
When I load the page
Then I see a skeleton loader
And it disappears when data arrives
```

**Technical Notes:**
- Skeleton loader with CSS animation
- Try-catch around fetch
- "Retry" button to refetch

**Dependencies:** 2-2

**Estimated Effort:** S (30 min)

---

## Story Dependency Graph

```
1-1 ──▶ 1-2 ──▶ 1-3
                 │
                 └──▶ 2-1 ──▶ 2-2 ──▶ 2-3
                              │        │
                              │        └──▶ 3-1
                              │
                              └──▶ 3-2
```

## Requirement Traceability

| Story | Requirements |
|-------|--------------|
| 1-1 | Infrastructure |
| 1-2 | FR-004, NFR-003 |
| 1-3 | FR-004, NFR-001 |
| 2-1 | FR-001, FR-005 |
| 2-2 | FR-002, NFR-003 |
| 2-3 | FR-002, FR-004 |
| 3-1 | FR-003 |
| 3-2 | FR-005, NFR-002 |

## Sprint Estimation

- **Epic 1:** ~1.5 hours (3 small stories)
- **Epic 2:** ~3 hours (3 medium stories)
- **Epic 3:** ~2 hours (1 medium + 1 small)
- **Total:** ~6.5 hours

**Recommended Start:** Epic 1, Story 1-1
