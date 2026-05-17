# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Czym jest ten projekt

Skill AI (`ogrod-suwalszczyzna.skill`) — baza wiedzy ogrodniczej dla konkretnej posesji na Suwalszczyźnie (54.011807, 22.362126), strefa USDA 6a. Plik `.skill` to archiwum ZIP zawierające pliki Markdown.

## Struktura

```
ogrod-suwalszczyzna.skill (ZIP)
├── SKILL.md                         — definicja skilla, routing, reguły
└── references/
    ├── garden.md                    — kanon: lokalizacja, układ grządek P1–P5, inwentarz roślin, historia upraw
    ├── plants-pl.md                 — kalendarz upraw i odmian (daty 6a)
    ├── garden-planner.md            — planowanie sezonu, rotacja 4-letnia
    ├── weekly-garden-manager.md     — tygodniowe listy zadań (🔴🟡🟢⛔)
    ├── plant-doctor.md              — diagnostyka chorób/szkodników
    ├── orchard-manager.md           — cięcie, opryski, pielęgnacja drzew/krzewów
    ├── companion-planting.md        — sąsiedztwo roślin
    └── soil-health.md               — gleba, kompost, pH, mulcz
```

## Architektura i przepływ danych

1. **Single source of truth** — `garden.md` jest jedynym miejscem przechowywania stanu posesji (grządki, historia). Moduły referencyjne nie duplikują tych danych.
2. **Routing po intencji** — SKILL.md definiuje, który moduł załadować na podstawie słów kluczowych zapytania (np. "plamy" → plant-doctor, "ciąć" → orchard-manager).
3. **Kontekst zawsze ładowany** — każde zapytanie wymaga załadowania `garden.md` + `plants-pl.md` przed odpowiedzią.

## Zasady nieprzekraczalne (z SKILL.md)

- **Strefa 6a zawsze** — daty przesunięte +1–2 tygodnie vs. centralna Polska. Ostatni przymrozek: 15–25 maja.
- **Progresja 🟢→🟡→🔴** — mechaniczne/naturalne → biologiczne → chemiczne (ostateczność).
- **Nigdy oprysk w kwitnienie** — zabija zapylacze.
- **Karencje obowiązkowe** — każdy środek chemiczny musi mieć podany czas karencji w dniach.
- **Polskie nazwy i produkty** — Plantico, W. Legutko, PNOS; nie katalogi US/UK.
- **Konkretność** — odpowiedzi odwołują się do grządek P1–P5, nie ogólniki.
- **Brak zgadywania** — jeśli brakuje informacji, zadaj 1 pytanie zamiast zakładać.

## Praca z plikiem .skill

Aby edytować zawartość:
```powershell
# Rozpakuj
Expand-Archive -Path ogrod-suwalszczyzna.skill -DestinationPath ogrod-skill-extracted -Force

# Po edycji — spakuj z powrotem
Compress-Archive -Path ogrod-skill-extracted\* -DestinationPath ogrod-suwalszczyzna.skill -Force
```

## Znane luki w danych (garden.md)

- Odmiany jabłoni nie udokumentowane
- "Szary" — zakładany grab, wymaga potwierdzenia
- Pomidory — zakładana uprawa gruntowa (brak tunelu/szklarni?)
- Borówka — zakładane stanowisko kwaśne, brak potwierdzenia
- Typ gleby — zakładany "glina średnia", brak badania
- Historia upraw 2024–2026 — pusta, osłabia doradztwo rotacyjne
