# Duikmonitor-conformiteit

Status: concept, ondersteunend, niet normatief.

## 1. Doel

Dit document beschrijft hoe Duikmonitor kan aantonen dat een specifieke versie OSOD-conform is.

## 2. Vereiste verklaring

Een conformiteitsverklaring ZOU minimaal bevatten:

```text
Applicatie: Duikmonitor
Applicatieversie: <versie>
OSOD-versie: 0.1
Schema: schema/osod-logbook-record.schema.json
Conformiteitsniveaus: OSOD-S en/of OSOD-R
Datum toetsing: <datum>
Bronfingerprint rekenreferentie: <sha256 indien R wordt geclaimd>
Afwijkingen: <geen / lijst>
```

## 3. Toetslagen

1. JSON Schema-validatie van OSOD-exportrecords.
2. Basistoetsen uit `conformiteit/toetsvectoren/basistoetsen-v0.1.json`.
3. Besloten volledige DCIEM/IWOD-vectorenset, indien beschikbaar en juridisch bruikbaar.
4. Review van veiligheidskritische presentatie: geen geldige groene uitkomst bij RED of BLOCKED.

## 4. Niet-normatief

Duikmonitor MAG extra veiligheidscontroles doen boven OSOD. Extra controles zijn toegestaan zolang de OSOD-export niet onwaar of misleidend wordt.


## 5. Relatie met architectuurschets

`duikmonitor-architectuurschets.md` beschrijft een mogelijke technische scheiding tussen DCIEM-rekenkern, OSOD-mapping/export en conformiteitscontrole. Die scheiding is niet verplicht, maar maakt een conformiteitsclaim beter toetsbaar.
