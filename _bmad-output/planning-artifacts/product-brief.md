# Product Brief: Weather Page

**Date:** 2026-02-19
**Author:** Product Owner Agent

## Vision

Prosta, błyskawiczna strona z pogodą, która działa wszędzie, nie wymaga rejestracji i ładuje się w ułamku sekundy. Zero śmieci, zero reklam, tylko pogoda.

## Problem Statement

Ludzie chcą sprawdzić pogodę szybko. Obecne opcje są albo wolne (ciężkie strony z reklamami), albo wymagają instalacji aplikacji, albo zmuszają do tworzenia kont. Wystarczy jedna liczba - temperatura na zewnątrz - a trzeba przechodzić przez reklamy, cookies bannery i ciężkie UI.

**Dlaczego to ważne?** Bo sprawdzanie pogody nie powinno wymagać 5 kliknięć i czekania na załadowanie 2MB skryptów śledzących.

## Target Users

### Persona 1: Szybki sprawdź

- **Role:** Codzienny użytkownik internetu
- **Pain Points:** 
  - Aplikacje pogodowe zajmują miejsce na telefonie
  - Weather.com ładuje się wieki
  - Wszędzie reklamy i komunikaty o cookies
- **Goals:** 
  - Sprawdzić temperaturę w < 2 sekundy
  - Zobaczyć czy brać parasol
  - Zapamiętać stronę w zakładkach

### Persona 2: Minimalista techniczny

- **Role:** Programista/entuzjasta technologii
- **Pain Points:**
  - Aplikacje zbierają dane
  - Chce czegoś lekkiego i otwartego
  - Niepotrzebne funkcje (radary, mapy, długoterminowe prognozy)
- **Goals:**
  - Proste rozwiązanie bez nadmiaru
  - Może hostować samodzielnie
  - Zrozumiały kod

## Value Proposition

For **osób sprawdzających pogodę na co dzień** who **chcą szybkiego dostępu bez zbędnych przeszkód**, Weather Page is a **statyczna strona HTML** that **pokazuje aktualną pogodę w < 2 sekundy**. Unlike **aplikacje mobilne i ciężkie serwisy pogodowe**, our product **nie wymaga instalacji, konta ani ładowania reklam**.

## MVP Features

### Must-Have

1. **Aktualna pogoda dla lokalizacji użytkownika** — Podstawa produktu. Temperatura, warunki (słonecznie/pochmurno/deszcz), wiatr. Bez tego produkt nie ma sensu.
2. **Geolokalizacja automatyczna** — Użytkownik wchodzi na stronę i od razu widzi pogodę dla swojej lokalizacji. Zero konfiguracji.
3. **Responsywny design** — Musi działać na telefonie i desktopie. Większość sprawdzania pogody to mobile.

### Should-Have

1. **Ikony pogody** — Wizualna reprezentacja warunków (słońce, chmura, deszcz). Znacząco poprawia UX bez dużego kosztu.
2. **Prognoza godzinowa (24h)** — "Czy za 2h będzie padać?" - częste pytanie. Dodaje wartość bez komplikacji.
3. **Obsługa błędów lokalizacji** — Jeśli geolokalizacja nie działa, fallback do domyślnego miasta lub pozwól wpisać ręcznie.

### Nice-to-Have (Post-MVP)

1. **Prognoza 7-dniowa** — Przydatne ale nie krytyczne dla MVP.
2. **Przełącznik C°/F°** — Dla użytkowników z USA. Mała wartość dla głównego rynku (Europa).
3. **PWA offline** — Cache'owanie ostatniej pogody. Nice to have ale wymaga więcej kodu.
4. **Wielu języków** — Dla szerszego rynku. Angielski wystarczy na start.

## Success Metrics

| Metric | Target | Rationale |
|--------|--------|-----------|
| Czas ładowania (LCP) | < 1.5s | To główna wartość produktu - szybkość |
| Rozmiar strony | < 50KB | Minimalizm, szybkość na mobile |
| Użytkownicy wracający (7 dni) | > 30% | Jeśli produkt rozwiązuje problem, ludzie wrócą |
| Bounce rate | < 40% | Użytkownicy znajdują to czego szukali |

## Constraints

- **GitHub Pages** — Statyczny hosting, zero backendu
- **Open-Meteo API** — Darmowe, bez API key, limit 1000 requestów/dzień
- **Vanilla JS/CSS** — Bez frameworków dla prostoty i małego rozmiaru
- **Single HTML file** — Jeśli możliwe, dla maksymalnej prostoty

## Open Questions

- Jak obsłużyć użytkowników z wyłączoną geolokalizacją? (fallback city vs manual input)
- Czy pokazywać "odczuwalną" temperaturę jako dodatkową informację?
- Jakie miasto domyślne jeśli geolokalizacja zawiedzie? (Warszawa? Wrocław? Geolokalizacja po IP?)

## Next Steps

1. Business Analyst to create detailed PRD
2. Architect to design technical architecture
3. UX Designer to create design specification
