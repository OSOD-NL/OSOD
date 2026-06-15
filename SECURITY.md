# Veiligheid en meldingen

OSOD raakt veiligheidskritische duikregistratie en rekenkundige blokkades. Behandel meldingen over fouten in schema, rekencontract of bronverwijzing daarom als veiligheidsrelevant.

## Geen operationele afhankelijkheid

OSOD v0.1 is een publieke conceptversie. Gebruik deze versie niet als zelfstandige basis voor operationele inzet zonder eigen verificatie, besluitvorming en aansluiting op de geldende procedures.

## Veiligheidskritische onderwerpen

De volgende onderwerpen zijn veiligheidskritisch:

- DCIEM-/IWOD-rekencontract;
- tabelbron, tabelenvelop en bronfingerprint;
- blokkeergedrag buiten de envelop;
- statusmapping naar `GREEN`, `AMBER`, `RED`, `BLOCKED` of `NOT_CALCULATED`;
- onderscheid tussen werkelijke diepte en tabeldiepte;
- ademgasbeperking voor DCIEM-luchtberekening;
- toetsvectoren met rekenkundige verwachte uitkomsten.

## Kernprincipe

Een rekenende implementatie MOET blokkeren en melden wanneer invoer buiten de gevalideerde envelop valt. Een implementatie MOET NIET stilzwijgend extrapoleren, afvlakken, afkappen toepassen of een ongeldige berekening als geldig presenteren.

## Privacy

Meldingen, voorbeelden en toetsdata mogen geen echte persoonsgegevens, medische gegevens of herleidbare incidentgegevens bevatten.

## Melding

Gebruik in de conceptfase een besloten reviewkanaal of besloten melding aan de beheerder voor veiligheidsbevindingen. Vermeld minimaal:

```text
OSOD-versie
Betrokken bestand/sectie
Probleem
Veiligheidsimpact
Reproduceerbare input zonder persoonsgegevens
Verwachte veilige uitkomst
Waargenomen onveilige uitkomst
Bron of onderbouwing
```
