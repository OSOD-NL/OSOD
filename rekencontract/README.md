# Rekencontract DCIEM/IWOD

Onderdeel van OSOD v0.1. Dit document beschrijft het rekencontract voor Niveau R. De volledige numerieke DCIEM/IWOD-tabellen worden in deze publieke conceptversie niet hergepubliceerd.

Status: publieke conceptversie. Nog niet vastgesteld. Niet bedoeld voor direct operationeel gebruik zonder eigen verificatie en besluitvorming.

## 1. Rekenbron en publicatiebeleid

De rekenkundige bron voor Niveau R is de IWOD/DCIEM-luchttabel zoals vastgelegd in het bronregister (`BRONNEN.md`).

Deze publieke v0.1 herpubliceert geen volledige DCIEM/IWOD-tabellen. Redenen:

1. OSOD moet een open standaard zijn, geen ongeautoriseerde verveelvoudiging van bronuitgaven.
2. Tabelwaarden zijn veiligheidskritisch en moeten via de rekenbronfingerprint van de implementatie en conformiteitstoetsing worden beheerd.
3. Herpublicatierechten en beheerafspraken moeten vóór volledige open distributie expliciet worden vastgesteld.

De beperkte publieke basistoetsen staan in `conformiteit/toetsvectoren/basistoetsen-v0.1.json`.

## 2. Ademgasafbakening

Niveau R geldt uitsluitend voor `ademlucht` binnen de DCIEM-luchttabel.

Een implementatie die rekent, MOET blokkeren met status `BLOCKED` wanneer:

- ademgas `nitrox` is;
- ademgas `heliox` is;
- ademgas `zuurstof` is;
- ademgas `anders` of `onbekend` is;
- het ademgas ontbreekt.

Een implementatie MAG deze duik wel registreren op Niveau S.

## 3. Envelop

Een rekenende implementatie MOET binnen de gevalideerde envelop blijven. Buiten de envelop MOET zij blokkeren en melden.

Voor OSOD v0.1 omvat de envelop ten minste:

- DCIEM-luchttabelwaarden voor de vastgelegde tabeldieptes;
- brandweer-no-deco-scope tot en met 15 m;
- herhalingsduikregels, oppervlakte-interval en herhalingsfactoren;
- lege of uitgesloten tabelcellen als niet-toegestaan;
- meterregels voor 6, 9 en 12 m conform sectie 4;
- HG-aanpassing bij herhalingsduiken conform sectie 5;
- DMC-opstijgingsadvies voor de expliciet genoemde categorieën 6 m en 9 m.

## 4. Meterregels (6, 9 en 12 m)

De meterregels uit de Landelijke Werkinstructie WOD v2.0 (03-12-2024), paragraaf 11.5.1, zijn normatief voor Niveau R.

| Meterregel | Maximale opgetelde duiktijd |
|---|---|
| 6 meter-regel | 420 minuten |
| 9 meter-regel | 210 minuten |
| 12 meter-regel | 120 minuten |

Toepasbaarheid, conform de bron:

- de regel geldt wanneer uitsluitend wordt gedoken tot en met de betreffende diepte;
- de afzonderlijke duiken van dezelfde duiker gelden samen als één duik en de duiktijden worden opgeteld; met de totale duiktijd wordt in airtabel 1 de herhalingsgroep bepaald;
- de regel is alleen van toepassing wanneer de herhalingsfactor (HF) bij aanvang van de eerste duik 1.0 bedraagt;
- men houdt een herhalingsinterval (HI) aan tot de HF weer 1.0 is.

De meterregelgrens is **cumulatief**. Zij is geen tabelgrens voor één afzonderlijke duik. De hoogste tabelwaarde van airtabel 1 op tabeldiepte 6 m is 720 minuten (herhalingsgroep M); overschrijding van die tabelgrens valt onder `BOTTOM_TIME_EXCEEDS_TABLE`, niet onder de meterregel.

Een rekenende implementatie MOET een overschrijding van de opgetelde duiktijd boven het meterregelmaximum blokkeren met `BOTTOM_TIME_EXCEEDS_METER_RULE` en status `RED` of `BLOCKED`. De feitelijke registratie blijft mogelijk.

De overige bepalingen van paragraaf 11.5.1, waaronder de terugvalregels tussen meterregels, het maximum van 3 duiken bij duiken dieper dan 12 m en de procedure bij niet-geplande diepteoverschrijding, zijn in v0.1 nog niet normatief opgenomen. Zie open punten.

## 5. HG-aanpassing bij herhalingsduiken

