# Bronnen en bronpositie

Dit document is het bronregister van OSOD v0.1. Bronnen worden geïdentificeerd via hun officiële aanduiding: titel, uitgever of houder, datum of versie en waar beschikbaar het officiële publicatienummer. Niet-openbare bronnen worden bij titel en houder genoemd. De brondocumenten zelf worden niet in deze publieke repository hergepubliceerd en de repository bevat geen fingerprints van brondocumenten van derden; de interne bronadministratie berust bij de beheerder.

## Hoofdregel

OSOD is geen kopie van bestaande formulieren, tabellen of applicaties. OSOD definieert een eigen open gegevensmodel, rekencontract en conformiteitskader, met traceerbaarheid naar wettelijke, normatieve en operationele bronnen.

## Bronhiërarchie

1. Arbobesluit, Arbeidsomstandighedenregeling en daarop gebaseerde wettelijke publicaties.
2. Staatscourant 2024-registratieschema's voor duiker, duikploegleider, duikmedisch begeleider en duikerarts.
3. Werkinstructie Duikarbeid van de Nederlandse Arbeidsinspectie als inspectie- en minimumcontrolekader.
4. Landelijke Werkinstructie Werken onder overdruk brandweer als brandweer-operationeel kader.
5. IWOD/DCIEM-luchttabel als rekenkundige bron voor Niveau R.
6. IFV/NIPV-logboek, A0-lijst en brancherichtlijn als formulier-, praktijk- en vakbekwaamheidsbronnen.
7. Implementaties, waaronder Duikmonitor, als toetsbare uitvoeringen van de standaard.

## Publicatiebeleid

Volledige bronuitgaven en volledige DCIEM/IWOD-tabellen worden in deze publieke conceptversie niet integraal hergepubliceerd. Beperkte toetsvectoren kunnen worden opgenomen om gedrag van implementaties te toetsen zonder de volledige bron te verspreiden.

## Bronlijst

| Bron | Gebruik in OSOD |
|---|---|
| Staatsblad 2024, nr. 332 | wettelijke context persoonsregistratie en logboekplicht |
| Registratieschema duiker (Staatscourant 2024, 39678) | minimumvelden duikerlogboek en scopes |
| Registratieschema duikploegleider (Staatscourant 2024, 39683) | minimumvelden duikploegleiderlogboek en scopes |
| Registratieschema duikmedisch begeleider (Staatscourant 2024, 39685) | rol- en registratiecontext |
| Registratieschema duikerarts (Staatscourant 2024, 39687) | medisch attest en duikerartscontext |
| SWOD Toetsplan (CES 400), versie 5.0 (november 2024) | examen-, registratie- en portfoliocontext |
| NIPV Handleiding Register duikarbeid brandweer en politie, versie 3.1 (29 april 2026) | registerproces en uploadprocedure portfolio |
| Werkinstructie Duikarbeid, Nederlandse Arbeidsinspectie (11 december 2025) | inspectieanker en minimumcontrolepunten |
| Landelijke Werkinstructie WOD brandweer, v2.0 (3 december 2024) | operationeel brandweerkader, duikrapport, A0-lijst, DCIEM-bijlage en meterregels (par. 11.5.1) |
| WOD-SOE, SWOD Arbocatalogus Werken onder Overdruk (CAT 002.5, 2024) | systeem- en onderhoudseisen, materieelbegrippen |
| Brancherichtlijn blijvende vakbekwaamheid brandweerduiker en duikploegleider (maart 2026) | oefencoderingen en vakbekwaamheidscontext |
| Brandweer Duiklogboek (IFV, geheel herziene uitgave januari 2019, ISBN 978-90-5643-467-0; drukken: Deel 1 5e druk 2e oplage, Deel 2 4e druk 1e oplage, Deel 3 4e druk 1e oplage) | formulier- en veldtracering operationele logbladen; rechtenvoorbehoud, geen reproductie |
| A0-lijst, intakeformulier duikongeval (ADivP-02, Editie C, versie 2) | ongeval- en triageveldenreferentie |
| ADivP-02 Annex B | NATO casualty signal en incident medical record, referentie |
| IWOD 002 Decompressiemethoden (1 april 2019), par. 2300 luchttabellen | rekenkundige bron voor Niveau R |
| DCIEM Diving Manual: Air Decompression Procedures and Tables (DR 52-19-52, 1992) | herkomstbron en terminologie van de luchttabellen, informatief |
| DMC-advies aantal opstijgingen, 10 december 2013 (niet openbaar; berust bij het Duikmedisch Centrum) | opstijgingsadvies voor 6 m en 9 m |
| Duikmonitor v1.1.0 | beoogde eerste referentie-implementatie en mogelijke toetsgevallen |
| Kamerstukken 35925-X nr. 93 en 36360-X nr. 1 | bronlijn defensie-instructies: IWOD per 1 januari 2023 binnen Defensie vervangen door de Instructie Duikarbeid Defensie |

