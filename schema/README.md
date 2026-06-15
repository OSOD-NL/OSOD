# Pijler 1: Registratiegegevensmodel

Onderdeel van OSOD v0.1. Dit document beschrijft de mensleesbare veldcatalogus. De machineleesbare eerste versie staat in `osod-logbook-record.schema.json`.

Status: publieke conceptversie. Nog niet vastgesteld. Niet bedoeld voor direct operationeel gebruik zonder eigen verificatie en besluitvorming.

## 1. Doel

Het OSOD-record legt feiten over operationele brandweerduiken vast op een manier die door meerdere systemen kan worden gelezen en getoetst. Het record is niet hetzelfde als de interne opslagstructuur van een implementatie.

## 2. Kernprincipes

### 2.1 Registratie en berekening zijn gescheiden

Een registratie MOET de feitelijke duik kunnen vastleggen, ook wanneer een berekening wordt geblokkeerd. Een rekenkundige blokkade MAG de feitelijke registratie niet wissen, afronden of afvlakken.

### 2.2 Werkelijke diepte en tabeldiepte zijn gescheiden

- `dive.maximaleDiepteWerkelijkM`: feitelijk maximaal bereikte diepte. Geen bovengrens in het schema, behalve dat de waarde niet negatief mag zijn.
- `dive.tabelDiepteM`: tabeldiepte die voor DCIEM-luchtberekening is gebruikt. Alleen `6`, `9`, `12`, `15` of `null`.

Bij een werkelijke diepte boven 15 m blijft `maximaleDiepteWerkelijkM` de werkelijke waarde en wordt `tabelDiepteM` `null` of niet-toepasbaar. De berekening krijgt dan `RED` of `BLOCKED`.

### 2.3 Ademgas bepaalt rekenbaarheid

In v0.1 is Niveau R uitsluitend gedefinieerd voor `ademlucht`. Andere waarden, zoals `nitrox`, `heliox`, `zuurstof`, `anders` of `onbekend`, mogen wel worden geregistreerd, maar mogen niet als geldige DCIEM-luchtberekening worden gepresenteerd.

### 2.4 Brontracering

Elk veld dat normatief is, ZOU herleidbaar moeten zijn naar wettelijke, inspectie-, operationele of rekenkundige bron. De bronmatrix staat in `BRONNEN.md`.

## 3. Top-level structuur

| Veld | Type | Vereist | Norm |
|---|---|---:|---|
| `osodVersion` | string | ja | Constante `0.1`. |
| `recordId` | string/uuid | ja | Stabiele recordidentifier. |
| `recordType` | string | ja | `operationeleDuikregistratie`. |
| `conformanceClaims` | array | ja | Bevat `S` en optioneel `R`. |
| `createdAt` | date-time | ja | Aanmaakmoment record. |
| `updatedAt` | date-time | nee | Laatste wijzigingsmoment. |
| `sourceSystem` | object | nee | Implementatie, versie en exportinformatie. |
| `context` | object | ja | Organisatie, regio, locatie en activiteitstype. |
| `roles` | object | ja | Duikploegleider en duikers. |
| `dive` | object | ja | De feitelijke duikgegevens. |
| `signoff` | object | ja | Aftekening/status. |
| `calculation` | object/null | conditioneel | Vereist bij claim `R`; optioneel bij `S`. |
| `incidentReference` | object/null | nee | Verwijzing naar A0/ongevalsregistratie, niet het volledige medisch dossier. |
| `extensions` | object | nee | Lokale uitbreidingen onder namespace. |

## 4. Context

| Veld | Type | Vereist | Norm |
|---|---|---:|---|
| `context.veiligheidsregio` | string | ja | Naam of code van de veiligheidsregio. |
| `context.duikteam` | string | nee | Regionale teamnaam of code. |
| `context.locatie` | object | ja | Locatie zonder onnodige persoonsgegevens. |
| `context.activiteitType` | enum | ja | `oefening`, `inzet`, `training`, `reintegratie`, `anders`. |
| `context.oefencode` | string/null | nee | Code uit brancherichtlijn of regionaal systeem. OSOD bezit de codelijst niet. |
| `context.omschrijving` | string | nee | Korte neutrale omschrijving. |

## 5. Rollen

OSOD v0.1 gebruikt een sober rollenmodel.

