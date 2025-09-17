# Npm paketit

Tässä tehtävässä opit luomaan ja käsittelemään npm-paketteja. Npm (Node Package Manager) on Node.js:n mukana tuleva pakettien hallintajärjestelmä, joka mahdollistaa kolmannen osapuolen kirjastojen ja työkalujen asentamisen ja hallinnan projekteissasi:

> *"The public npm registry is a database of JavaScript packages, each comprised of software and metadata. Open source developers and developers at companies use the npm registry to contribute packages to the entire community or members of their organizations, and download packages to use in their own projects."*
>
> https://docs.npmjs.com/about-the-public-npm-registry

Tehtävässä sinun *ei tarvitse* julkaista pakettia npm-rekisteriin, vaan riittää, että luot paketin paikallisesti, varmistat sen toimivuuden ja kokeilet sen käyttöä toisessa projektissa. Julkaiseminen on sallittua, mutta ei pakollista.

Suosittelemme TypeScriptin käyttöä, mutta se ei ole ehdoton vaatimus.


## Tehtävän ratkaiseminen

Tämän tehtävän suorittamiseksi sinun tulee perehtyä npm:n dokumentaatioon ja seurata sen vaiheita: https://docs.npmjs.com/packages-and-modules/contributing-packages-to-the-registry. Tässä readme-tiedostossa olevat ohjeet täydentävät virallista dokumentaatiota ja tarkentavat vaatimuksia, joita pakettisi tulee täyttää. Lisäksi suosittelemme etsimään tutoriaaleja ja ohjeita sitä mukaan, kun kohtaat uusia asioita.

Aloita kloonaamalla kopio tehtävärepositoriosta omalle koneellesi. Avaa repositorio VS Code:ssa sekä komentorivillä. Tehtävän edetessä tee uusia committeja ja pushaa ne GitHubiin.

Tehtävänanto etenee vaiheittain, mutta tosiasiassa joudut mahdollisesti palaamaan aiempiin vaiheisiin myöhemmin, mikäli kohtaat seuraavissa vaiheissa virheitä. Tehtävä arvostellaan vaiheiden perusteella, eli voit saada ratkaisustasi kelvollisen arvosanan, vaikka et saisi kaikkia vaiheita valmiiksi.


## Toiminnalliset vaatimukset

