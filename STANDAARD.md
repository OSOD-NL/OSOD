# OSOD: Open Standaard Operationele Duikregistratie

Voluit: **Open Standaard Operationele Duikregistratie**, gericht op de Nederlandse brandweer.  
Korte naam: **OSOD**.  
Versie: **0.1**.  
Status: **publieke conceptversie**. Nog niet vastgesteld. Niet bedoeld voor direct operationeel gebruik zonder eigen verificatie, besluitvorming en aansluiting op geldende procedures.

OSOD legt vast hoe een operationeel brandweerduiklogboekrecord uniform, machineleesbaar en uitwisselbaar wordt vastgelegd. OSOD is een standaard, geen applicatie. Implementaties, waaronder Duikmonitor, kunnen OSOD volgen en conformiteit aantonen, maar bepalen de norm niet.

## 1. Normatieve positie

### 1.1 Leidende volgorde

OSOD volgt deze volgorde:

```text
wet- en regelgeving
→ registratieschema's en inspectiekaders
→ brandweer-operationele werkinstructies
→ OSOD-standaard
→ implementaties
```

Duikmonitor is de eerste beoogde referentie-implementatie, maar is **niet normatief**. Een afwijking tussen Duikmonitor en OSOD wordt behandeld als implementatie- of standaardinterpretatievraag, niet als automatische wijziging van OSOD.

### 1.2 Bronhiërarchie

Voor OSOD v0.1 geldt:

1. Arbobesluit, Arbeidsomstandighedenregeling en daarop gebaseerde wettelijke publicaties.
2. Staatscourant 2024-registratieschema's voor duiker, duikploegleider, duikmedisch begeleider en duikerarts.
3. Werkinstructie Duikarbeid van de Nederlandse Arbeidsinspectie als inspectie- en minimumcontrolekader.
4. Landelijke Werkinstructie Werken onder overdruk brandweer als brandweer-operationeel kader.
5. IWOD/DCIEM-luchttabel als rekenkundige bron voor Niveau R.
6. IFV/NIPV-logboek, A0-lijst en brancherichtlijn als formulier-, praktijk- en vakbekwaamheidsbronnen.
7. Duikmonitor als beoogde eerste referentie-implementatie en bron van mogelijke toetsgevallen.

### 1.3 Normatieve taal

De sleutelwoorden **MOET**, **MOET NIET**, **ZOU MOETEN**, **ZOU NIET MOETEN** en **MAG** worden gebruikt in de betekenis van RFC 2119. Een implementatie die een **MOET**-eis niet haalt, is voor dat onderdeel niet conform.

## 2. Doel en scope

### 2.1 Probleem

De wettelijke en operationele kaders beschrijven welke gegevens in persoonlijke logboeken en operationele registraties thuishoren. Wat ontbreekt is één open, machineleesbare structuur waarmee systemen records op dezelfde manier kunnen vastleggen, uitwisselen en toetsen.

### 2.2 Doel

OSOD definieert een open kern voor:

- registratie van operationele brandweerduiken;
- koppeling aan persoonsregistratie en vakbekwaamheidscontext;
- vastlegging van gevolgde schema's en decompressieverloop;
- toetsbare DCIEM-luchtberekening binnen de gevalideerde envelop;
- conformiteitstoetsen voor leveranciers en referentie-implementaties.

Het doel is uniformiteit van het record en de toetsregels, niet uniformiteit van de applicatie.

### 2.3 Binnen scope

- Operationeel logboekrecord voor brandweerduikregistratie.
- Verplichte en aanbevolen velden voor Niveau S.
- Rekencontract voor DCIEM-luchttabelgebruik binnen Niveau R.
- Blokkeer- en meldgedrag bij invoer buiten de gevalideerde rekenkundige envelop.
- Bronverantwoording, rekenbronfingerprint, governance en conformiteit.
- Beperkte publieke toetsvectoren zonder herpublicatie van volledige beschermde tabellen.

### 2.4 Buiten scope

