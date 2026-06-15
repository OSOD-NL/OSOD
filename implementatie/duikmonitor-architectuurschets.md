# Duikmonitor-architectuurschets richting OSOD

Status: informatief, niet normatief.

Dit document beschrijft één mogelijke manier om Duikmonitor als beoogde eerste referentie-implementatie OSOD-conform te maken. Deze schets is geen eis aan andere implementaties. Andere systemen mogen een andere technische indeling gebruiken zolang zij aantoonbaar voldoen aan OSOD.

## 1. Twee sporen

OSOD v0.1 kent in deze publiceerbare conceptversie twee duidelijk gescheiden sporen:

```text
Spoor 1: OSOD als standaard
Spoor 2: Duikmonitor als implementatiepad richting OSOD-conformiteit
```

Spoor 1 is normatief en staat in `STANDAARD.md`, `schema/`, `rekencontract/` en `conformiteit/`.

Spoor 2 is informatief en staat in `implementatie/`. Dit spoor beschrijft hoe Duikmonitor de standaard praktisch kan volgen zonder dat Duikmonitor de standaard bepaalt.

## 2. Uitgangspunt

Duikmonitor hoeft niet opnieuw gebouwd te worden om OSOD-conform te kunnen worden. Ook een HTML-only applicatie kan lokaal:

- DCIEM/IWOD-invoer valideren;
- een berekening uitvoeren of blokkeren;
- een OSOD-record opbouwen;
- het OSOD-record tegen het schema controleren;
- toetsvectoren uitvoeren;
- JSON, CSV of PDF exporteren.

De technische indeling hieronder is dus een bouwprincipe. De functies kunnen in één `index.html` staan, of later worden gesplitst naar losse JavaScript-bestanden.

## 3. Aanbevolen scheiding

Voor Duikmonitor is deze scheiding wenselijk:

```text
1. DCIEM-rekenkern
2. OSOD-mapping en export
3. Conformiteitscontrole
```

Het doel is dat de rekenlogica, de OSOD-export en de toetsing apart begrijpelijk en testbaar zijn.

## 4. Mogelijke bestandsindeling

Onderstaande namen zijn voorbeelden. Ze zijn niet verplicht.

```text
dciem-core.js
  DCIEM_TABLES
  validateDciemEnvelope()
  calculateDciemSingleDive()
  calculateDciemRepetitiveDive()
  explainDciemTrace()

osod-adapter.js
  exportOsodRecord()
  validateOsodRecord()
  mapDuikmonitorStateToOsod()
  mapCalculationToOsod()

conformance.js
  runOsodSchemaTests()
  runOsodDciemVectors()
  runOsodExportBasiscontrole()
```

Wanneer Duikmonitor één HTML-bestand blijft, kan dezelfde indeling als interne codeblokken of namespaces worden toegepast.

## 5. DCIEM-rekenkern

De DCIEM-rekenkern hoort alleen verantwoordelijk te zijn voor rekenen en rekenveiligheid.

De rekenkern ZOU minimaal moeten kunnen:

- de gebruikte DCIEM/IWOD-bron en fingerprint tonen;
- controleren of invoer binnen de DCIEM-/brandweer-envelop valt;
- enkelvoudige duiken berekenen;
- herhalingsduiken berekenen;
- ademgas beperken tot `ademlucht` voor Niveau R;
- lege of uitgesloten tabelcellen blokkeren;
- een blokkeerreden teruggeven;
- een uitlegbaar rekenpad of trace teruggeven.

De rekenkern mag geen feitelijke registratie verwijderen of afvlakken. Een diepte buiten de envelop moet in de registratie kunnen blijven staan, terwijl de berekening blokkeert.

## 6. OSOD-mapping en export

De OSOD-adapter of mappinglaag vertaalt Duikmonitor-interne gegevens naar een OSOD-record.

Deze laag ZOU minimaal moeten kunnen:

- interne project- en livegegevens mappen naar `context`, `roles`, `dive`, `signoff` en `calculation`;
- `maximaleDiepteWerkelijkM` gescheiden houden van `tabelDiepteM`;
- het rekenresultaat mappen naar OSOD-statussen zoals `GREEN`, `AMBER`, `RED` en `BLOCKED`;
- blokkeerredenen opnemen in `calculation.blockingReasons`;
- een stabiel OSOD JSON-record exporteren;
- voorkomen dat een volledige interne app-state dump als OSOD-record wordt gepresenteerd.

De mappinglaag mag ontbrekende verplichte OSOD-velden niet stilzwijgend verzinnen. Als gegevens ontbreken, moet dat zichtbaar worden in validatie of aftekenstatus.

## 7. Conformiteitscontrole

De conformiteitslaag toont of een Duikmonitor-versie voldoet aan OSOD v0.1.

Deze laag ZOU minimaal moeten kunnen:

- OSOD JSON-records valideren tegen `schema/osod-logbook-record.schema.json`;
- publieke basistoetsen uitvoeren;
- DCIEM-vectoren uitvoeren voor zover bronwaarden en rechten beschikbaar zijn;
- exportbasiscontrole uitvoeren;
- een toetsresultaat tonen met versie, datum, bronfingerprint en afwijkingen.

Een mogelijke verklaring is:

```text
Applicatie: Duikmonitor
Applicatieversie: <versie>
OSOD-versie: 0.1
Claim: Niveau S of Niveau S+R
Schema: schema/osod-logbook-record.schema.json
Toetsvectoren: conformiteit/toetsvectoren/basistoetsen-v0.1.json
Resultaat: geslaagd / niet geslaagd / afwijkingen
```

## 8. Externe systemen

Een eventuele koppeling naar een extern systeem hoort na het OSOD-record te komen:

```text
Duikmonitor
→ OSOD-record
→ mapping naar extern systeem
```

Een externe API, importbestand of formuliermodule is geen normbron voor OSOD. Een externe mapping mag OSOD-velden niet herdefiniëren en mag feitelijke waarden niet afvlakken.

## 9. Grens van HTML-only

Een HTML-only applicatie is voldoende voor lokale OSOD-export, lokale validatie en lokale basistoetsen.

Een HTML-only applicatie is niet geschikt om geheime API-sleutels veilig te bewaren. Rechtstreekse koppeling met een extern systeem kan alleen verantwoord als het externe systeem daar officieel in voorziet en de authenticatie past bij een browserclient. Anders ligt export naar bestand of een backend-koppeling meer voor de hand.

## 10. Niet-normatief

Dit document legt geen verplichte technologie, bestandsnamen of programmeertaal op. Het beschrijft alleen een praktische route om Duikmonitor aantoonbaar dichter bij OSOD-conformiteit te brengen.
