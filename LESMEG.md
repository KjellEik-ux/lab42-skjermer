# LAB42 – skjermer på kontoret

Fem skjermer, nummerert 1–5 fra venstre, én side, én datafil:

| Skjerm | Adresse                  | Viser |
|--------|--------------------------|-------|
| 1      | `skjerm.html?skjerm=1`   | (ny, vertikal – innhold kommer) |
| 2      | `skjerm.html?skjerm=2`   | Andy og Christoph |
| 3      | `skjerm.html?skjerm=3`   | Kristian, Johannes, Annkarin + beskjed |
| 4      | `skjerm.html?skjerm=4`   | Oppdrag: pågående prosjekter med sjekkliste, kommende oppdrag, 3-ukers kalender, utleie |
| 5      | `skjerm.html?skjerm=5`   | (ny, horisontal – innhold kommer) |

De gamle adressene (`venstre`, `midt`, `hoyre`) virker fortsatt og viser skjerm 2, 3 og 4.

Alt innhold ligger i **`data.json`**. Skjermene henter filen på nytt hvert minutt, så når dere endrer den, oppdateres skjermene av seg selv. Ingen ny opplasting i Yodeck.

Sidene skalerer seg selv til skjermen (laget for 1920×1080). Står det mye tekst i en kolonne, krymper skriften automatisk så alt får plass.

---

## Adresser (allerede satt opp)

- Skjerm 1: https://kjelleik-ux.github.io/lab42-skjermer/skjerm.html?skjerm=1
- Skjerm 2: https://kjelleik-ux.github.io/lab42-skjermer/skjerm.html?skjerm=2
- Skjerm 3: https://kjelleik-ux.github.io/lab42-skjermer/skjerm.html?skjerm=3
- Skjerm 4: https://kjelleik-ux.github.io/lab42-skjermer/skjerm.html?skjerm=4
- Skjerm 5: https://kjelleik-ux.github.io/lab42-skjermer/skjerm.html?skjerm=5

Filene ligger i GitHub-repoet `KjellEik-ux/lab42-skjermer` og publiseres med GitHub Pages. I Yodeck er hver adresse lagt inn som en Web Page-app med egen playlist per skjerm.

> Repoet er offentlig, så hvem som helst med lenken kan se oppgavelista.

## Push nye oppgaver (hver gang)

1. Åpne `data.json` på github.com → klikk blyanten (Edit).
2. Endre det du vil (se under) → «Commit changes».
3. Skjermene viser endringen innen 1–2 minutter.

Oppdater gjerne `"oppdatert"` øverst, da står tidspunktet i hjørnet på skjermene.

---

## Slik skriver du i data.json

**En oppgave** (under en person):
```json
{ "kategori": "Lager", "tekst": "Sorter kabler etter type/funksjon" }
```
Valgfrie felt:
- `"med": ["Andy", "Kristian"]` – hvem man jobber sammen med (vises som brikker)
- `"notat": "Status uavklart"` – grå brikke
- `"advarsel": "Bekreft at strømmen er frakoblet først"` – oransje varsel
- `"haster": true` – havner øverst i en rød «Haster»-boks, og kortet får et rødt merke
- `"evergreen": true` – løpende oppgave som alltid står nederst under «Løpende» (teller ikke som vanlig oppgave)

Kategorier med egen farge: Kunde/prosjekt, Verksted, Utstyr, Lager, Bygg/drift, Transport/biler. Andre navn fungerer også (får standardfarge).

Skjul utleieboksen på en skjerm med `"utleie": false` under skjermen i `skjermer`.

**Flytte folk mellom skjermene:** endre `"kolonner"` under `"skjermer"`. Skjerm 2 har to brede kolonner, skjerm 3 har tre smalere + sidepanel.

**Nytt oppdrag** (skjerm 4):
```json
{ "tittel": "Rigg Oslo Spektrum", "type": "Rigg", "fra": "2026-11-03", "til": "2026-11-05", "tid": "07:00", "sted": "Oslo", "info": "2 biler" }
```
Bruk `"nr"` med jobbkoden (f.eks. `"OSV-26-003"`) – den vises over datoen og tidspunktet, og bestemmer fargen: **LAB-… = turkis (LAB42)**, **OSV-… = oransje (Oslo Sveiseverksted)**. Uten `nr` gir `type` farge: Retur, Levering, Rigg, Event, Transport.

**Utleie** legges i `"utleid"`, f.eks. `{ "navn": "Skal vi danse", "til": "2026-12-17" }`. Det vises i boksen «Utleie» og i blått i kalenderen. Bruk `"fra"`/`"til"` eller `"uke"`; uten `"fra"` regnes det som ute fra i dag. Oppdrag med `"type": "Utleie"` flyttes dit automatisk og vises ikke under «Oppdrag» eller i kalenderen. Uten `fra`-dato havner oppdraget under «Uten dato». Vet du bare uka, bruk `"uke": 46` i stedet for `fra`/`til`.

**Prosjekt med sjekkliste** (skjerm 4, under «Pågår nå»):
```json
{ "tittel": "Onkel Blaa", "nr": "LAB-26-001", "type": "Prosjekt", "pagar": true,
  "sjekkliste": [
    { "tekst": "Hente bordplate", "hvem": "Andy", "ferdig": false },
    { "tekst": "Lakkere bord", "ferdig": true }
  ] }
```
Sett `"ferdig": true` når et punkt er gjort, da strekes det over og fremdriftslinjen fylles. `"pagar": true` legger oppdraget øverst under «Pågår nå» (gjelder også utleie som er ute).
Vil du knytte en personoppgave til prosjektet, legg til `"prosjekt": "LAB-26-001"` på oppgaven, så får den en liten prosjektmerkelapp.

**Hvor beskjeden vises:** Beskjeden vises bare på skjermer med `"beskjed": true` under `"skjermer"` (nå skjerm 3).

**Beskjed og utleid utstyr:** `"beskjed"` og `"utleid"` øverst/nederst i filen.

**Oslo Sveiseverksted – fakturert denne måneden** (skjerm 4, under Utleie):
Ligger i **`okonomi.json`**, ikke i `data.json`, og oppdateres automatisk fra Tripletex av en planlagt oppgave i Claude (hver time på hverdager, 07–17). Ikke rediger den for hånd. Tallet er sum fakturert eks. mva på avdeling «Avdeling Oslo Sveiseverksted» fra den 1. i måneden, med kreditnotaer trukket fra. Første dag i ny måned viser skjermen 0 til første oppdatering. Mangler filen, vises ikke kortet.

**Tips:** JSON er pirkete med komma og anførselstegn. Lim filen inn på jsonlint.com før du lagrer hvis du er usikker. Blir filen ødelagt, viser skjermene sist kjente versjon og en oransje prikk nede til høyre.
