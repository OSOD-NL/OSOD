# OSOD

**OSOD** staat voor **Open Standaard Operationele Duikregistratie**, een standaard voor de Nederlandse brandweer.

Versie: **0.1**  
Status: **publieke conceptversie**. Nog niet vastgesteld. OSOD is een onafhankelijk initiatief en geen uitgave van een veiligheidsregio of bronorganisatie. Niet bedoeld voor direct operationeel gebruik zonder eigen verificatie, bestuurlijke besluitvorming en aansluiting op de geldende procedures van de veiligheidsregio.

## Doel

OSOD beschrijft hoe een operationeel brandweerduiklogboekrecord uniform, machineleesbaar en uitwisselbaar kan worden vastgelegd. De standaard is bedoeld als gemeenschappelijke basis voor veiligheidsregio's, leveranciers, referentie-implementaties en toetsende partijen.

OSOD is een **standaard**, geen applicatie. De standaard beschrijft het record, de rekenreferentie, de conformiteitseisen en het beheer. De gebruikersinterface, lokale werkwijze, operationele besluitvorming en formele vaststelling blijven buiten deze repository.

## Leidende volgorde

OSOD wordt niet afgeleid uit één applicatie. De leidende volgorde is:

```text
wet- en regelgeving
→ registratieschema's en inspectiekaders
→ brandweer-operationele werkinstructies
→ OSOD-standaard
→ implementaties, waaronder Duikmonitor
```

Duikmonitor kan een referentie-implementatie zijn, maar is **niet normatief** voor OSOD. Een afwijking tussen Duikmonitor en OSOD wordt opgelost door de implementatie aan de standaard te toetsen, niet andersom.

Voor Duikmonitor wordt daarnaast een informatief implementatiepad beschreven: rekenkern, OSOD-mapping/export en conformiteitscontrole worden bij voorkeur apart gehouden. Dat is een praktische route naar aantoonbare OSOD-conformiteit, geen verplichte technologiekeuze.

## Bronhiërarchie

Voor OSOD v0.1 geldt de volgende bronhiërarchie:

1. Arbobesluit, Arbeidsomstandighedenregeling en daarop gebaseerde publicaties.
2. Staatscourant 2024-registratieschema's voor duiker, duikploegleider, duikmedisch begeleider en duikerarts.
3. Werkinstructie Duikarbeid van de Nederlandse Arbeidsinspectie als inspectie- en minimumcontrolekader.
4. Landelijke Werkinstructie Werken onder overdruk brandweer als brandweer-operationeel kader.
5. IWOD/DCIEM als rekenkundige bron voor de DCIEM-luchttabel binnen de vastgelegde envelop.
6. IFV/NIPV-logboek, A0-lijst en brancherichtlijn als formulier- en praktijkbronnen, voor zover herleidbaar en juridisch herpubliceerbaar.
7. Duikmonitor als beoogde eerste referentie-implementatie en bron van mogelijke toetsgevallen.

## Inhoud

| Pad | Doel |
|---|---|
| `STANDAARD.md` | Hoofdtekst van OSOD v0.1. |
| `schema/README.md` | Mensleesbare veldcatalogus en normen voor Niveau S. |
| `schema/osod-logbook-record.schema.json` | Eerste machineleesbare JSON Schema voor het OSOD-logboekrecord. |
| `rekencontract/README.md` | Rekencontract, rekenbron, envelop en blokkeergedrag voor Niveau R. |
| `conformiteit/README.md` | Conformiteitsniveaus en toetsaanpak. |
| `conformiteit/toetsvectoren/basistoetsen-v0.1.json` | Beperkte publieke basistoetsen zonder herpublicatie van volledige tabellen. |
| `voorbeelden/` | Voorbeeldrecords zonder persoonsgegevens. |
| `BRONNEN.md` | Bronhiërarchie, bronmatrix en publicatiebeleid. |
| `implementatie/` | Niet-normatieve mapping, conformiteitsnotities en Duikmonitor-architectuurschets. |
| `GOVERNANCE.md` | Beheer- en wijzigingsmodel. |
| `CONTRIBUTING.md` | Bijdragerregels. |
| `SECURITY.md` | Veiligheids- en meldproces. |
| `LICENSE` | Licentie-indeling voor standaardtekst en machineleesbare artefacten. |
| `PUBLICATIE.md` | Samenvatting van publicatiekeuzes in deze v0.1. |

## Conformiteitsniveaus

OSOD v0.1 maakt onderscheid tussen:

- **Niveau S, schema:** een implementatie leest en schrijft OSOD-records conform het schema.
- **Niveau R, rekenen:** een implementatie voert de DCIEM-luchtberekening uit conform het rekencontract, blokkeert buiten de gevalideerde envelop en kan dat aantonen met toetsvectoren.

Een implementatie MAG Niveau S ondersteunen zonder Niveau R te claimen. Een implementatie die Niveau R claimt, MOET ook Niveau S ondersteunen.

## Externe systemen

OSOD-records kunnen worden gebruikt als basis voor export naar externe systemen, bijvoorbeeld via een importbestand, API of veldmapping. Zo'n koppeling is niet normatief voor OSOD en mag de betekenis van OSOD-velden niet veranderen.

De voorkeursvolgorde is:

```text
Duikmonitor of andere invoerlaag
→ OSOD-record
→ eventuele mapping naar extern systeem
```

## Publicatie- en rechtenbeleid

Deze publieke conceptversie herpubliceert geen volledige beschermde bronuitgaven en geen volledige DCIEM/IWOD-tabellen. Waar bronwaarden nodig zijn voor toetsing, worden in v0.1 alleen beperkte publieke basistoetsen opgenomen. De volledige numerieke bron blijft extern of privaat beheerd totdat herpublicatierechten en governance expliciet zijn vastgesteld.

## Privacy

Zet geen echte namen, geboortedata, telefoonnummers, e-mailadressen, locaties, incidentgegevens of andere persoonsgegevens in issues, voorbeelden, toetsdata of pull requests. Gebruik neutrale waarden zoals `Duiker 1`, `DPL 1` en fictieve locaties.

## Status

Dit is een **publieke conceptversie**. Zij is bedoeld voor review, standaardvorming en implementatievoorbereiding. Zij is niet bedoeld als zelfstandig operationeel voorschrift.