Bij verscheidene herhalingsduiken kan de herhalingsgroep (HG) blijven cirkelen op dezelfde of een lagere letter, waardoor de restbelasting wordt onderschat. De HG-aanpassing uit de Landelijke Werkinstructie WOD v2.0, paragraaf 11.5.1, en het Brandweer Duiklogboek Deel 1 (p. 32) doorbreekt dat en is normatief voor Niveau R.

Een rekenende implementatie MOET na iedere herhalingsduik de geldende HG als volgt bepalen, met HI als het oppervlakte-interval sinds de voorgaande duik en HG-nieuw als de met airtabel 1 bepaalde herhalingsgroep van de uitgevoerde duik:

- HI langer dan 6 uur: geen aanpassing; HG-nieuw geldt.
- HI van 6 uur of korter en HG-nieuw hoger dan de voorgaande HG: geen aanpassing; HG-nieuw geldt.
- HI van 6 uur of korter en HG-nieuw lager dan of gelijk aan de voorgaande HG: de geldende HG MOET worden gesteld op de voorgaande HG plus één letter.

De geldende HG na aanpassing MOET worden gebruikt voor alle vervolgbepalingen, waaronder de HF-bepaling via Tabel 4a. Bronvoorbeeld: voorgaande duik HG D, HI korter dan 6 uur, uitgevoerde duik HG B; de geldende HG wordt E. Toetsvector `R-BASIS-011` legt dit vast.

## 6. Blokkeergedrag

Een implementatie die Niveau R claimt, MOET blokkeren bij onder meer:

| Code | Situatie | Verwachte status |
|---|---|---|
| `OUTSIDE_DEPTH_ENVELOPE` | Werkelijke diepte buiten de DCIEM-/brandweer-envelop voor de berekening. | `RED` of `BLOCKED` |
| `UNSUPPORTED_BREATHING_GAS` | Ademgas is niet `ademlucht`. | `BLOCKED` |
| `EMPTY_TABLE_CELL` | Benodigde tabelcel is leeg of uitgesloten, waaronder een herhalingsfactor boven 2.0 (lege cel in tabel 4a) en een oppervlakte-interval onder het tabel 4a-domein (korter dan 15 minuten). | `BLOCKED` |
| `BOTTOM_TIME_EXCEEDS_TABLE` | Bodemtijd valt boven hoogste toegestane tabelwaarde. | `RED` of `BLOCKED` |
| `BOTTOM_TIME_EXCEEDS_METER_RULE` | Opgetelde duiktijd onder een meterregel overschrijdt het brongedekte maximum (par. 11.5.1 Werkinstructie WOD). | `RED` of `BLOCKED` |
| `SOURCE_FINGERPRINT_MISMATCH` | Rekenbron komt niet overeen met de verwachte fingerprint. | `BLOCKED` |
| `INVALID_INPUT` | Ontbrekende of onmogelijke invoer. | `BLOCKED` |

Een blokkerende uitkomst MAG niet als waarschuwing of groen resultaat worden gepresenteerd.

## 7. Registratie blijft mogelijk

Blokkeren gaat over de geldigheid van een rekenuitkomst. Blokkeren mag niet betekenen dat een feitelijke duik niet kan worden geregistreerd. Een OSOD-record moet afwijkingen, overschrijdingen en incidenten juist zichtbaar kunnen vastleggen.

## 8. Bronconflicten

Waar bronnen numeriek afwijken, hanteert OSOD v0.1 IWOD/DCIEM als prevalerende rekenbron voor Niveau R. Afwijkingen worden als bronconflict vastgelegd.

Gedocumenteerd bronconflict, Tabel 4a, herhalingsgroep B, oppervlakte-interval 2:00-2:59:

- De Landelijke Werkinstructie WOD v2.0 (2024) vermeldt voor deze cel `1.1`.
- IWOD 002 (1 april 2019) en de DCIEM-bijlage van het Brandweer Duiklogboek vermelden `1.2`.
- Het kolompatroon ondersteunt `1.2`: de reeks loopt per herhalingsgroep monotoon op (A `1.1`, B `1.2`, C `1.2`, D `1.3`); een waarde `1.1` voor B zou die reeks onderbreken.
- De werkinstructie verwijst zelf naar de DCIEM-tabellen als bron; een bewuste afwijking van één cel zonder vermelding ligt niet voor de hand.

OSOD v0.1 hanteert daarom `1.2` in de toetsvectoren (R-BASIS-002). De WOD-waarde `1.1` wordt als bronconflict vastgelegd; OSOD volgt de prevalerende rekenbron IWOD 002.

Publieke tekst gebruikt hiervoor de term **bronconflict** totdat een formeel erratum of beheerbesluit anders bepaalt.

## 9. Opstijgingslaag

Het DMC-advies van 10 december 2013 noemt expliciet:

