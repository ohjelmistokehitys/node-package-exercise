# Calendar converter -paketin testit

Tämä paketti sisältää yksinkertaisia testejä kalenterin muuntamiseksi iCalendar-muotoon. Testit on kirjoitettu [Vitest](https://vitest.dev/)-testauskirjastolla, joka on asennettu kehityskäyttöön `devDependencies`-kenttään. Testien on tarkoitus varmistaa, että koodin paketointi on onnistunut ja että kalenterin muutaminen iCalendar-muotoon toimii yleisellä tasolla. Testit eivät kata kattavasti erilaisia tapahtumia, päivämääriä eikä virhetilanteita.

Testien käyttäminen edellyttää, että olet ensin toteuttanut testattavan funktion erilliseen pakettiin, ja paketoinut kyseisen paketin `npm pack` -komennolla. Testattava paketti tulee asentaa tämän testipaketin riippuvuudeksi komennolla `npm install <paketin-nimi>-<versionumero>.tgz`.

## Valmistelut

Aloita asentamalla testattava paketti ja tämän testipaketin riippuvuudet:

```bash
# asennetaan testattava paketti:
npm install <paketin-nimi>-<versionumero>.tgz

# asennetaan riippuvuudet (vitest):
npm install
```

## Testien suorittaminen

Testit suoritetaan komennolla:

```bash
npm test
```


## Paketin uudelleenasennus

Jos testattava paketti päivitetään, tulee se asentaa uudelleen tämän testipaketin riippuvuudeksi. Poista ensin vanha versio komennolla `npm uninstall @example/calendar-converter`, ja asenna sitten uusi versio kuten yllä on kuvattu.