- Gebruikersinterface, workflow en schermontwerp.
- Operationele besluitvorming op de duiklocatie.
- Formele vaststelling door een veiligheidsregio of landelijke vakgroep.
- Een verplicht softwareproduct.
- Recreatief of technisch duiken buiten de brandweer-/WOD-context.
- DCIEM-berekening voor nitrox, heliox, zuurstof of onbekend ademgas.
- Herpublicatie van volledige bronuitgaven of tabellen zonder vastgestelde rechten.

## 3. Conformiteitsniveaus

### 3.1 Niveau S: Schema

Een implementatie claimt Niveau S als zij OSOD-records kan lezen, schrijven en valideren volgens `schema/osod-logbook-record.schema.json` en de mensleesbare regels in `schema/README.md`.

Niveau S vereist geen eigen DCIEM-rekenmotor. Een Niveau S-implementatie MOET wel het gevolgde schema, het gevolgde decompressieverloop en feitelijke waarden kunnen registreren.

### 3.2 Niveau R: Rekenen

Een implementatie claimt Niveau R als zij:

1. Niveau S ondersteunt;
2. de DCIEM-luchtberekening uitvoert binnen de vastgelegde envelop;
3. ademgasvoorwaarden respecteert;
4. buiten bereik blokkeert en meldt;
5. publieke en, waar beschikbaar, besloten toetsvectoren haalt;
6. de gebruikte rekenbron en fingerprint kan tonen.

Niveau R geldt in v0.1 uitsluitend voor **ademlucht** binnen de DCIEM-luchttabel. Bij nitrox, heliox, zuurstof of onbekend ademgas MAG geen geldige DCIEM-luchtuitkomst worden gepresenteerd.

## 4. Pijler 1: Registratiegegevensmodel

Het OSOD-record beschrijft feiten over een operationele duik. Het record MOET onderscheid maken tussen:

- feitelijke registratie;
- toegepaste of geplande tabelwaarden;
- rekenkundige beoordeling;
- operationele of lokale besluitvorming.

### 4.1 Feitelijke waarden mogen niet worden afgevlakt

Een implementatie MOET de werkelijk geregistreerde waarde kunnen vastleggen, ook als die buiten de toegestane scope of rekenkundige envelop ligt.

Daarom splitst OSOD minimaal:

```text
maximaleDiepteWerkelijkM
```

en, indien van toepassing:

```text
tabelDiepteM
```

Voorbeeld: een feitelijke diepte van 16,4 m wordt als 16,4 m geregistreerd. De DCIEM-luchtberekening wordt geblokkeerd omdat de brandweer-no-deco-envelop tot 15 m loopt. De registratie blijft bestaan.

### 4.2 Minimale recordonderdelen

Een OSOD-record bevat minimaal:

- OSOD-versie en record-id;
- datum en tijden;
- context: veiligheidsregio, duikteam, locatie en activiteitstype;
- rollen: duikploegleider en een of meer duikers;
- feitelijke maximale diepte;
- duiktijd of aanvang/eindtijd;
- gevolgd schema;
- gevolgd decompressieverloop;
- ademgas;
- duiksysteem of uitrusting;
- aard van de duikarbeid;
- bijzondere sessies of afwijkingen;
- aftekenstatus;
- optioneel of verplicht rekenresultaat afhankelijk van conformiteitsclaim.

### 4.3 Privacy-minimalisatie

OSOD schrijft geen volledige persoonsgegevens voor als een stabiele, lokaal herleidbare identifier volstaat. Implementaties ZOUden persoonsgegevens zoveel mogelijk moeten pseudonimiseren of minimaliseren, met behoud van wettelijke controleerbaarheid binnen de veiligheidsregio.

## 5. Pijler 2: Rekencontract

### 5.1 Rekenbron

De numerieke rekenbron voor OSOD Niveau R is de IWOD/DCIEM-luchttabel zoals vastgelegd in de bronmatrix. Deze publieke repository herpubliceert de volledige tabelwaarden niet in v0.1. Implementaties MOETEN de gebruikte bronversie en fingerprint kunnen tonen.

### 5.2 Ademgasbeperking

