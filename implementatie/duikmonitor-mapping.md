# Duikmonitor mapping naar OSOD v0.1

Status: informatief, niet normatief.

Dit document beschrijft het beoogde type mapping tussen Duikmonitor en OSOD. De exacte veldnamen kunnen per Duikmonitor-versie verschillen.

| OSOD-onderdeel | Duikmonitor-bron | Opmerking |
|---|---|---|
| `context` | project-/inzetgegevens | Alleen exporteren wat nodig is voor OSOD. |
| `roles.duikploegleider` | ploegleider-/projectgegevens | Persoonsgegevens minimaliseren. |
| `roles.duikers[]` | duikerlijst/live-planning | Gebruik lokale identifiers waar mogelijk. |
| `dive.datum`, `tijdIn`, `tijdUit` | planning/live-registratie | ISO-formaat bij export. |
| `dive.maximaleDiepteWerkelijkM` | gemeten of ingevoerde maximale diepte | Niet afvlakken naar 15 m. |
| `dive.tabelDiepteM` | DCIEM-tabelkeuze | Alleen 6/9/12/15 of `null`. |
| `dive.gevolgdSchema` | DCIEM/meterregelkeuze | Bron en versie opnemen. |
| `dive.decompressieverloop` | berekening/live-verloop | Ook no-deco expliciet maken. |
| `calculation` | DCIEM-kern/zelftoetsen | Vereist voor Niveau R. |
| `signoff` | export-/aftekenstatus | Lokale digitale of papieren status mappen. |
| `extensions` | app-specifieke gegevens | Alleen onder namespace. |

## Belangrijk

OSOD-export mag geen volledige interne app-state dump zijn. Het moet een stabiel standaardrecord zijn.


## Externe systemen

Deze mapping stopt bij het OSOD-record. Een eventuele koppeling naar een extern systeem hoort daarna als aparte mapping te worden beschreven:

```text
Duikmonitor → OSOD-record → extern systeem
```

Het externe systeem is daarbij niet normatief voor OSOD.
