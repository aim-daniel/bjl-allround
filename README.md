# BJL Allround — landingspage

Eén bestand: `index.html`, plus `img/` (foto's) en `fonts/` (lettertypes, lokaal gehost). Geen framework. Live via GitHub Pages op https://www.bjlallround.nl (zie §5).

## 1. Gegevens aanpassen
Er is geen instellingenblok meer: telefoon, WhatsApp, e-mail, KvK en btw-id staan gewoon als tekst in `index.html`.
Wijzig je er een, zoek dan op de oude waarde en vervang **alle** plekken, ook in de JSON-LD bovenin (`<script type="application/ld+json">`).

| gegeven | waarde | staat in |
|---|---|---|
| telefoon | `06-41178935`, link `tel:+31641178935` | strip bovenaan, kop, contactkaart, knoppen, voet, mobiele balk |
| WhatsApp | `https://wa.me/31641178935` | klussen, reacties, contact, voet, mobiele balk |
| e-mail | `bjlzakelijk@gmail.com` | contact, voet, JSON-LD |
| KvK / btw-id | `88991490` / `NL004679991B76` | voet (wettelijk verplicht zichtbaar), btw ook in JSON-LD |

Er is bewust **geen formulier** (Daniel, 2026-10-05): klanten bellen, appen of mailen. Aanvragen komen dus niet meer
vanzelf in de Klusboek-app binnen; het koppelpunt `app.bjlallround.nl/api/aanvraag` bestaat nog, maar de site gebruikt het niet.

## 2. Foto's
Staan in `img/` (verkleind, EXIF/locatie verwijderd). Elke werkfoto bestaat in drie maten:

- `werk-N.jpg` (900 px breed) en `werk-N-s.jpg` (450 px): de grote foto in de galerij, de browser kiest zelf
- `werk-N-t.jpg` (165 px): het kleine kiezer-fotootje eronder
- `hero.jpg` / `hero-s.jpg` / `hero-t.jpg`: de badkamer, eerste foto van de galerij
- `portret.jpg`: Benjamin (bij "Maak kennis" en klein in de contactkaart); `og.jpg`: deel-afbeelding voor WhatsApp en socials

Een klus toevoegen in de galerij gaat op drie plekken in de sectie `id="werk"`, steeds in dezelfde volgorde:
een `<img class="laag" data-src=…>` in `figure.uitgelicht`, een `<button class="kies" data-i=…>` in `.kiezers`
(nummer `data-i` doortellen), en een regel `{"t": titel, "d": uitleg}` in `<script type="application/json" id="g-data">`.
Pas ook de teller `/ 09` aan. Maak eerst de drie maten, bijvoorbeeld met `sips -Z 1200`, `-Z 600` en `-Z 220`.

## 3. Beweging
Het bewegingssysteem is hetzelfde als op aimintelligence.app: `data-reveal` (blok komt omhoog of schuift in),
`data-enter` (opening na elkaar), `data-steps` (de vier stappen lopen mee met scrollen). De galerij bladert elke 6 s door
en stopt zodra iemand zelf een foto kiest; de reacties komen als WhatsApp-berichten binnen. Zonder JavaScript en bij
"minder beweging" staat alles gewoon stil en volledig in beeld.

## 4. Reacties van klanten
Letterlijke WhatsApp-berichtjes, zonder naam, in de sectie `id="reacties"` als `<figure class="bericht">`.
Nieuwe toevoegen: kopieer er een. Geen verzonnen reviews; namen alleen met toestemming.

## 4b. Lettertypes en overige bestanden
- `fonts/` bevat Oxanium (koppen) en DM Sans (tekst) als woff2 (OFL-licentie), zodat er geen verzoek naar Google Fonts gaat (AVG).
- `404.html`, `robots.txt`, `sitemap.xml`, favicons en `img/og.jpg` staan in de root.
- Het ontwerp en het bouwscript staan in `ontwerp/` (niet in git): `python3 ontwerp/v6_volgorde.py` maakt `ontwerp/v6.html` uit `ontwerp/v5.html` (volgorde en telefoonversie), daarna maakt `python3 ontwerp/bouw.py` de echte `index.html`.
  Wie `index.html` met de hand aanpast, moet dat daarna niet meer doen, anders worden de handwijzigingen overschreven.

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

Updates daarna: bestand aanpassen, nieuwe bestanden (foto's, lettertypes) zelf toevoegen met `git add <bestand>`, dan `git commit -am "…"` en `git push`. Let op: `commit -am` neemt alleen bestanden mee die git al kent; een nieuwe foto zonder `git add` staat dus niet online.