- maximaal 6 opstijgingen binnen 4 uur bij een maximale diepte van 6 m;
- maximaal 4 opstijgingen binnen 4 uur bij een maximale diepte van 9 m;
- maximaal 12 opstijgingen per duikdag bij een maximale diepte van 6 m;
- maximaal 8 opstijgingen per duikdag bij een maximale diepte van 9 m;
- minimaal 15 minuten oppervlakte-interval tussen twee opstijgingen;
- minimaal 1 uur rust na een duikperiode van 4 uur.

In OSOD v0.1 zijn alleen de expliciet bron-gedekte 6 m- en 9 m-categorieën voorlopig normatief, inclusief de daglimieten. Diepere categorieën blijven informatief of projectmatig totdat het beheer daarover beslist.

Bronbevinding: de Landelijke Werkinstructie WOD v2.0 kondigt onder paragraaf 11.5.1 een tabel aan met maximale aantallen afdalingen en opstijgingen per dagdeel, maar die tabel ontbreekt in de uitgave. De aantallen volgen daarom uit het DMC-advies als primaire bron.

## 10. Kort oppervlakte-interval en gecombineerde duik

Definitie en bron: Een gecombineerde duik is een samenstel van twee of drie duiken waartussen het herhalingsinterval (HI, oppervlakte-interval) korter is dan 15 minuten, of een herhalingsduik waarvan de herhalingsfactor (HF) groter is dan 2.0. De meterregels van sectie 4 zijn hierop de uitzondering. Bron: Landelijke Werkinstructie WOD v2.0 (03-12-2024), begrippenlijst direct onder paragraaf 11.5; de procedures en de meterregel-uitzondering staan in paragraaf 11.5.1.

Tabeldomein: Tabel 4a definieert herhalingsfactoren voor herhalingsintervallen van 15 minuten tot en met 18 uur. Onder de 15 minuten bestaat geen tabelwaarde. Bron: Werkinstructie WOD v2.0, paragraaf 11.5.3; IWOD 002 (1 april 2019), paragraaf 2300 (tabelbron).

Verbodsnorm: Bij een oppervlakte-interval korter dan 15 minuten MOET een rekenende implementatie tabel 4a NIET toepassen en MOET zij de herhalingsgroep die uit alleen de eigen duiktijd van die duik volgt NIET als ketenwaarde voor een volgende herhalingsduik gebruiken.

De twee conforme invullingen: (1) het samenstel als gecombineerde duik berekenen voor zover de bron de methode dekt, of (2) het vervolgresultaat markeren als niet-berekend of geblokkeerd met vastgelegde reden: `EMPTY_TABLE_CELL`, status `BLOCKED`. Een implementatie MAG daarbij om handmatige beoordeling vragen; dat is een geldige invulling van (2), geen norm.

Bronvaste samenstel-elementen: De effectieve duiktijd (EDT) van een gecombineerde duik is de som van de effectieve duiktijd van de eerste duik en de duiktijd van de tweede en eventueel de derde duik. Is de eerste duik zelf een herhalingsduik, dan telt zijn effectieve duiktijd (duiktijd maal herhalingsfactor). Een samenstel omvat maximaal drie duiken. Bron: dezelfde begrippenlijst.

Noot, open bronpunt: de dieptekeuze voor het samenstel, de tabelafhandeling met de gesommeerde EDT boven de meterregels en de instap wanneer de trigger HF groter dan 2,0 is, zijn niet bronvast aangetroffen. Invulling (1) is daarom pas volledig specificeerbaar wanneer de ontbrekende bron beschikbaar is; tot die tijd is invulling (2) de enige volledig gespecificeerde route. Zie het open punt in sectie 11.

## 11. Open punten

1. Volledige machineleesbare toetsset vaststellen.
2. Publicatierechten voor volledige tabelwaarden vaststellen.
3. Definitief besluit nemen over normatieve status van opstijgingsregels dieper dan 9 m.
4. Definitieve fout-/statuscodecatalogus uitbreiden.
5. Normatieve status bepalen van de overige bepalingen van Werkinstructie WOD paragraaf 11.5.1: terugvalregels tussen meterregels, maximum van 3 duiken bij duiken dieper dan 12 m en de procedure bij niet-geplande diepteoverschrijding.
6. Onderzoeksvraag gecombineerde duik: bevat de IWOD 002-proza zelf een gecombineerde-duik-procedure voor dit bereik. Zolang dat niet is vastgesteld blijven de dieptekeuze voor het samenstel, de tabelafhandeling met de gesommeerde EDT boven de meterregels en de instap bij HF groter dan 2.0 open; een latere restauratie van een rekenprocedure neemt de voorwaarden en uitsluitingen van de dan beslissende bron integraal mee.
