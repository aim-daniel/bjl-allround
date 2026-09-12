# BJL Allround — landingspage

Eén bestand: `index.html`. Geen build, geen framework. Werkt op elke hosting (Vercel, Netlify, Cloudflare Pages, of gewoon een map op een webserver).

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
| `formEndpoint` | `"https://formspree.io/f/xxxx"` | formulier direct versturen (zie 3) |

Zolang een veld leeg is, staat er op de pagina een amber pil "… invullen". Zo zie je meteen wat nog mist. Niet live zetten met ambers.

## 2. Foto's
Staan al in `img/` (verkleind, EXIF/locatie verwijderd):

- `img/hero.jpg` — badkamer met verlichte spiegel (bovenaan)
- `img/werk-1.jpg` t/m `werk-8.jpg` — galerij: 4× sanitair, vloerverwarming, verdeler, riool, hemelwaterafvoer
- `img/portret.jpg` — portret van Benjamin bij "Maak kennis met Benjamin" (vierkant, gecropt)

Extra klus toevoegen: kopieer een `<figure class="shot">` in de sectie Werk (voorbeeld staat in commentaar).

Maak foto's max. ~1600px breed en comprimeer ze (bijv. squoosh.app), anders is de pagina traag op mobiel.

## 3. Formulier
- **Zonder endpoint**: het formulier opent de mail-app van de bezoeker met alles voorgevuld (mailto naar `email`). Werkt overal, geen backend.
- **Met endpoint**: vul `formEndpoint` in met een Formspree-, Basin- of eigen URL die `multipart/form-data` accepteert en JSON terugstuurt. Dan blijft de bezoeker op de pagina en krijgt een bevestiging. Het veld `_gotcha` is een honeypot tegen bots.
- Later CRM: het formulier stuurt de velden `naam, telefoon, email, plaats, type, omschrijving, _subject`. Een eigen endpoint kan die direct in een database schrijven.

## 4. Reviews
Sectie "Reviews" toont nu een placeholder. Echte reviews plaats je met de kaart die in commentaar in de sectie staat. Geen verzonnen reviews.

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
