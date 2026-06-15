# Conformiteit

Onderdeel van OSOD v0.1. Dit document beschrijft conformiteitsniveaus en toetsing. Het rekencontract zelf staat in `rekencontract/README.md`.

Status: publieke conceptversie. Nog niet vastgesteld. Niet bedoeld voor direct operationeel gebruik zonder eigen verificatie en besluitvorming.

## 1. Conformiteitsniveaus

OSOD kent twee niveaus:

- **Niveau S, schema:** de implementatie leest en schrijft OSOD-records conform `schema/osod-logbook-record.schema.json` en `schema/README.md`.
- **Niveau R, rekenen:** de implementatie ondersteunt Niveau S en voert de DCIEM-luchtberekening uit conform `rekencontract/README.md`.

Een implementatie MOET expliciet aangeven welk niveau zij claimt.

## 2. Toetsing

Een conformiteitsclaim wordt onderbouwd met:

- schema-validatie;
- bron- en versievermelding;
- rekenbronfingerprint wanneer Niveau R wordt geclaimd;
- publieke basistoetsen;
- aanvullende besloten of beheerderstoetsen waar volledige tabellen niet publiek worden hergepubliceerd.

## 3. Publieke basistoetsen

De publieke basistoetsen zijn beperkt en bedoeld om implementatiegedrag te toetsen zonder volledige tabelherpublicatie.

Bestand:

```text
conformiteit/toetsvectoren/basistoetsen-v0.1.json
```

Deze set is niet volledig. Een implementatie die Niveau R claimt, ZOU daarnaast een besloten of beheerderstoets tegen de volledige bron moeten kunnen doorlopen.

## 4. Resultaat van toetsing

Een toetsresultaat ZOU minimaal bevatten:

```text
OSOD-versie
implementatienaam
implementatieversie
geclaimd niveau
schema-versie
bronfingerprint indien Niveau R
uitgevoerde toetssets
resultaat per toets
afwijkingen
```

## 5. Niet-normatieve implementatieschetsen

Documenten in `implementatie/` kunnen beschrijven hoe een implementatie praktisch naar OSOD-conformiteit kan groeien. Zulke documenten zijn informatief en leggen geen verplichte technologie of bestandsindeling op.
