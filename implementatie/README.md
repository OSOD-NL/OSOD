# Implementaties

Deze map bevat informatieve documenten over implementaties. Documenten in deze map zijn niet normatief voor OSOD.

Een implementatie kan OSOD-conformiteit claimen op Niveau S of Niveau S+R. De claim moet aantoonbaar zijn met schema-validatie, bronfingerprints en toetsvectoren.

## Duikmonitor

Duikmonitor is de beoogde eerste referentie-implementatie. De documenten in deze map beschrijven daarom drie dingen:

| Bestand | Doel |
|---|---|
| `duikmonitor-mapping.md` | Veldkoppeling van Duikmonitor naar OSOD. |
| `duikmonitor-conformiteit.md` | Hoe Duikmonitor een OSOD-conformiteitsclaim kan onderbouwen. |
| `duikmonitor-architectuurschets.md` | Niet-normatieve bouwrichting: rekenkern, OSOD-mapping/export en conformiteitscontrole apart houden. |

De architectuurschets is geen verplichte bestandsindeling. De genoemde functies kunnen in een HTML-only applicatie staan of later naar losse JavaScript-bestanden worden gesplitst.

## Mapping naar externe systemen

Een externe koppeling hoort bij voorkeur te verlopen via een OSOD-record:

```text
implementatie
→ OSOD-record
→ mapping naar extern systeem
```

Een extern systeem, importbestand of API is geen normbron voor OSOD. De mapping mag de betekenis van OSOD-velden niet veranderen en mag feitelijke waarden niet afvlakken.