Niveau R geldt uitsluitend voor ademlucht binnen de DCIEM-luchttabel. Indien het ademgas niet `ademlucht` is, MOET een rekenende implementatie de DCIEM-luchtuitkomst blokkeren met status `BLOCKED`.

### 5.3 Blokkeergedrag

Een rekenende implementatie MOET blokkeren en melden wanneer invoer buiten de gevalideerde tabelenvelop valt. Zij MOET NIET stilzwijgend extrapoleren.

Blokkeren betekent in OSOD:

- geen geldige DCIEM-luchtuitkomst presenteren;
- geen groene status tonen;
- de reden vastleggen in `calculation.blockingReasons`;
- de feitelijke registratie niet verwijderen of afvlakken.

### 5.4 Statusmodel

OSOD onderscheidt minimaal:

- `NOT_CALCULATED`: er is geen berekening uitgevoerd;
- `GREEN`: berekening binnen envelop, geen blokkerende melding;
- `AMBER`: niet-blokkerende waarschuwing of aandachtspunt;
- `RED`: blokkerende uitkomst of buiten envelop;
- `BLOCKED`: berekening is niet uitgevoerd of afgeschermd wegens ongeldige invoer, bronconflict of integriteitsprobleem.

Een implementatie MAG lokale statuslabels gebruiken, maar MOET naar deze OSOD-statussen kunnen mappen.

### 5.5 Bronconflicten

Waar bronnen numeriek afwijken, hanteert OSOD v0.1 IWOD/DCIEM als prevalerende rekenbron voor Niveau R. Afwijkingen worden neutraal als bronconflict gedocumenteerd. Een publiek OSOD-document noemt bronconflicten zakelijk en vermijdt onnodige kwalificaties zoals “fout” zonder formeel erratum.

### 5.6 Opstijgingslaag

Het DMC-advies van 10 december 2013 is normatief bruikbaar voor de expliciet genoemde categorieën 6 m en 9 m. Dit omvat de limieten per duikperiode én de limieten per duikdag voor die categorieën. Diepere categorieën worden in v0.1 als informatief of projectmatig gemarkeerd totdat een formele bron of governancebesluit ze normatief maakt.

### 5.7 Meterregels

De meterregels voor 6, 9 en 12 m uit de Landelijke Werkinstructie WOD v2.0, paragraaf 11.5.1, zijn normatief voor Niveau R. De meterregelgrens geldt voor de opgetelde duiktijd van de duiken van dezelfde duiker en is geen tabelgrens voor één afzonderlijke duik. Een rekenende implementatie MOET een overschrijding blokkeren met `BOTTOM_TIME_EXCEEDS_METER_RULE`. De grenswaarden, toepasbaarheidsvoorwaarden en het onderscheid met de tabelenvelop staan in `rekencontract/README.md`.

### 5.8 HG-aanpassing

De HG-aanpassing bij verscheidene herhalingsduiken uit de Landelijke Werkinstructie WOD v2.0, paragraaf 11.5.1, is normatief voor Niveau R: bij een oppervlakte-interval van 6 uur of korter en een nieuwe herhalingsgroep die lager dan of gelijk aan de voorgaande is, geldt de voorgaande herhalingsgroep plus één letter. De beslisregels en het bronvoorbeeld staan in `rekencontract/README.md`.

### 5.9 Kort oppervlakte-interval en gecombineerde duik

Een gecombineerde duik is een samenstel van twee of drie duiken waartussen het herhalingsinterval (HI, oppervlakte-interval) korter is dan 15 minuten, of een herhalingsduik waarvan de herhalingsfactor (HF) groter is dan 2.0. Bij een oppervlakte-interval korter dan 15 minuten MOET een rekenende implementatie tabel 4a NIET toepassen en MOET zij de herhalingsgroep die uit alleen de eigen duiktijd van die duik volgt NIET als ketenwaarde voor een volgende herhalingsduik gebruiken. Conform zijn twee invullingen: (1) het samenstel als gecombineerde duik berekenen voor zover de bron de methode dekt, of (2) het vervolgresultaat markeren als niet-berekend of geblokkeerd met vastgelegde reden: `EMPTY_TABLE_CELL`, status `BLOCKED`. De bronnen, de bronvaste samenstel-elementen en het open bronpunt staan in `rekencontract/README.md`. Een implementatie MAG bij deze blokkade informatief het eerstvolgende geldige tijdstip tonen, exact volgens het tabeldomein van tabel 4a, en MAG de bestaande vervolgroutes als handelingsopties presenteren; die presentatie wijzigt de berekeningsstatus niet. De uitwerking en de bronvoorbeelden staan in `rekencontract/README.md`, sectie 10.

