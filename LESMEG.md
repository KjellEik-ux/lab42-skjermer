# LAB42 – skjermer på kontoret

Tre skjermer, én side, én datafil:

| Skjerm  | Adresse                         | Viser |
|---------|---------------------------------|-------|
| Venstre | `skjerm.html?skjerm=venstre`    | Andy og Christoph |
| Midt    | `skjerm.html?skjerm=midt`       | Kristian, Johannes, Annkarin + utstyr utleid og beskjed |
| Høyre   | `skjerm.html?skjerm=hoyre`      | Oppdrag: pågående prosjekter med sjekkliste, kommende oppdrag, 3-ukers kalender, utleie |

Alt innhold ligger i **`data.json`**. Skjermene henter filen på nytt hvert minutt, så når dere endrer den, oppdateres skjermene av seg selv. Ingen ny opplasting i Yodeck.

Sidene skalerer seg selv til skjermen (laget for 1920×1080). Står det mye tekst i en kolonne, krymper skriften automatisk så alt får plass.

---

## Adresser (allerede satt opp)

- Venstre: https://kjelleik-ux.github.io/lab42-skjermer/skjerm.html?skjerm=venstre
- Midt: https://kjelleik-ux.github.io/lab42-skjermer/skjerm.html?skjerm=midt
- Høyre: https://kjelleik-ux.github.io/lab42-skjermer/skjerm.html?skjerm=hoyre

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

Kategorier med egen farge: Lager, Utstyr, Verksted, Lokaler, Transport, Biler, Ekstern/kunde, Felles. Andre navn fungerer også (får standardfarge).

**Flytte folk mellom skjermene:** endre `"kolonner"` under `"skjermer"`. Venstre har to brede kolonner, midt har tre smalere + sidepanel.

**Nytt oppdrag** (høyre skjerm):
```json
{ "tittel": "Rigg Oslo Spektrum", "type": "Rigg", "fra": "2026-11-03", "til": "2026-11-05", "tid": "07:00", "sted": "Oslo", "info": "2 biler" }
```
Bruk `"nr"` med jobbkoden (f.eks. `"OSV-26-003"`) – den vises over datoen og tidspunktet, og bestemmer fargen: **LAB-… = turkis (LAB42)**, **OSV-… = oransje (Oslo Sveiseverksted)**. Uten `nr` gir `type` farge: Retur, Levering, Rigg, Event, Transport.

**Utleie** legges i `"utleid"`, f.eks. `{ "navn": "Skal vi danse", "til": "2026-12-17" }`. Det vises i boksen «Utleie» og i blått i kalenderen. Bruk `"fra"`/`"til"` eller `"uke"`; uten `"fra"` regnes det som ute fra i dag. Oppdrag med `"type": "Utleie"` flyttes dit automatisk og vises ikke under «Oppdrag» eller i kalenderen. Uten `fra`-dato havner oppdraget under «Uten dato». Vet du bare uka, bruk `"uke": 46` i stedet for `fra`/`til`.

**Prosjekt med sjekkliste** (høyre skjerm, under «Pågår nå»):
```json
{ "tittel": "Onkel Blaa", "nr": "LAB-26-001", "type": "Prosjekt", "pagar": true,
  "sjekkliste": [
    { "tekst": "Hente bordplate", "hvem": "Andy", "ferdig": false },
    { "tekst": "Lakkere bord", "ferdig": true }
  ] }
```
Sett `"ferdig": true` når et punkt er gjort, da strekes det over og fremdriftslinjen fylles. `"pagar": true` legger oppdraget øverst under «Pågår nå» (gjelder også utleie som er ute).
Vil du knytte en personoppgave til prosjektet, legg til `"prosjekt": "LAB-26-001"` på oppgaven, så får den en liten prosjektmerkelapp.

**Hvor beskjeden vises:** Beskjeden vises bare på skjermer med `"beskjed": true` under `"skjermer"` (nå midtskjermen).

**Beskjed og utleid utstyr:** `"beskjed"` og `"utleid"` øverst/nederst i filen.

**Tips:** JSON er pirkete med komma og anførselstegn. Lim filen inn på jsonlint.com før du lagrer hvis du er usikker. Blir filen ødelagt, viser skjermene sist kjente versjon og en oransje prikk nede til høyre.