## Verificatiestatus rekenbron

De grenswaarden in rekencontract en toetsvectoren zijn als volgt geverifieerd:

- De meterregelmaxima 420, 210 en 120 minuten en de toepasbaarheidsvoorwaarden zijn overgenomen uit de Landelijke Werkinstructie WOD v2.0 (03-12-2024), paragraaf 11.5.1.
- De tabelgrens van 720 minuten op tabeldiepte 6 m (herhalingsgroep M) is op celniveau geverifieerd in het IWOD 002-extract van airtabel 1.
- Het tabel 4a-domein (15 minuten tot en met 18 uur) is geverifieerd in de Werkinstructie WOD v2.0, paragraaf 11.5.3, en in het IWOD 002-tabelextract; de definitie van de gecombineerde duik staat in de begrippenlijst direct onder paragraaf 11.5, de procedures en de meterregel-uitzondering in paragraaf 11.5.1.
- Celverificatie van Airtabel 1 en Tabel 4a tegen het DCIEM 1992-origineel (DR 52-19-52) is een openstaande beheerderverplichting; zie open punten.
- De herhalingsnultijden van Tabel 4b zijn op celniveau bevestigd tegen het DCIEM-origineel (rapport 86-R-35, uitgave maart 1992, metrische versie); dit is gedocumenteerd in de tabelvalidatie van de beoogde referentie-implementatie (28 mei 2026). Airtabel 1 en Tabel 4a zijn op celniveau bevestigd tegen het IWOD 002-extract en de DCIEM-bijlage van het Duiklogboek. Celverificatie van Airtabel 1 en Tabel 4a tegen het 1992-origineel MOET door de beheerder zijn uitgevoerd vóór formele vaststelling van de standaard, met prioriteit voor de bronconflictcel (Tabel 4a, HG B, OI 2:00-2:59). Dit is een beheerderverplichting en geen conformiteitseis voor implementaties: DCIEM is herkomstbron, IWOD 002 is de tabelbron.
- Bronconflict Tabel 4a (HG B, OI 2:00-2:59): de Werkinstructie WOD v2.0 vermeldt 1,1; IWOD 002 en het Duiklogboek vermelden 1,2. OSOD volgt 1,2; onderbouwing in het rekencontract.
- Bronlijn defensie: binnen Defensie is de IWOD per 1 januari 2023 vervangen door de Instructie Duikarbeid Defensie, vastgesteld door de Militaire Duikautoriteit (Kamerstukken 35925-X nr. 93 en 36360-X nr. 1). De brandweerketen (Werkinstructie WOD v2.0, Duiklogboek) verwijst onverminderd naar IWOD 1 april 2019; OSOD volgt de brandweerketen.
- Het Brandweer Duiklogboek (geheel herziene uitgave januari 2019) hanteert als tabelbasis IWOD, 1 april 2019: zo vermeld in de DCIEM-bijlage van Deel 1 en in de invul-instructie van Deel 2. Dit bevestigt de rekenbron waarop OSOD pint. De voetnoot in de aangetroffen druk van Deel 3 die naar VKM januari 2014 verwijst, is een verouderd redactioneel restant en geen afwijkende tabelbasis.

## Open punten

- Formele herpublicatierechten voor volledige DCIEM/IWOD-tabelwaarden.
- Rechte heropname van twee persoonsgegevenspagina's uit Duiklogboek Deel 1 (velden nu tentatief vastgelegd).
- Definitieve bewaartermijnen en AVG-/Archiefwetgrondslag voor operationele records.
- Celverificatie van Airtabel 1 en Tabel 4a tegen het DCIEM 1992-origineel (DR 52-19-52) MOET zijn uitgevoerd vóór formele vaststelling van de standaard, met prioriteit voor de bronconflictcel; Tabel 4b is al bevestigd.
- Wakend punt: zodra de brandweerketen naar de Instructie Duikarbeid Defensie of een nieuwe tabelversie verwijst, het bronregister en de interne bronadministratie van de beheerder bijwerken.
- Normatieve status van de overige bepalingen van Werkinstructie WOD paragraaf 11.5.1 (terugvalregels, maximum 3 duiken dieper dan 12 m, procedure niet-geplande diepteoverschrijding).
