# Publicatie OSOD v0.1

Deze versie is gemaakt als publiceerbare conceptversie van OSOD v0.1. De versie blijft **0.1**; dit is geen nieuwe minorversie.

Belangrijkste publicatiekeuzes:

1. OSOD blijft leidend; Duikmonitor is beoogde eerste referentie-implementatie en niet normatief.
2. Volledige DCIEM/IWOD/WOD-tabellen zijn niet integraal hergepubliceerd.
3. Registratie en berekening zijn gescheiden.
4. Werkelijke diepte en tabeldiepte zijn gesplitst.
5. DCIEM-luchtberekening is expliciet beperkt tot ademlucht.
6. Bronconflicten worden neutraal benoemd.
7. Governance, licentie, bronmatrix, schema en publieke basistoetsen zijn toegevoegd.
8. Externe koppelingen kunnen via OSOD-records en mapping worden onderzocht, maar externe systemen zijn niet normatief voor OSOD.
9. Bronnen worden geïdentificeerd via officiële aanduidingen in `BRONNEN.md`; de repository bevat geen kopieën en geen fingerprints van brondocumenten van derden.

## Integriteitscontrole

Alle tekstbestanden gebruiken LF-regeleinden; `.gitattributes` dwingt dit op elk platform af. De SHA-256-controlesommen zijn over deze LF-bytes berekend. Controle vanuit de hoofdmap van de repository:

```text
sha256sum -c CHECKSUMS.sha256
```

Een regeleindeconversie (bijvoorbeeld LF naar CRLF door een uitpakprogramma of editor) laat elke controlesom falen zonder dat de inhoud is gewijzigd. Pak in dat geval het publicatiepakket opnieuw uit met een hulpmiddel dat de bytes ongemoeid laat.

Deze versie is geschikt voor publieke conceptreview, maar nog niet voor formele vaststelling of operationeel gebruik.