| Rol | Vereist | Norm |
|---|---:|---|
| `duikploegleider` | ja | Persoon die leiding geeft aan de duikarbeid. |
| `duikers[]` | ja | Een of meer duikers. |
| `reserveduiker` | nee | Mag als rol of als eigenschap bij duiker worden geregistreerd. |
| `signaalhouder` | nee | Operationele functie; geen aparte landelijke persoonsregistratierol in OSOD v0.1. |
| `duikmedischBegeleider` | nee | In brandweercontext vaak belegd bij de duikploegleider; alleen opnemen als lokale registratie dit vereist. |

Een persoonobject bevat bij voorkeur een pseudonieme of lokale identifier. Volledige persoonsgegevens worden alleen opgenomen wanneer de lokale wettelijke en organisatorische grondslag dat vereist.

## 6. Duikgegevens

| Veld | Type | Vereist | Norm |
|---|---|---:|---|
| `dive.datum` | date | ja | Datum van de duik. |
| `dive.tijdIn` | time | ja | Aanvang afdaling / tijd in. |
| `dive.tijdUit` | time | nee | Eindtijd / tijd uit. |
| `dive.duiktijdMin` | integer | ja | Duiktijd in minuten. |
| `dive.totaleTijdOnderDrukMin` | integer/null | nee | Totaal onder druk; verplicht wanneer lokaal of wettelijk vereist. |
| `dive.maximaleDiepteWerkelijkM` | number | ja | Werkelijke maximale diepte in meters. Niet begrenzen op 15 m. |
| `dive.tabelDiepteM` | 6/9/12/15/null | nee | Tabeldiepte indien berekend. |
| `dive.gevolgdSchema` | object | ja | Naam/type/bron van het gevolgde duikschema. |
| `dive.decompressieverloop` | object | ja | Feitelijk gevolgd decompressieverloop; ook expliciet opnemen bij no-deco. |
| `dive.ademgas` | enum | ja | `ademlucht`, `zuurstof`, `nitrox`, `heliox`, `anders`, `onbekend`. |
| `dive.duiksysteem` | enum | ja | `SCUBA`, `SCUBA_OLV`, `SSE`, `anders`, `onbekend`. |
| `dive.aardDuikarbeid` | enum/string | ja | Oefening, inzet, redding, berging, training, re-integratie of anders. |
| `dive.aantalDuikers` | integer | ja | Totaal aantal ingezette duikers. |
| `dive.bijzondereSessies` | array | ja | Ook leeg expliciet opnemen. |

### 6.1 Waarden van dive.duiksysteem

- SCUBA: zelfstandig duiktoestel. De ademluchtvoorziening komt uit drukvaten die de duiker meedraagt, onafhankelijk van de oppervlakte.
- SCUBA_OLV: SCUBA-duikuitrusting met aansluiting op een hogedrukluchtvoorziening vanaf de oppervlakte, waarbij de ademlucht pas bij de meegedragen ademgasvoorziening tot lage druk wordt gereduceerd. Het meegedragen SCUBA-toestel is daarbij de noodluchtvoorziening. Dit komt overeen met de configuratie van registratiescope A15OLV en met de brandweerinzet van een SCUBA-duiktoestel aangevuld met oppervlakteluchtvoorziening.
- SSE: surface supplied equipment. Duiksysteem dat standaard is voorzien van ademgasvoorziening vanaf de oppervlakte, waarbij een of meer duikers zijn aangesloten op een duikpaneel; bedoeld voor zware werkzaamheden.
- anders: een duiksysteem dat niet onder de bovenstaande waarden valt.
- onbekend: het duiksysteem is niet vastgelegd of niet meer te achterhalen.

Toelichting bij het vervallen van een losse waarde OLV (besluit B-0018): in de geldende werkinstructies komt OLV uitsluitend voor als aanvulling op een SCUBA-systeem; oppervlaktelucht zonder volledig meegedragen toestel heet SSE. Een registratie die elders de kale aanduiding OLV gebruikt, zoals op het intakeformulier duikongeval (A0-lijst), wordt in OSOD vastgelegd als SCUBA_OLV.

Bronnen: Werkinstructie Duikarbeid, Nederlandse Arbeidsinspectie, 11 december 2025 (duiktechnieken SCUBA en SSE; registratiescope A15OLV). Landelijke Werkinstructie Werken onder overdruk brandweer, versie 2.0, 3 december 2024, paragraaf 4.1 en begrippenlijst (OLV als aanvulling op SCUBA; het meegedragen toestel als noodluchtvoorziening).