## 6. Pijler 3: Conformiteit

Een implementatie MOET haar conformiteitsclaim expliciet maken:

```text
OSOD v0.1 Niveau S
OSOD v0.1 Niveau S+R
```

Conformiteit bestaat uit:

- schema-validatie;
- bron- en versievermelding;
- rekenbronfingerprint;
- publieke basistoetsen;
- aanvullende besloten of beheerderstoetsen waar volledige tabellen niet publiek worden hergepubliceerd.

## 7. Pijler 4: Beheer en governance

De standaard heeft beheer nodig dat losstaat van één applicatie. In v0.1 ligt tijdelijk beheer bij de initiatiefnemer. Het beoogde eindbeeld is beheer onder een organisatieaccount en later een breder gremium.

Veiligheidswijzigingen zijn wijzigingen aan:

- rekencontract;
- tabelenvelop;
- bronkeuze of fingerprint;
- blokkeergedrag;
- statussemantiek;
- velden die wettelijke of operationele registratie raken.

Zulke wijzigingen MOETEN expliciet worden gemarkeerd en gereviewd.


## 8. Externe systemen en mapping

OSOD kan worden gebruikt als neutraal uitwisselformaat tussen een invoerlaag en een extern registratiesysteem. Een koppeling naar een extern systeem kan via een importbestand, API of veldmapping verlopen.

De normatieve volgorde blijft:

```text
invoerlaag of applicatie
→ OSOD-record
→ eventuele mapping naar extern systeem
```

Een extern systeem, importformaat of API is niet normatief voor OSOD. Een systeemgerichte mapping MAG OSOD-velden niet herdefiniëren of feitelijke waarden afvlakken.

## 9. Relatie met Duikmonitor

Duikmonitor is de eerste beoogde referentie-implementatie. Dat betekent:

- Duikmonitor ZOU OSOD-records moeten kunnen exporteren;
- Duikmonitor ZOU OSOD-schema-validatie moeten kunnen uitvoeren;
- Duikmonitor ZOU OSOD-toetsvectoren moeten kunnen uitvoeren;
- Duikmonitor MAG niet als normbron voor OSOD worden behandeld.

Voor Duikmonitor hanteert OSOD v0.1 twee sporen:

```text
Spoor 1: OSOD als normatieve standaard.
Spoor 2: Duikmonitor als informatief implementatiepad richting OSOD-conformiteit.
```

Documenten in `implementatie/` kunnen beschrijven hoe Duikmonitor-interne velden naar OSOD worden gemapt en hoe Duikmonitor de rekenkern, OSOD-mapping/export en conformiteitscontrole kan scheiden. Die documenten zijn informatief, niet normatief. Andere implementaties mogen een andere technische indeling gebruiken zolang zij aantoonbaar aan OSOD voldoen.

## 10. Open punten voor latere versies

1. Definitieve juridische licentiekeuze bevestigen vóór formele brede publicatie.
2. Governance overdragen van initiatiefnemer naar organisatie/gremium.
3. Volledige machineleesbare toetsset vaststellen, inclusief publicatierechten voor tabelwaarden.
4. Definitieve bewaartermijnen en AVG-/Archiefwetgrondslag voor operationele records uitwerken.
5. Formuliertracering naar het Brandweer Duiklogboek is gelegd voor de operationele logbladen van Deel 2 en Deel 3 (zie `schema/README.md`); resterend zijn de samenvoegvoorwaarde voor onderbroken duikarbeid en de beoordeling van een duikeraftekenveld.
6. Definitief beleid bepalen voor lokale/regiospecifieke velden en extensies.
