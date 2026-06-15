# Voorbeelden

Deze map bevat synthetische voorbeeldrecords zonder persoonsgegevens.

- `geldig/` bevat records die tegen het JSON Schema valideren.
- `ongeldig/ontbrekende-duikploegleider.json` is schema-ongeldig omdat de verplichte rol `duikploegleider` ontbreekt.
- `ongeldig/r-claim-zonder-berekening.json` is schema-ongeldig omdat niveau `R` wordt geclaimd zonder verplicht rekenresultaat (`calculation`).
- `ongeldig/groen-met-blokkeerreden.json` is schema-ongeldig omdat status `GREEN` samengaat met een gevulde `blockingReasons`; dit schendt de machinaal afgedwongen invarianten uit `schema/README.md`, sectie 8.1.
- `ongeldig/red-zonder-blokkeerreden.json` is schema-ongeldig omdat status `RED` samengaat met een lege `blockingReasons`.

Let op: `geldig/diepteoverschrijding-geblokkeerd.json` en `geldig/meterregel-geblokkeerd.json` zijn bewust geldig. Zij tonen dat een feitelijke overschrijding geregistreerd mag blijven terwijl de berekening wordt geblokkeerd: de eerste voor de diepte-envelop, de tweede voor de 6-meterregel met cumulatieve duiktijd.