## 7. Aftekening

| Veld | Type | Vereist | Norm |
|---|---|---:|---|
| `signoff.duikploegleiderGetekend` | boolean | ja | Aftekening door DPL of digitale status. |
| `signoff.werkgeverOfOpdrachtgeverGetekend` | boolean | ja | Aftekening namens werkgever/opdrachtgever of digitale status. |
| `signoff.status` | enum | ja | `concept`, `vastgelegd`, `gecorrigeerd`, `ongeldig`. |
| `signoff.toelichting` | string | nee | Reden bij correctie of ongeldigheid. |

Niet afgetekende of onjuist ingevulde logbladen kunnen in wettelijke/operationele context gevolgen hebben. OSOD legt de status vast; lokale procedures bepalen de formele werking.

## 8. Berekening

`calculation` is vereist wanneer `conformanceClaims` de waarde `R` bevat. Bij alleen Niveau S mag `calculation` `null` zijn of ontbreken.

| Veld | Type | Vereist bij R | Norm |
|---|---|---:|---|
| `calculation.status` | enum | ja | `NOT_CALCULATED`, `GREEN`, `AMBER`, `RED`, `BLOCKED`. |
| `calculation.resultValid` | boolean | ja | `false` bij `RED` of `BLOCKED`. |
| `calculation.engine` | object | ja | Naam, versie, bronfingerprint. |
| `calculation.inputEnvelope` | object | ja | Vastgelegde invoer die voor de berekening is gebruikt. |
| `calculation.outputs` | object | nee | HG, HF, nultijd, meldingen indien van toepassing. |
| `calculation.blockingReasons` | array | ja | Leeg bij geldig resultaat; gevuld bij RED/BLOCKED. |
| `calculation.messages` | array | ja | Mensleesbare meldingen. |

### 8.1 Machinaal afgedwongen invarianten

De volgende kruisveldregels worden sinds deze versie machinaal afgedwongen in `osod-logbook-record.schema.json` en zijn niet langer alleen proza:

1. Bij `status` `RED` of `BLOCKED` MOET `resultValid` `false` zijn en MOET `blockingReasons` minimaal één reden bevatten.
2. Bij `resultValid` `true` MOET `blockingReasons` leeg zijn.
3. Bij `status` `GREEN` MOET `blockingReasons` leeg zijn.

Records die deze regels schenden, valideren niet tegen het schema. De voorbeelden in `voorbeelden/ongeldig/` tonen dit gedrag. Wijzigingen aan deze invarianten zijn veiligheidswijzigingen in de zin van `STANDAARD.md`, sectie 7.

## 9. Incident- en A0-verwijzing

OSOD neemt in v0.1 niet het volledige medische incidentdossier op. Wel kan een record verwijzen naar een A0- of ongevalsregistratie:

| Veld | Type | Norm |
|---|---|---|
| `incidentReference.hasIncidentRecord` | boolean | Geeft aan of er een afzonderlijk incident-/A0-record bestaat. |
| `incidentReference.referenceId` | string/null | Lokale identifier, geen medische inhoud. |
| `incidentReference.summary` | string/null | Alleen niet-medische korte verwijzing indien noodzakelijk. |

## 10. Extensies

Lokale velden horen onder `extensions` met een namespace, bijvoorbeeld:

```json
{
  "extensions": {
    "vr-demo": {
      "regionaalFormulierNummer": "VR-DEMO-2026-001"
    }
  }
}
```

Extensies mogen verplichte OSOD-velden niet vervangen of van betekenis veranderen.

## 11. Formuliertracering Brandweer Duiklogboek

Het Brandweer Duiklogboek (IFV/Brandweeracademie, ISBN 978-90-5643-467-0) is het papieren bronformulier voor de operationele duikregistratie. De uitgave behoudt alle rechten voor; deze tracering legt daarom alleen de veldkoppeling vast, geen reproductie van de uitgave.

De tracering geldt voor de operationele logbladen: Duiklog Duiker (Deel 2, in te vullen direct na de duik) en Duiklog Duikploegleider (Deel 3, in te vullen direct na de duik). De invul-instructies van beide delen zijn inhoudelijk gelijk. Beide logbladen volgen voor het duikschema de bijlage DCIEM-tabellen van de uitgave, met tabelopbouw conform IWOD, 1 april 2019, gelijk aan de rekenbron waarop OSOD pint.