Tässä tehtävässä toteutat npm-paketin, jossa oleva funktio muuttaa sille annetut JSON-muotoiset kalenteritiedot kalenteriohjelmien laajasti tukemaan [iCalendar-muotoon](https://fi.wikipedia.org/wiki/ICalendar).

Funktiotasi voidaan hyödyntää joko osana CLI-työkalua tai esimerkiksi web-sovellusta, joka tarjoaa käyttäjille mahdollisuuden synkronoida opintojaksojen tapahtumia omiin kalentereihinsa.


### Kalenterin tietorakenne

Tehtävässä käsiteltävä JSON-muotoinen kalenteriformaatti sisältää kalenterin yleisiä tietoja sekä tapahtumia. Yksittäinen tapahtuma voi näyttää esimerkiksi tältä:

```json
{
    "event_id": 101,
    "start_date": "2027-09-02 09:00",
    "end_date": "2027-09-02 10:30",
    "subject": "Sprint Planning",
    "location": [
        {
            "class": "ConfRoom-A",
            "name": "Conference Room",
            "parent": "Scranton"
        }
    ]
}
```

Kokonainen esimerkki kalenterista löytyy tiedostosta [`example-input.json`](./example-input.json).

Kalenteria varten on luotu valmiit TypeScript-tyypit, jotka löydät tiedostosta [`utils/types.ts`](./utils/types.ts). Siirrä halutessasi nämä tyypit omaan pakettiisi tehtävän seuraavissa vaiheissa, mikäli käytät TypeScriptiä.


### Tuotettava formaatti

Funktiosi tulee palauttaa iCalendar-muotoa noudattava merkkijono, joka sisältää funktiolle JSON-muodossa annetut tapahtumat. Tämä tapahtumatieto tulee voida lähettää esimerkiksi Googlen tai Outlookin kalenteriin.

Edellä esitetty JSON-tapahtuma näyttää iCalendar-muodossa esimerkiksi tältä:

```
BEGIN:VEVENT
UID:101
DTSTAMP:20250917T114617Z
DTSTART:20270902T060000Z
DTEND:20270902T073000Z
SUMMARY:Sprint Planning
LOCATION:Conference Room\, Scranton
DESCRIPTION:Sprint Planning
END:VEVENT
```

Kokonainen esimerkki JSON-datan perusteella muodostettavasta iCalendar-muodosta löytyy tiedostosta [`example-output.ics`](./example-output.ics).

> [!IMPORTANT]
> Sinun ei tarvitse itse muodostaa iCalendar-muotoa käsin, vaan voit hyödyntää npm-paketteja, jotka tekevät tämän puolestasi. Palaamme aiheeseen alempana.


## Vaihe 1: paketin luominen

Repositoriosta löytyy valmiina kansio nimeltä [`calendar-converter`](./calendar-converter), johon npm-paketti toteutetaan. Siirry kyseiseen hakemistoon ja alusta sinne uusi npm-projekti [npm:n ohjeiden mukaisesti](https://docs.npmjs.com/creating-a-package_json_file).

Anna paketillesi nimeksi `@example/calendar-converter`. `@example` on ns. [scope](https://docs.npmjs.com/about-scopes), joka erottaa pakettisi muista mahdollisesti samalla nimellä olevista paketeista. Tässä harjoituksessa `@example`-scopea käytetään varmistamaan, että paketin nimi ei vastaa mitään npm-rekisterissä olevaa tai sinne myöhemmin julkaistavaa pakettia.

> [!IMPORTANT]
> Paketin nimen on oltava täsmälleen `@example/calendar-converter`, jotta sen asennus onnistuu myöhemmissä vaiheissa. Mikäli haluat myöhemmin julkaista pakettisi npm-rekisteriin, vaihda jälkikäteen tilalle jokin toinen nimi.

Määrittele `package.json`-tiedostoon ohjeiden mukaisesti vähintään kentät `"name"`, `"version"` ja `"description"`. Versionumeron täytyy noudattaa [semver](https://semver.org/) -käytäntöjä, eli muotoa `<major>.<minor>.<patch>`. Voit harkintasi mukaan määritellä myös muita kenttiä.

Tehdessäsi muutoksia koodiin, voit päivittää versionumeroa `npm version <major|minor|patch>`-komennolla, eli esimerkiksi `npm version patch`. `npm version` kasvattaa versionumeroa `package.json`-tiedostossa. Lisää tietoa aiheesta löydät [npm:n dokumentaatiosta](https://docs.npmjs.com/cli/commands/npm-version).

Palaamme tehtävässä myöhemmin täydentämään `package.json`-tiedostoa uusilla kentillä.


## Vaihe 2: riippuvuuksien asentaminen

Kalenteridatan muodostaminen käsin vaadittuun muotoon on työlästä ja voi vaatia runsaasti perehtymistä iCalendar-formaattiin. Kalenteriformaatin ohella tehtävässä käsitellään myös päivämääriä ja kellonaikoja, joiden muuntaminen eri aikavyöhykkeiden sekä kesä- ja talviaikojen välillä voi olla haastavaa.

Näiden ongelmien ratkaisemiseksi kannustamme käyttämään valmiita npm-paketteja, jotka helpottavat tehtävää huomattavasti. Seuraavaksi käsitellään kahta tehtävässä hyödylliseksi havaittua pakettia, mutta voit käyttää myös muita paketteja tai toteuttaa logiikan käsin.

**ical-generator**

Tätä tehtävää laadittaessa iCalendar-muodon generoinnissa on hyödynnetty [ical-generator](https://www.npmjs.com/package/ical-generator) -nimistä npm-pakettia, joka vaikuttaa suositulta ja hyvin dokumentoidulta. Lisäksi siitä löytyy TypeScript-tyypit valmiina ja se on lisensoitu avoimen lähdekoodin MIT-lisenssillä. ical-generatorin dokumentaatio ja esimerkkejä sen käyttämisestä löytyy GitHubista: https://github.com/sebbo2002/ical-generator.

ical-generator tukee suoraan Luxon-kirjaston `DateTime`-olioita, joten se sopii hyvin yhteen Luxonin kanssa.

**Luxon**

Lähdeaineistossa päivämäärät ja kellonajat on esitetty merkkijonoina ilman aikavyöhykettä sekä ilman tietoa kesä- ja talviajasta (esim. `"2027-09-02 09:00"`). [Luxon](https://www.npmjs.com/package/luxon)-niminen kirjasto tarjoaa hyvät työkalut päivämäärien ja kellonaikojen käsittelyyn, mukaan lukien aikavyöhykkeet ja kesä-/talviaika.

Luxon ei sisällä valmiita TypeScript-tyyppejä, mutta ne löytyvät erikseen paketista [@types/luxon](https://www.npmjs.com/package/@types/luxon). Luxon on lisensoitu avoimen lähdekoodin MIT-lisenssillä ja sen dokumentaatio löytyy osoitteesta https://moment.github.io/luxon/.



**dependencies vs. devDependencies**

Npm-paketit asennetaan komennolla `npm install <paketti>`, joka lataa valitun paketin `node_modules`-hakemistoon sekä päivittää tiedon riippuvuudesta `package.json`-tiedostoon. Kalenteridatan ja päivämäärien käsittelyyn tarvittavat paketit tulee asentaa "tuotantokäyttöön" (`dependencies`-kenttä), jotta ne asentuvat pakettisi mukana silloin, kun joku muu asentaa pakettisi. Sen sijaan kehitystyössä tarvittavat paketit, kuten TypeScript, tsx, testauskirjastot tai pakettien tyyppimäärittelyt, tulee asentaa "kehityskäyttöön" (`devDependencies`-kenttä) komennolla `npm install <paketti> --save-dev`.

Perehdy npm:n dokumentaation sivuun [Specifying dependencies and devDependencies in a package.json file](https://docs.npmjs.com/specifying-dependencies-and-devdependencies-in-a-package-json-file) ja asenna tarvittavat paketit projektiisi.


## Vaihe 3: funktion luonti

Toteuta pakettiisi JavaScript- tai TypeScript-tiedosto, jossa on funktio nimeltä `convertToICalendar`. Funktion tulee ottaa vastaan [JavaScript-kalenteriolio](./example-input.json) ja palauttaa [iCalendar-muotoinen merkkijono](./example-output.ics). Tämä funktio tulee "exportata" paketin oletusfunktiona.  Vaikka kyseessä on yksittäinen funktio, kannattaa paketin toteutuksessa mahdollisesti hyödyntää tarkoituksenmukaista hakemistorakennetta ja jakaa koodia erillisiin funktioihin, joilla ratkaisusta tulee selkeä ja ymmärrettävä.

Jos käytät TypeScriptiä, voit hyödyntää kalenterin tietorakenteisiin valmiiksi [luotuja tyyppejä](./utils/types.ts) sekä [utils/tsconfig.json](./utils/tsconfig.json)-tiedostoa, jotka voit yksinkertaisesti kopioida pakettisi sisään. Voit myös katsoa valmiista [testikoodista](./calendar-converter-tests/tests/calendarConverter.test.js), miten funktiosi on tarkoitus lopulta importata ja miten sitä voidaan käyttää.


**CommonJS vs. ES Modules**

Npm-paketteja voidaan toteuttaa eri moduulijärjestelmillä, joista yleisimmät ovat ES Modules ja CommonJS. Node.js tukee molempia, mutta niiden välillä on eroja siinä, miten moduuleja tuodaan (`require` ja `import`) sekä viedään (`export`):

```javascript
// ES Modules
import ical from 'ical-generator';
export default function convertToICalendar() { ... }

// CommonJS
const ical = require('ical-generator');
module.exports = function convertToICalendar() { ... };
```

Valitse jompikumpi tapa ja käytä sitä johdonmukaisesti sekä lähdekoodissa että asetustiedostoissa. [Määrittele `package.json`-tiedostoon, kumpaa moduulijärjestelmää pakettisi käyttää](https://nodejs.org/api/packages.html#type):

```js
{
  "type": "module" // tai "commonjs"
}
```

CommonJS on perinteinen ja erityisesti Node.js-ympäristössä käytetty vaihtoehto, kun taas ES Modules on yleinen etenkin selainympäristössä ja TypeScript-projekteissa.


## Vaihe 4: kalenteridatan sekä päivämäärien käsittely

Perehdy valitsemiisi npm-paketteihin ja hyödynnä niitä kalenteridatan ja päivämäärien käsittelyssä. Muunna JSON-muotoinen kalenteri iCalendar-muotoon käyttäen valitsemiasi työkaluja. Lähdeaineistossa päivämäärät on esitetty "paikallisessa ajassa" ilman tietoa aikavyöhykkeestä sekä kesä- ja talviajasta, joten hyödynnä kirjastoja, lähteitä ja tarvittaessa tekoälyä saadaksesi päivämäärät muunnettua oikeaan muotoon. Esimerkiksi [Luxon-kirjaston `DateTime.fromFormat`-metodi](https://moment.github.io/luxon/#/zones?id=creating-datetimes) voi olla hyödyllinen.

Muodostaessasi iCalendar-kalenteria, käytä apunasi seuraavaa taulukkoa, joka kuvaa JSON-datan ja iCalendar-muodon vastaavuudet:

| JSON           | iCalendar      | Kuvaus                                                            |
|----------------|----------------|-------------------------------------------------------------------|
| event_id       | UID            | Tapahtuman yksilöllinen id                                        |
| subject        | SUMMARY        | Tapahtuman nimi                                                   |
|                | DESCRIPTION    | Vapaavalintainen kuvaus, voi olla sama kuin SUMMARY               |
| location       | LOCATION       | Hyödynnä `name` ja `parent` kenttiä ("Conference Room, Scranton") |
| start_date     | DTSTART        | Aloituspäivämäärä ja -aika muutettuna UTC-muotoon                 |
| end_date       | DTEND          | Lopetuspäivämäärä ja -aika muutettuna UTC-muotoon                 |

Hyödynnä tarpeen mukaan omia yksikkötestejä, `console.log-tulostuksia` tai muita testausmenetelmiä, joilla voit varmistaa, että funktiosi toimii oikein. Hyödynnä myös annettua `example-input.json`-tiedostoa ja vertaa funktiosi tuottamaa iCalendar-muotoa annettuun `example-output.ics`-tiedostoon.

> [!IMPORTANT]
> Vaikka testaamisessa voi olla hyödyksi tulostaa tekstiä konsoliin tai käsitellä tiedostoja, lopullisen paketin ei tule sisältää ylimääräisiä tulostuksia tai tiedostokäsittelyjä.


## Vaihe 5: paketin valmistelu

Jotta pakettisi olisi ulkopuolisten käyttäjien hyödyttävissä, sinun tulee määritellä `package.json`-tiedostoon muutamia lisäkenttiä. Lisäksi, jos käytät TypeScriptiä, sinun tulee kääntää TypeScript-lähdekoodisi JavaScriptiksi ennen paketin julkaisua ja käyttöönottoa.

**`files` ja `.npmignore`**

Projektisi sisältää tyypillisesti tiedostoja, joita ei ole syytä julkaista loppukäyttäjille:

> *"Publishing sensitive information to the registry can harm your users, compromise your development infrastructure, be expensive to fix, and put you at risk of legal action."*
>
> [Creating and publishing scoped public packages (docs.npmjs.com)](https://docs.npmjs.com/creating-and-publishing-scoped-public-packages#reviewing-package-contents-for-sensitive-or-unnecessary-information)

Pois jätettäviä tiedostoja voivat olla esimerkiksi testit, konfiguraatiotiedostot, ympäristömuuttujat ja kehitystyössä käytettävät skriptit. Määrittele `package.json`-tiedostoon [uusi `files`-kenttä, joka sisältää listan tiedostoista ja hakemistoista](https://docs.npmjs.com/cli/commands/npm-publish#files-included-in-package), jotka haluat sisällyttää pakettiisi. Näin varmistat, että asennettava paketti ei sisällä esimerkiksi TypeScript-lähdekoodeja ja konfiguraatiota, vaan pelkästään käännetyt tiedostot.

Npm huomioi oletuksena `.gitignore`-tiedoston, jos se sijaitsee samassa hakemistossa kuin `package.json`, mutta voit käyttää myös `.npmignore`-tiedostoa, jos haluat määritellä erikseen, mitkä tiedostot jätetään pois paketista.

**`main` ja `types`**

Määrittele `package.json`-tiedostoon [`main`-kenttä](https://docs.npmjs.com/cli/configuring-npm/package-json#main), joka osoittaa pakettisi pääasialliseen JavaScript-tiedostoon. Tämä on tiedosto, joka ladataan, kun joku käyttää pakettiasi `require`- tai `import`-lauseella.

> *"For most modules, it makes the most sense to have a main script and often not much else."*
>
> https://docs.npmjs.com/cli/configuring-npm/package-json#main

Jos käytät TypeScriptiä, `"main"`-tiedoston tulee viitata valmiiksi käännettyyn JavaScript-tiedostoon, eikä TypeScript-tiedostoon. Määrittele lisäksi [kenttä `"types"`](https://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html), joka osoittaa lähdekoodeistasi generoituun tyyppitiedostoon (esim. `index.d.ts`). Tämä mahdollistaa TypeScript-käyttäjien hyödyntää pakettisi tyyppejä omissa projekteissaan.


## Vaihe 6: readme.md ja paketointi

Npm-paketin yhteyteen on tärkeää lisätä `readme.md`-tiedosto, joka sisältää perustiedot paketista, sen asennuksesta ja käytöstä. Tämä tiedosto näytetään automaattisesti npm-rekisterissä, mikäli julkaiset pakettisi sinne. Lisää `calendar-converter`-hakemistoon `readme.md`-tiedosto, joka sisältää vähintään paketin nimen ja lyhyen kuvauksen paketista.

Kun projektisi on valmis paketoitavaksi, käytä [`npm pack`-komentoa](https://docs.npmjs.com/cli/commands/npm-pack). `npm pack` luo paketin sisällön perusteella `.tgz`-paketin, joka sisältää kaikki ne tiedostot, jotka määrittelit `package.json`-tiedostossa `"files"`-kentässä. `npm pack`-komennon tarkoituksena on simuloida, miltä pakettisi näyttää, jos julkaiset sen npm-rekisteriin.

Suorita siis `npm pack`-komento pakettisi hakemistossa:

```sh
cd calendar-converter
npm pack
```

Varmista, että komento loi hakemistoon tiedoston, jonka nimi on muotoa `example-calendar-converter-X.Y.Z.tgz`. Nimessä `X.Y.Z` on versionumero, jonka määrittelit `package.json`-tiedostossa. Jos paketin nimi ei täsmää, tarkista `package.json`-tiedoston kentät `"name"` ja `"version"`.


## Vaihe 7: paketin testaaminen

> *"To reduce the chances of publishing bugs, we recommend testing your package before publishing it to the npm registry."*
>
> [Creating and publishing scoped public packages (docs.npmjs.com)](https://docs.npmjs.com/creating-and-publishing-scoped-public-packages#testing-your-package)

Ennen kuin julkaiset pakettisi muille kehittäjille, on tärkeää testata, että se toimii oikein. Tyypillisesti paketin testausta varten luodaan erillinen projekti, jossa paketti asennetaan paikallisesti ja testataan sen toimivuutta. Niin toimitaan myös tässä harjoituksessa.

Tässä repositoriossa on valmiina kansio nimeltä [`calendar-converter-tests`](./calendar-converter-tests/), joka sisältää erillisen Node.js-projektin, joka testaa kirjoittamasi npm-paketin toimintaa. Testit on toteutettu [Vitest-testaustyökalulla](https://vitest.dev/).

Testataksesi pakettisi asennusta, siirry testihakemistoon ja asenna edellisessä vaiheessa luomasi `tgz`-paketti sinne:

```bash
# siirry repositorion juuresta testihakemistoon:
cd calendar-converter-tests

# korvaa vesionumero X.Y.Z oman pakettisi versiolla:
npm install ../calendar-converter/example-calendar-converter-X.Y.Z.tgz
```

Varmista, että asennus onnistuu ilman virheitä. Tämän jälkeen tarkasta, että testiprojektin `node_modules`-hakemistoon on luotu `@example/calendar-converter`-hakemisto, joka sisältää pakettisi julkaistuksi tarkoitetut tiedostot:

```bash
ls node_modules/@example/calendar-converter
```

Asenna lisäksi testiprojektiin ennalta määritellyt riippuvuudet (Vitest) `npm install`-komennolla:

```bash
# calendar-converter-tests -hakemistossa:
npm install
```

Kun riippuvuudet on asennettu, suorita testit `npm test`-komennolla:

```bash
# calendar-converter-tests -hakemistossa:
npm test
```

Löydät tarkemmat ohjeet testiprojektin käyttämisestä [`calendar-converter-tests/readme.md`-tiedostosta](./calendar-converter-tests/readme.md).

Jos testit eivät syystä tai toisesta mene läpi, tutki saamaasi virheilmoitusta. Tee tarvittavat korjaukset ja toista vaiheet 5-7, kunnes testit menevät läpi.

Kun olet tehnyt muutoksia pakettisi koodiin, kasvata sen versionumeroa `npm version patch` -komennolla. Tämän jälkeen paketoi se uudestaan `npm pack` -komennolla. Poista edellinen versio testiprojektitsa komennolla `npm uninstall @example/calendar-converter` ja asenna uusi versio `npm install ...`-komennolla.


## Vaihe 8: ratkaisun lähettäminen GitHubiin

Tarkista, että tekemäsi tgz-paketti löytyy `calendar-converter`-hakemistosta ja että se on mukana commitissa. Varmista myös, että `calendar-converter-tests`-hakemiston `package.json`-tiedoston muutokset on commitoitu:

```
git status
git add <muutetut ja lisätyt tiedostot>
git commit -m "<sopiva viesti>"
git push
```

Tyypillisesti `tgz`-paketti jätetään versionhallinnan ulkopuolelle, mutta tässä harjoituksessa se tulee automaattisen arvioinnin vuoksi sisällyttää.

Push-komennon jälkeen tarkista GitHubista, että tekemäsi muutokset näkyvät repositoriossa. Varmista myös actions-välilehdeltä, että automaattinen arviointi tuottaa odotetun lopputuloksen.


## Tietoa harjoituksesta

Tämän tehtävän on kehittänyt Teemu Havulinna ja se on lisensoitu [Creative Commons BY-NC-SA -lisenssillä](https://creativecommons.org/licenses/by-nc-sa/4.0/).

Tehtävänannon, lähdekoodien ja testien toteutuksessa on hyödynnetty ChatGPT-kielimallia sekä GitHub copilot -tekoälyavustinta.
