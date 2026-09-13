# BJL Allround — landingspage

Eén bestand: `index.html`, plus `img/` (foto's) en `fonts/` (lettertypes, lokaal gehost). Geen build, geen framework. Live via GitHub Pages op https://www.bjlallround.nl (zie §5).

## 1. Gegevens invullen
Bovenaan `index.html` staat een blok `window.BJL = { ... }`. Vul daar in:

| veld | voorbeeld | gebruikt voor |
|---|---|---|
| `phone` | `"06 12345678"` | getoond op de pagina en in de belknoppen |
| `phoneRaw` | `"+31612345678"` | de `tel:`-link (internationaal formaat) |
| `whatsapp` | `"31612345678"` | de WhatsApp-knop (`wa.me`), zonder `+` of `00` |
| `email` | `"info@bjlallround.nl"` | mailknop én fallback van het formulier |
| `region` | `"Rotterdam en omstreken"` | werkgebied in FAQ, contact en footer |
| `kvk` | `"12345678"` | footer |
| `formEndpoint` | `"https://…"` (eigen endpoint of EU-verwerker) | formulier direct versturen (zie 3) |
| `btw` | `"NL001234567B01"` | btw-id in de footer |
| `adres` | `"Straat 1, 1234 AB Plaats"` | vestigingsadres in de footer |

Zolang een veld leeg is, wordt dat onderdeel op de pagina verborgen (lege e-mail: mailknop weg en het formulier gaat via WhatsApp). Extra velden: `btw` (btw-id) en `adres` (vestigingsadres) verschijnen in de footer zodra ze zijn ingevuld; wettelijk moeten KvK, btw-id en een contactmogelijkheid zichtbaar zijn.

## 2. Foto's
Staan al in `img/` (verkleind, EXIF/locatie verwijderd):

- `img/hero.jpg` — badkamer met verlichte spiegel (bovenaan)
- `img/werk-1.jpg` t/m `werk-8.jpg` — galerij: 4× sanitair, vloerverwarming, verdeler, riool, hemelwaterafvoer
- `img/portret.jpg` — portret van Benjamin bij "Maak kennis met Benjamin" (vierkant, gecropt)

Extra klus toevoegen: kopieer een `<figure class="shot">` in de sectie Werk (voorbeeld staat in commentaar).

Maak foto's max. ~1600px breed en comprimeer ze (bijv. squoosh.app), anders is de pagina traag op mobiel.

## 3. Formulier
- **Zonder endpoint en zonder e-mail**: het formulier opent WhatsApp met de aanvraag voorgevuld; de bezoeker ziet het bericht eerst en drukt zelf op verzenden.
- **Zonder endpoint, met e-mail**: het formulier opent de mail-app van de bezoeker met alles voorgevuld (mailto naar `email`). Werkt overal, geen backend.
- **Met endpoint**: vul `formEndpoint` in met een eigen URL of een verwerker binnen de EU die `multipart/form-data` accepteert en JSON terugstuurt (let op: een Amerikaanse dienst zoals Formspree vraagt om een verwerkersovereenkomst en doorgiftegrondslag onder de AVG). Dan blijft de bezoeker op de pagina en krijgt een bevestiging. Het veld `_gotcha` is een honeypot tegen bots.
- Later CRM: het formulier stuurt de velden `naam, telefoon, email, plaats, type, omschrijving, _subject`. Een eigen endpoint kan die direct in een database schrijven.

## 4. Reacties van klanten
De sectie "Reacties" toont letterlijke WhatsApp-berichtjes van klanten, zonder naam. Nieuwe toevoegen: kopieer een `<figure class="review">` in die sectie. Geen verzonnen reviews; namen alleen met toestemming.

## 4b. Lettertypes en overige bestanden
- `fonts/` bevat Archivo, Instrument Sans en Caveat als woff2 (OFL-licentie), zodat er geen verzoek naar Google Fonts gaat (AVG).
- `404.html`, `robots.txt`, `sitemap.xml`, favicons (`favicon.ico`, `favicon-32.png`, `apple-touch-icon.png`) en `img/og.jpg` (deel-afbeelding voor WhatsApp/socials) staan in de root.
- Galerijfoto's: `img/werk-N.jpg` (groot, voor de lightbox) én `img/werk-N-s.jpg` (450 px, voor de tegels). Nieuwe foto: beide maten aanmaken.

## 5. Live zetten (GitHub Pages + Strato-domein)
Hosting: GitHub Pages, repo `aim-daniel/bjl-allround`, branch `main`, map `/`. Elke push naar `main` is binnen een minuut live.

Eenmalig (in de terminal, ingelogd als aim-daniel):

```bash
cd ~/bjl-allround && gh repo create aim-daniel/bjl-allround --public --source=. --push && gh api -X POST repos/aim-daniel/bjl-allround/pages -f build_type=legacy -f 'source[branch]=main' -f 'source[path]=/'
```

Tijdelijke URL: https://aim-daniel.github.io/bjl-allround/

Domein `www.bjlallround.nl` (staat in het bestand `CNAME`). Bij Strato → Domeinen → bjlallround.nl → DNS-instellingen:

| type | naam/host | waarde |
|---|---|---|
| A | @ (bjlallround.nl) | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | aim-daniel.github.io |

De bestaande A-record van Strato (217.160.0.231) en de bestaande CNAME `www → bjlallround.nl` verwijderen. Daarna in de repo: Settings → Pages → Custom domain = www.bjlallround.nl, "Enforce HTTPS" aanzetten zodra het certificaat er is (tot 24 uur). Het kale domein bjlallround.nl stuurt GitHub dan automatisch door naar www.

Updates daarna: bestand aanpassen, `git commit -am "…"`, `git push`.