| OSOD-veld | Deel 2: Duiklog Duiker | Deel 3: Duiklog Duikploegleider |
|---|---|---|
| `dive.datum` | Datum duik | Datum duik |
| `context.locatie.omschrijving` | Duiklocatie | Duiklocatie |
| `context.activiteitType`, `dive.aardDuikarbeid` | Betreft een: oefening / inzet | Betreft een: oefening / inzet |
| `dive.bijzondereSessies` | Specifiek: zwembad-, toren-, re-integratietraining | Specifiek: zwembad-, toren-, VR-, re-integratietraining |
| `dive.duiksysteem` | Uitrusting: SCUBA / SSE | Uitrusting: SCUBA / SSE |
| `dive.gevolgdSchema` (`DCIEM_AIR_TABLE`), `dive.tabelDiepteM` | Air tabel 1: 6 / 9 / 12 / 15 meter | Air tabel 1: 6 / 9 / 12 / 15 meter |
| `dive.gevolgdSchema` (`METERREGEL`) | Er is gebruik gemaakt van de 6 / 9 / 12 meter regel | Er is gebruik gemaakt van de 6 / 9 / 12 meter regel |
| `calculation.outputs` (herhalingsgroep) | HG | HG per duiker |
| `dive.tijdIn`, `dive.tijdUit` | Tijd in / Tijd uit | Tijd in (eerste duiker) / Tijd uit (laatste duiker) |
| `dive.duiktijdMin` | Duiktijd | Duiktijd; DT per duiker 1 t/m 4 |
| `dive.maximaleDiepteWerkelijkM` | Maximale duikdiepte | Maximale duikdiepte |
| `calculation.inputEnvelope` (opgetelde duiktijd meterregel) | Minutenstaatje: voorgaande (totale) DT / nieuwe DT / totaal DT | DT per duiker |
| `context.oefencode`, `context.omschrijving` | Korte beschrijving duikarbeid / oefenkaart(en) | Korte beschrijving duikarbeid / oefenkaart(en) |
| `roles.duikploegleider` (met `registrationNumber`) | Naam en handtekening duikploegleider; DPL no. | Handtekening DPL; DPL no. |
| `signoff.duikploegleiderGetekend` | Aftekening door DPL | Handtekening DPL |
| `signoff.werkgeverOfOpdrachtgeverGetekend` | n.v.t. op dit blad | Handtekening namens werkgever of DPL |
| `dive.aantalDuikers` | n.v.t. (persoonlijk blad) | Afleidbaar uit DT duiker 1 t/m 4 |

Traceringsbevindingen:

1. Het minutenstaatje op het Deel 2-blad (voorgaande, nieuwe en totale duiktijd) en het aankruisveld voor de 6/9/12-meterregel bevestigen de cumulatieve duiktijdbewaking die het rekencontract in sectie 4 normatief maakt.
2. De invul-instructie van Deel 2 staat toe onderbroken duikarbeid opgeteld op één logblad weer te geven wanneer de herhalings- of oppervlakte-intervallen binnen 15 minuten zijn gehouden. Dit is informatief vastgelegd; de normatieve uitwerking van deze samenvoegvoorwaarde is een open punt.
3. Het Deel 2-blad vereist een persoonlijke handtekening van de duiker. Het OSOD-aftekenmodel kent in v0.1 alleen `duikploegleiderGetekend` en `werkgeverOfOpdrachtgeverGetekend`. Of een veld voor duikeraftekening nodig is voor persoonlijk-logboekgebruik is een open punt.

Deel 1 (Basisinformatie) bevat persoonsregistratie, keuringen, opleidingen en de DCIEM-bijlage. Die onderdelen vallen buiten het operationele duikrecord; de DCIEM-bijlage en de meterregeluitwerking zijn als rekenbroncontext verwerkt via het rekencontract.

## 12. Open punten

1. Normatieve uitwerking van de samenvoegvoorwaarde voor onderbroken duikarbeid (intervallen binnen 15 minuten, invul-instructie Deel 2).
2. Beoordelen of een aftekenveld voor de duiker nodig is voor persoonlijk-logboekgebruik (Deel 2 vereist handtekening duiker).
3. Definitieve lijst met aanbevolen lokale velden voor duikrapporten.
4. Definitief privacyprofiel voor pseudonimisering en bewaartermijnen.
5. Volledige machineleesbare toetsset voor Niveau R zodra tabelpublicatie en beheerafspraken zijn vastgesteld.
