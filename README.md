# BJL Allround — landingspage

Eén bestand: `index.html`, plus `img/` (foto's) en `fonts/` (lettertypes, lokaal gehost). Geen framework. Live via GitHub Pages op https://www.bjlallround.nl (zie §5).

## 1. Gegevens aanpassen
Er is geen instellingenblok meer: telefoon, WhatsApp, e-mail, KvK en btw-id staan gewoon als tekst in `index.html`.
Wijzig je er een, zoek dan op de oude waarde en vervang **alle** plekken, ook in de JSON-LD bovenin (`<script type="application/ld+json">`).

| gegeven | waarde | staat in |
|---|---|---|
| telefoon | `06-41178935`, link `tel:+31641178935` | spoedbalk, kop, opening + Benjamins kaart, vergelijking, contact, voet, mobiele balk |
| WhatsApp | `https://wa.me/31641178935` (met voorgevuld berichtje) | opening, reacties, diensten, vragen, contact, voet, mobiele balk |
| e-mail | `bjlzakelijk@gmail.com` | contact, voet, JSON-LD |
| KvK / btw-id | `88991490` / `NL004679991B76` | voet (wettelijk verplicht zichtbaar), btw ook in JSON-LD |

Er is bewust **geen formulier** (Daniel, 2026-10-05): klanten bellen, appen of mailen. Aanvragen komen dus niet meer
vanzelf in de Klusboek-app binnen; het koppelpunt `app.bjlallround.nl/api/aanvraag` bestaat nog, maar de site gebruikt het niet.

## 2. Foto's
Staan in `img/` (verkleind, EXIF/locatie verwijderd):

- `werk-N.jpg` (900 px breed) en `werk-N-s.jpg` (450 px): galerij en vergroting
- `hero.jpg` / `hero-s.jpg`: de badkamer in de opening (op de telefoon donker als achtergrond)
- `portret.jpg`: Benjamin (Maak kennis, kaartje in de opening, vragen, contact); `og.jpg`: deel-afbeelding voor WhatsApp en socials
- `werk-N-t.jpg` en `hero-t.jpg` worden sinds 2026-10-11 niet meer gebruikt

Een klus toevoegen: kopieer in de sectie `id="werk"` een `<figure class="shot">` en pas `data-title`, `data-desc`
(uitleg in de vergroting), `data-full`, `href`, `src`/`srcset`, `alt` en het bijschrift aan. Houd het aantal deelbaar door 4
(computer: 4 naast elkaar) en 2 (telefoon). Maak eerst de twee maten, bijvoorbeeld met `sips -Z 1200` en `sips -Z 600`.

## 3. Beweging
Rustig en alleen als de bezoeker geen 'minder beweging' heeft gekozen: blokken komen binnen bij scrollen (`data-rv`),
de opening komt na elkaar binnen (`data-in`), in de vergelijking loopt een rand vakje voor vakje door de kolom BJL Allround,
de WhatsApp-berichten komen één voor één binnen (twee rondes, dan blijven de laatste drie staan), een kader in blauw chroom
loopt langs de vier stappen en op de computer stroomt vloerverwarming door de achtergrond van Diensten en Vragen.
Zonder JavaScript, bij 'minder beweging' en bij printen staat alles gewoon stil en volledig in beeld.

## 4. Reacties van klanten
Letterlijke WhatsApp-berichtjes, zonder naam, in de sectie `id="reacties"` als `<figure class="bericht">`.
Nieuwe toevoegen: kopieer er een. Geen verzonnen reviews; namen alleen met toestemming.

## 4b. Lettertypes en overige bestanden
- `fonts/` bevat Archivo (koppen), Instrument Sans (tekst) en Caveat (handgeschreven woorden), lokaal gehost (OFL-licentie), zodat er geen verzoek naar Google Fonts gaat (AVG).
- `404.html`, `robots.txt`, `sitemap.xml`, favicons en `img/og.jpg` staan in de root.
- Sinds 2026-10-11 is `index.html` weer **met de hand onderhouden**: de vertrouwde opbouw van de eerste site, aangevuld met delen van de tweede (spoedbalk, Benjamins kaart, vergelijking, WhatsApp-reacties, stappen, vloerverwarming).
  Het werkexemplaar staat in `ontwerp/terug/site/index.html` (niet in git) en is gelijk aan `index.html`. Het oude bouwscript `ontwerp/bouw.py` maakte de vorige versie en stopt nu met een melding; niet meer draaien, anders wordt `index.html` overschreven.

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
