# PRD: Weather Page

**Version:** 1.0
**Date:** 2026-02-19
**Author:** Business Analyst (HANS)

## 1. Overview

Prosta, statyczna strona pogodowa hostowana na GitHub Pages. Używa Open-Meteo API i geolokalizacji przeglądarki.

## 2. User Journeys

### UJ-01: Szybkie sprawdzenie pogody
1. Użytkownik wchodzi na stronę
2. Strona prosi o geolokalizację (jeśli pierwsza wizyta)
3. Użytkownik akceptuje
4. Strona pokazuje aktualną pogodę w < 2s

### UJ-02: Geolokalizacja niedostępna
1. Użytkownik wchodzi na stronę
2. Geolokalizacja niedostępna/odrzucona
3. Strona pokazuje fallback (Warszawa) lub pozwala wpisać miasto
4. Użytkownik widzi pogodę dla wybranej lokalizacji

### UJ-03: Sprawdzenie prognozy godzinowej
1. Użytkownik widzi aktualną pogodę
2. Przewija/expanduje sekcję godzinową
3. Widzi prognozę na najbliższe 24h

## 3. Functional Requirements

### FR-001: Geolokalizacja
- System musi pobrać lokalizację użytkownika przez Browser Geolocation API
- Fallback: Warszawa (52.2297, 21.0122) gdy niedostępna

### FR-002: Pogoda aktualna
- System musi wyświetlać: temperaturę, warunki, prędkość wiatru, ikonę
- Dane z Open-Meteo API

### FR-003: Prognoza godzinowa
- System musi pokazywać prognozę na 24h
- Godzina + temperatura + ikona

### FR-004: Responsywność
- Musi działać na mobile (320px+) i desktop
- Mobile-first approach

### FR-005: Obsługa błędów
- Wyświetla komunikat gdy API niedostępne
- Graceful degradation przy braku geolokalizacji

## 4. Non-Functional Requirements

### NFR-001: Performance
- LCP < 1.5s
- Total size < 50KB

### NFR-002: Dostępność
- WCAG 2.1 AA
- Kontrast 4.5:1 dla tekstu

### NFR-003: Browser Support
- Chrome, Firefox, Safari, Edge (ostatnie 2 wersje)
- Mobile Safari, Chrome Android

## 5. Data Model

```
WeatherData {
  current: {
    temp: number       // °C
    conditions: string // "clear", "cloudy", "rain", etc.
    windSpeed: number  // km/h
    icon: string       // icon code
  }
  hourly: [{
    time: string       // "14:00"
    temp: number
    icon: string
  }] // 24 entries
  location: {
    name: string       // "Warszawa"
    lat: number
    lon: number
  }
}
```

## 6. API Overview

### Open-Meteo API

**Endpoint:** `https://api.open-meteo.com/v1/forecast`

**Parameters:**
- `latitude` - szerokość geograficzna
- `longitude` - długość geograficzna
- `current_weather=true` - aktualna pogoda
- `hourly=temperature_2m,weathercode` - prognoza godzinowa
- `forecast_days=1` - tylko dzisiaj

**Response:**
```json
{
  "current_weather": {
    "temperature": 5.2,
    "weathercode": 0,
    "windspeed": 12.3
  },
  "hourly": {
    "time": ["2026-02-19T14:00", ...],
    "temperature_2m": [5.2, ...],
    "weathercode": [0, ...]
  }
}
```

**Rate Limit:** 1000 requests/day (no API key needed)

### Geocoding API (opcjonalne)

**Endpoint:** `https://geocoding-api.open-meteo.com/v1/search`

Dla reverse geocoding (lat/lon → nazwa miasta)
