# Changelog

## 0.1: publieke conceptversie, aanscherping juni 2026

Aanscherping vóór eerste publicatie. De versie blijft 0.1.

**VEILIGHEIDSWIJZIGING: statussemantiek eenduidig gemaakt.** De ongedefinieerde statusterm `NOT_APPLICABLE` stond in het rekencontract (sectie 2) en een gelijkwaardige formulering in `STANDAARD.md` (sectie 5.2), terwijl het statusmodel en toetsvector R-BASIS-005 `BLOCKED` voorschrijven. Beide passages verwijzen nu eenduidig naar blokkeren met status `BLOCKED`. Het statusmodel zelf is ongewijzigd.

**VEILIGHEIDSWIJZIGING: meterregels normatief opgenomen.** Governancebesluit van de beheerder: de meterregels voor 6, 9 en 12 m uit de Landelijke Werkinstructie WOD v2.0, paragraaf 11.5.1, zijn normatief onderdeel van het rekencontract (Niveau R). De blokkeercode `BOTTOM_TIME_EXCEEDS_METER_RULE` is gedefinieerd in de normatieve codecatalogus. Het rekencontract omvat hiermee naast enkelduikberekening ook cumulatieve duiktijdbewaking. Zie `rekencontract/README.md`, sectie 4, en `STANDAARD.md`, sectie 5.7.

**VEILIGHEIDSWIJZIGING: toetsvector R-BASIS-006 gesplitst en bronverankerd.** De eerdere vector vermengde de meterregelgrens (420 minuten, cumulatief) met de tabelenvelop (720 minuten, hoogste tabelcel op 6 m). De set bevat nu afzonderlijke meterregelvectoren (R-BASIS-006, R-BASIS-007, R-BASIS-008) en envelopvectoren (R-BASIS-009, R-BASIS-010), elk met bronvermelding.

**VEILIGHEIDSWIJZIGING: HG-aanpassing normatief opgenomen.** Governancebesluit van de beheerder: de HG-aanpassing bij verscheidene herhalingsduiken (cirkeldoorbreking) uit de Landelijke Werkinstructie WOD v2.0, paragraaf 11.5.1, is normatief onderdeel van het rekencontract (Niveau R), inclusief het gebruik van de aangepaste HG voor de HF-bepaling. Nieuwe toetsvector `R-BASIS-011` legt het bronvoorbeeld vast (voorgaand D, HI korter dan 6 uur, uitgevoerd B, aangepast E). Zie `rekencontract/README.md`, sectie 5, en `STANDAARD.md`, sectie 5.8.

**VEILIGHEIDSWIJZIGING: daglimieten opstijgingslaag toegevoegd.** Het DMC-advies van 10 december 2013 bevat naast de limieten per 4 uur ook daglimieten: maximaal 12 opstijgingen per duikdag bij 6 m en maximaal 8 bij 9 m. Deze ontbraken in het rekencontract en zijn toegevoegd aan de normatieve opstijgingslaag voor de brongedekte categorieën. Zie `rekencontract/README.md`, sectie 9.

**VEILIGHEIDSWIJZIGING: calculation-invarianten machinaal afgedwongen.** De kruisveldregels voor `status`, `resultValid` en `blockingReasons` stonden alleen in proza en worden nu afgedwongen in `schema/osod-logbook-record.schema.json`. Zie `schema/README.md`, sectie 8.1. Twee nieuwe ongeldige voorbeelden bewijzen de afdwinging; één nieuw geldig randgeval toont een correct geblokkeerde meterregeloverschrijding.

Overig:

