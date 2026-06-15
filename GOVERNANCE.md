# Governance

Status: publieke conceptversie voor OSOD v0.1.

## 1. Beheerfase

OSOD v0.1 bevindt zich in de initiatieffase. Tijdelijk beheer ligt bij de initiatiefnemer. Het beoogde eindbeeld is beheer onder een organisatieaccount en later onder een breder inhoudelijk gremium met vertegenwoordiging uit het brandweer-/duikarbeidwerkveld.

## 2. Normatieve artefacten

Normatief binnen OSOD v0.1 zijn:

- `STANDAARD.md`;
- `schema/README.md`;
- `schema/osod-logbook-record.schema.json`;
- `rekencontract/README.md`;
- `conformiteit/README.md`;
- gepubliceerde toetsvectoren voor zover als normatief gemarkeerd.

Informatief zijn:

- voorbeelden;
- implementatiemappingen;
- Duikmonitor-notities;
- bronanalyses;
- toelichtingen.

## 3. Wijzigingstypen

| Type | Voorbeeld | Versie-effect |
|---|---|---|
| Redactioneel | spelling, links, verduidelijking zonder inhoudelijke normwijziging | patch |
| Schemawijziging | veld toevoegen, veld verplicht maken, enum wijzigen | minor of major |
| Rekenwijziging | tabelbron, envelop, blokkeergedrag, statussemantiek | veiligheidsreview; minor of major |
| Governancewijziging | beheerder, reviewregels, licentie | minor |
| Erratum | correctie van aantoonbare fout zonder nieuwe norm | patch met erratumvermelding |

## 4. Veiligheidswijzigingen

Veiligheidswijzigingen zijn wijzigingen aan:

- rekencontract;
- tabelbron of bronfingerprint;
- tabelenvelop;
- DCIEM-blokkeergedrag;
- status `GREEN`, `AMBER`, `RED`, `BLOCKED`;
- wettelijke minimumvelden;
- ademgasafbakening;
- toetsvectoren die R-gedrag toetsen.

Een veiligheidswijziging MOET apart worden gemarkeerd en mag niet als zuiver redactioneel worden samengevoegd.

## 5. Bronverplichting

Elke normatieve wijziging MOET herleidbaar zijn naar:

- wet- of regelgeving;
- een registratieschema;
- een inspectie- of branchekader;
- IWOD/DCIEM-bron;
- een expliciet governancebesluit.

Als een voorstel geen bron heeft, wordt het als open punt of implementatievraag gemarkeerd.

## 6. Publicatiebeleid

Publieke releases bevatten geen echte operationele gegevens en geen volledige beschermde bronuitgaven. Volledige tabellen of formulieren worden alleen hergepubliceerd als daarvoor rechten en governance expliciet zijn vastgesteld.

## 7. Relatie met referentie-implementaties

Referentie-implementaties mogen aantonen dat OSOD uitvoerbaar is. Zij bepalen de standaard niet. Implementatiemappingen zijn informatief.