- Redactioneel: het subkopje met de waarden van `dive.duiksysteem` in `schema/README.md` genummerd als 6.1, conform de bestaande subkopnummering. Geen inhoudelijke wijziging; de standaardversie blijft 0.1.
- Schemawijziging (besluit B-0018): de losse duiksysteemwaarde `OLV` vervalt uit `dive.duiksysteem`; de toegestane waarden zijn `SCUBA`, `SCUBA_OLV`, `SSE`, `anders` en `onbekend`, elk voorzien van een bronherleide definitie in `schema/README.md`, sectie 6. Een registratie die elders de kale aanduiding OLV gebruikt, wordt in OSOD vastgelegd als `SCUBA_OLV`. De standaardversie blijft 0.1.
- Voorbeeldcorrectie: in `voorbeelden/geldig/meterregel-geblokkeerd.json` stond een rekenmelding in `dive.bijzondereSessies`; dat veld registreert sessievormen. Het veld is nu leeg; de melding staat in `calculation.messages`.
- Typografie geharmoniseerd: em-dashes in koppen en opsommingen vervangen door dubbele punt of komma. Geen inhoudelijke wijziging.
- Bronverantwoording uitgebreid met verificatiestatus van de rekenbrongrenzen en bijbehorende open punten.
- `voorbeelden/README.md` gecorrigeerd: padverwijzingen wijzen nu naar de werkelijke mappen `geldig/` en `ongeldig/`.
- Integriteitsketen platformvast gemaakt: `.gitattributes` dwingt LF-regeleinden af, en de paden in `CHECKSUMS.sha256` en `MANIFEST.json` zijn repo-relatief gemaakt zodat de controle rechtstreeks vanuit de hoofdmap van de repository werkt. Inhoudelijk ongewijzigd.
- Naamsontvouwing gecorrigeerd: OSOD staat voluit voor Open Standaard Operationele Duikregistratie, zonder organisatienaam in de naam; de gerichtheid op de Nederlandse brandweer staat als omschrijving. Dit voorkomt de onbedoelde suggestie van een uitgave van Brandweer Nederland en sluit aan op de onafhankelijkheidsverklaring.
- Attributie en onafhankelijkheid vastgelegd: naamsvermelding onder CC-BY-4.0 geschiedt aan het project OSOD-NL, zonder persoonsnaam; in NOTICE en README is verklaard dat OSOD een onafhankelijk initiatief is en geen uitgave van een veiligheidsregio, werkgever of bronorganisatie.
- Governancebesluit bronregister: `bronnen/bron-fingerprints.json` is uit het publieke pakket verwijderd. `BRONNEN.md` is het bronregister; bronnen worden geïdentificeerd via officiële aanduidingen, waaronder Staatscourant 2024, 39678, 39683, 39685 en 39687, en niet-openbare bronnen bij titel en houder. De interne bronadministratie met documentfingerprints berust bij de beheerder. Het integriteitsmechanisme van de rekenbron in het rekencontract (rekenbronfingerprint, `SOURCE_FINGERPRINT_MISMATCH`) is ongewijzigd.
- Brongetrouwheidscorrectie in normatieve tekst, geen functionele wijziging: de meterregelbullet over het herhalingsinterval is gelijkgetrokken met de bronformulering van Werkinstructie WOD par. 11.5.1. De eerdere formulering koppelde het herhalingsinterval ten onrechte aan "na het bereiken van het maximum"; de bron koppelt de HI-eis aan de regel als geheel. Blokkeergedrag, codes en grenswaarden zijn ongewijzigd.
- Bronconflict Tabel 4a (HG B, OI 2:00-2:59): IWOD 002, het Duiklogboek en het DCIEM-origineel geven 1,2; de Werkinstructie WOD v2.0 geeft 1,1. OSOD volgt de rekenbron en hanteert 1,2; de WOD-waarde is neutraal als bronconflict vastgelegd.
- Bronbevinding vastgelegd: de in Werkinstructie WOD par. 11.5.1 aangekondigde tabel met aantallen afdalingen en opstijgingen per dagdeel ontbreekt in de uitgave; de aantallen volgen uit het DMC-advies als primaire bron.
- DCIEM geherpositioneerd als herkomstbron (informatief), geen normbron; Tabel 4b is tegen het 1992-origineel bevestigd. Celverificatie van Airtabel 1 en Tabel 4a tegen het origineel MOET als beheerderverplichting zijn uitgevoerd vóór formele vaststelling van de standaard, met prioriteit voor de bronconflictcel.
- Bronlijn defensie vastgelegd: IWOD per 1 januari 2023 binnen Defensie vervangen door de Instructie Duikarbeid Defensie (Kamerstukken 35925-X nr. 93 en 36360-X nr. 1); de brandweerketen verwijst nog naar IWOD 1 april 2019 en OSOD volgt de brandweerketen, met wakend open punt.
- Formuliertracering naar het Brandweer Duiklogboek toegevoegd voor de operationele logbladen van Deel 2 (Duiklog Duiker) en Deel 3 (Duiklog Duikploegleider), op veldniveau en zonder reproductie van de beschermde uitgave. De uitgave (geheel herziene uitgave januari 2019) hanteert tabelopbouw conform IWOD, 1 april 2019. Editiegegevens vastgelegd in de bronlijst; bijbehorende open punten bijgewerkt.

## 0.1: publieke conceptversie

Deze versie is gebaseerd op de interne OSOD v0.1 en aangescherpt tot eerste publieke conceptversie. De versie blijft bewust 0.1.

Belangrijkste aanpassingen:

- Bronhiërarchie expliciet gemaakt.
- Duikmonitor gepositioneerd als beoogde eerste referentie-implementatie, niet als normbron.
- Registratie en berekening gescheiden.
- Werkelijke maximale diepte gescheiden van tabeldiepte.
- DCIEM Niveau R expliciet beperkt tot ademlucht.
- Volledige DCIEM/IWOD-tabellen uit publieke conformiteitsdocumentatie verwijderd.
- Beperkte publieke basistoetsen toegevoegd.
- Bronconflicten neutraler geformuleerd.
- Opstijgingslaag gescheiden naar bronhardheid.
- Ruimte beschreven voor externe systeemkoppelingen via OSOD-records en mapping.
- Informatieve Duikmonitor-architectuurschets toegevoegd: DCIEM-rekenkern, OSOD-mapping/export en conformiteitscontrole.
- Governance-, bijdrage-, veiligheids-, bronnen- en licentiedocumenten toegevoegd.
- Eerste JSON Schema en voorbeeldrecords toegevoegd.

## 0.1: interne werkversie

- Eerste standaardtekst met vier pijlers.
- Eerste schema- en conformiteitsuitwerking.
- Eerste normatieve eis voor blokkeren buiten de gevalideerde DCIEM-envelop.
