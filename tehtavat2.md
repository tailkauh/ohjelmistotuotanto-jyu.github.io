---
layout: page
title: Osa 2
inheader: no
permalink: /tehtavat2/
---


Tehtävissä tutustutaan koodin _staattiseen analyysin_ Pylint-työkalun avulla, _riippuvuuksien injektointiin_ ja jatketaan Gitiin tutustumista.

### Typoja tai epäselvyyksiä tehtävissä?

{% include typo_instructions.md %}

{% include norppa.md %}

{% include poetry_ongelma.md %}

### 1. Pylint ja koodin staattinen analyysi

**Tämä ja seuraava tehtävä tehdään viime viikon tehtävissä käytettyyn ohtuvarasto-repositorioon**

Kurssin [kolmannessa osassa](/osa3) teemana on ohjelmien laadun varmistaminen. Eräs ohjelman laatua useimmiten edistävä tekijä on järkevän _koodityylin_ noudattaminen. Koodin tyyliä voidaan tarkkailla automatisoidusti niin sanottujen staattisen analyysin työkaluilla.

Tutustutaan nyt staattisen analyysin työkaluun nimeltään [Pylint](https://pylint.pycqa.org/en/latest/index.html). Ennen kuin syvennymme aiheeseen, tutustu pylintin käyttöön lukemalla seuraava [Pylint-ohje](/pylint).

**Mene nyt edellisen viikon varasto-projektiin liittyvien tehtävien palautusrepositorioosi.**

Ota varasto-projektissa käyttöön Pylint noudattamalla lukemiasi ohjeita. Konfiguraationa käytettävän _.pylintrc_-tiedoston sisältö tulee toistaiseksi olla [tämän]({{site.python_exercise_repo_url}}/blob/main/osa2/varasto/.pylintrc) tiedoston sisällön mukainen.

Pylintin tarkistamat säännöt konfiguroidaan _.pylintrc_-tiedostoon oikeiden osioiden alle. `[main]`-osio sisältää yleistä konfiguraatio, kuten mitkä hakemistot tai tiedostot pitäisi jättää tarkistuksien ulkopuolelle. `[MESSAGE CONTROL]`-osiossa taas voidaan määritellä esimerkiksi tarkistuksia, joista ei tarvitse huomauttaa. Loput osiot ovat eri sääntöjen konfigurointia varten, jotka on dokumentoitu pylintin [dokumentaatiossa](http://pylint.pycqa.org/en/v3.0.2/technical_reference/features.html). Jos haluamme esimerkiksi asettaa funktioiden ja metodien argumenttien maksimilukumäärään neljään, voimme lisätä sen `[DESIGN]`-osioon seuraavasti:

```
[DESIGN]

max-args=4
```

Helpoin tapa löytää sääntöjä on hakemalla sopivalla hakusanalla niitä dokumentaatiosta tai Googlettamalla. Oikean osion löytää dokumentaatiosta (esimerkiksi `max-args`-sääntö löytyy dokumentaatiosta [Design checker](https://pylint.pycqa.org/en/v3.0.2/user_guide/configuration/all-options.html#design-options) -osion alta).

**Toimi nyt seuraavasti:**

- Siirry virtuaaliympäristöön komennolla `poetry shell` ja suorita sen sisällä komento `pylint src`. Jos tarkistuksissa löytyy virheitä, korjaa ne
- Määrittele nyt tiedostoon _.pylintrc_ seuraavat säännöt (katso lista säännöistä pylintin [dokumentaatiosta](http://pylint.pycqa.org/en/v3.0.2/technical_reference/features.html)):

  - Rivin pituus on maksimissaan 80 merkkiä
    - Vinkki: sääntö löytyy [Format checker](https://pylint.pycqa.org/en/v3.0.2/user_guide/configuration/all-options.html#format-options) -osiosta ja tulee määrittää `[FORMAT]`-osion alle
  - Ei yli kahta sisäkkäistä lohkoa (esimerkiksi if- tai for-lohkoa) funktion tai metodin sisällä
    - Vinkki: sääntö löytyy [Refactoring checker](https://pylint.pycqa.org/en/v3.0.2/user_guide/configuration/all-options.html#refactoring-options) -osiosta ja tulee määrittää `[REFACTORING]`-osion alle)
  - Funktiossa tai metodissa on enintään 15 lausetta (statements), etsi sääntö dokumentaatiosta
  - Määrittele myös jokin itse valitsemasi, mielenkiintoiselta/hyödylliseltä kuulostava sääntö

- Muuta koodiasi siten, että saat jokaisen määritellyistä pylint-säännöistä rikkoutumaan
- Korjaa koodisi ja varmista, että se noudattaa kaikkia sääntöjä

Usein _.pylintrc_-konfiguraatiota ei ole järkevää kirjoittaa tyhjästä käsin, vaan käytetään lähtökohtana pylintin suosittelemaa konfiguraatiota. Suositellun konfiguraation voi tulostaa komentoriville komennolla `pylint --generate-rcfile`.

### 2. Koodin staattinen analyysi ja GitHub Actionit

**Tämä tehtävä tehdään edellisen viikon tehtävissä käytettyyn ohtuvarasto-repositorioon**

Laajenna ohtuvarastosi GitHub Actionien määritelmää siten, että myös Pylint-tarkastukset suoritetaan aina kun koodi pushataan GitHubiin.

Varmista, että GitHub huomaa tilanteen, missä koodi rikkoo projektin Pylint-sääntöjä:

![]({{ "/images/py-lh2-11.png" | absolute_url }})

Varmista myös, että kun korjaat koodin, kaikki toimii taas moitteettomasti:

![]({{ "/images/py-lh2-12.png" | absolute_url }})


### 3. Palautusrepositorion luominen

Seuraavat tehtävät palautetaan eri repositorioon, kuin aikaisemmissa tehtävissä käytetty ohtuvarasto. Luo siis ny **uusi repositorio**.

Nyt luotavan palautusrepositorion rakenne voi olla esimerkiksi seuraava:

```
osa2
  riippuvuuksien-injektointi-1
  nhl-statistics-1
  poetry-web
  project-reader
  nhl-reader
osa3
  login-robot
...
```

Tätä tehtävää ei palaute tai arvostella.

### 4. Riippuvuuksien injektointi osa 1

**Tämä tehtävä tehdään juuri luomaasi uuteen palautusrepositorioon.**

- tehtävässä ei tosin tehdä itse mitään koodia...

Tutustumme kurssin aikana muutamiin _suunnittelumalleihin_ (engl. design pattern), eli hyviksi tunnettuihin useisiin erilaisiin tilanteisiin sopiviin ratkaisutapoihin, joiden soveltaminen usein parantaa koodin laatua.

Kurssin ensimmäinen suunnittelumalli _riippuvuuksien injektointi_ (engl. dependency injection), on yksinkertainen periaate, jota noudattamalla koodin automatisoitua testaamista on monissa tilanteissa mahdollista helpottaa ratkaisevalla tavalla.

- Tutustu riippuvuuksien injektointiin lukemalla [tämä dokumentti](/riippuvuuksien_injektointi_python/)
- Hae esimerkkiprojekti kurssin [tehtävärepositorion]({{site.python_exercise_repo_url}}) hakemistosta _osa2/riippuvuuksien-injektointi-1_ ja kokeile että se toimii
  - Järkevintä lienee että kloonaat repositorion paikalliselle koneellesi
  - **Tämän jälkeen kannattaa kopioida projekti edellisessä tehtävässä luomasi palautusrepositorion sisälle**
  - **HUOM** lue 15 cm ylempää miten koodi kannattaa organisoida palautusrepositorion sisälle

Tutustu riippuvuuksien injektointiin esimerkin avulla. Asenna projektin riippuvuudet sen juurihakemistossa (eli hakemistossa missä tiedosto _pyproject.toml_ sijaitsee) komennolla `poetry install`. Tämän jälkeen saat suoritettua koodin virtuaaliympäristön sisällä komennolla `python3 src/index.py`. Voit myös halutessasi suorittaa testit virtuaaliympäristön sisällä komennolla `pytest`. Jos unohtui miten virtuaaliympäristön sisälle päästään, kertaa asia viime viikon tehtävistä.

### 5. Riippuvuuksien injektointi osa 2: NHL-tilastot

**Myös Tämä tehtävä tehdään juuri luomaasi palautusrepositorioon**

- Kurssin [tehtävärepositorion]({{site.python_exercise_repo_url}}) hakemistossa _osa2/nhl-statistics-1_ on ohjelma, jonka avulla on mahdollista tutkia <https://nhl.com>-sivulla olevia tilastotietoja (vaihtamalla sovelluksen käyttämää URL:ia, voit katsoa eri kausien tilastoja)
  - Asenna projektin riippuvuudet suorittamalla sen juurihakemistossa komento `poetry install`
- Ohjelma koostuu kolmesta luokasta.
  - `StatisticsService` on palvelun tarjoava luokka, se tarjoaa metodit yhden pelaajan tietojen näyttämiseen, pistepörssin näyttämiseen ja yhden joukkueen pelaajien tietojen näyttämiseen
  - `Player` on luokka, jonka olioina `StatisticsService`-luokka käsittelee yksittäisen pelaajan tietoja
  - `PlayerReader` on luokka, jonka avulla ohjelma käy hakemassa pelaajien tiedot internetistä
- Ohjelma on nyt ikävästi struktoroitu ja esim. yksikkötestaus on kovin hankalaa

**Itse tehtävä:**

- Muokkaa ohjelman rakennetta siten, että `StatisticsService`-luokka saa konstruktoriparametrina `PlayerReader`-luokan olion, ja että `PlayerReader` saa konstruktoriparametrina osoitteen mistä se hakee pelaajien tiedot
- Muokkaa pääohjelma siten, että se injektoi `StatisticsService`-oliolle `PlayerReader`-luokan olion (jolle on annettu konstruktoriparametrina haluttu osoite) ja kokeile että ohjelma toimii edelleen:

```python
stats = StatisticsService(
  PlayerReader("https://studies.cs.helsinki.fi/nhlstats/2022-23/players.txt")
)
```

**HUOM:** jos törmäät virheeseen `URLError: <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed`, mahdollinen ratkaisu ongelmaan löytyy [täältä](https://stackoverflow.com/a/42334357).


### 6. Poetryn harjoittelua

**Myös Tämä tehtävä tehdään juuri luomaasi palautusrepositorioon, eli EI KÄYTETÄ viime viikon ohtuvarasto-repositoriota.**

- Tee palautusrepositorioon hakemisto _osa2_ ja sen sisälle hakemisto _poetry-web_ tätä tehtävää varten

{% include no_pip2.md %}

Tässä tehtävässä harjoittelemme lisää Poetryn käyttöä ja tutustumme semanttiseen versiointiin. Apua tehtävän tekoon saa mm. [Poetry-ohjeesta](/poetry) ja [Poetryn dokumentaatiosta](https://python-poetry.org/docs/).

Kuvitellaan tilanne, jossa työskentelet ohjelmistokehittäjänä kehitystiimissä, joka on alkamassa kehittämään web-sovellusta. Olette päätyneet kehittämään sovelluksen Pythonilla ja käyttämään Poetrya riippuvuuksien hallinnassa.

Tee seuraavat toimenpiteet:

- Aluksi Poetry-pohjainen projekti täytyy alustaa. **Alusta projekti Poetryn avulla _poetry-web_ nimiseen hakemistoon tehtävien palautukseen käyttämäsi repositorion hakemiston viikko2 sisälle**. Muista käyttää alustuksessa komentoa `poetry init --python "^3.10"`, jotta projektin Python-version vaatimus asetetaan oikein.
- Etsit Googlettamalla sopivia kirjastoja web-sovellusta varten ja törmäät [Flask](https://pypi.org/project/Flask/)-viitekehykseen. **Asenna Flask projektin riippuvuudeksi Poetryn avulla**
- Sovelluksessa ilmenee ensimmäinen bugi. Syynä oli luultavasti se, ettei sovellukselle ole toteutettu vielä yhtään testiä. Päädyt käyttämään testauksessa [pytest](https://pypi.org/project/pytest/)-viitekehystä. **Asenna pytest projektin _kehityksen aikaiseksi riippuvuudeksi_**
  - Pohdi itseksesi, miksi on hyödyllistä määritellä riippuvuus erikseen kehityksen aikaiseksi riippuvuudeksi
- Sovelluksessa käsitellään paljon JSON-muotoista dataa, joten päädyt etsimään sen serialisointiin ja deserialisointiin sopivia kirjastoja. Törmäät tarkoitukseen sopivaan kirjastoon nimeltä [jsonpickle](https://pypi.org/project/jsonpickle/). **Asenna jsonpickle projektin riippuvuudeksi**
- Huomaat bugin jsonpickle-kirjastossa, joten alat tutkimaan sen GitHub repositorion [issueita](https://github.com/jsonpickle/jsonpickle/issues). Eräässä issuessa kerrotaan, että löytämäsi bugi ei ilmene kirjaston versiossa `1.3.0`. **Asenna jsonpickle-kirjastosta versio `1.3.0`**.
  - Tutustu _semanttiseen versiointiin_ [täällä](https://semver.org/)
  - Pohdi itseksesi, mitä hyötyjä semanttisesta versioinnista on. Jos kirjasto noudattaa semanttista versiointia, miksi kirjaston version `1.1.2` päivittäminen versioon `2.0.0` saattaa sisältää riskejä? Miksei samoja riskejä luultavasti ole version `1.1.3` kanssa?
  - Versiovaatimuksissa on mukana usein `^`-, tai `~`-etuliite. Pohdi itseksesi, mitä näillä ilmaistaan. Asiaa käsitellään mm. [Poetryn dokumentaatiossa](https://python-poetry.org/docs/dependency-specification/)
- Päätät, että jsonpickle-kirjastosta on ollut vain harmia ja voit helposti toteuttaa sen tarjoaman toiminallisuuden itse. **Poista jsonpickle projektin riippuvuuksien joukosta**

Palautettavasta _poetry-web_-hakemistosta ei tarvitse löytyä muita tiedostoja kuin _pyproject.toml_ ja _poetry.lock_.

### Bonus: VS coden konfigurointi

**HUOM:** VS coden konfigurointi on täysin vapaaehtoista, selviät aivan hyvin ilman konfigurointeja. Kaikilla konfigurointi ei mene täysin alla kuvatun kaltaisesti. Eli jos konfigurointi ei onnistu, siirry eteenpäin.
 
Palaa vielä edellisen tehtävän projektiin, ja asenna projektiin kirjasto `cowsay`.

Luo projektiin hakemisto _src_ ja sen sisälle tiedosto _index.py_, jolla on seuraava sisältö

```py
import cowsay

cowsay.cow('Laitetaan VS Code toimimaan kunnolla!')
```

Koodi toimii komentoriviltä suoritettaessa mutta VS Codesta käsin suorittaminen ei onnistu:

![]({{ "/images/lh2-vscode1.png" | absolute_url }})

Kuten virheilmoitus kertoo, koodin suorittaminen VS Coden "play"-napilla ei onnistu sillä kirjastoa ei löydy. Komennon _import_ punainen alleviivaus kertoo myös mistä on kyse, VS Code ei löydä kirjastoa _cowsay_. Asia saadaan korjattua muutamalla toimenpiteellä:

- Lisää projektin sisältävä hakemisto VS Coden "workspaceen" valikosta _file_ valitsemalla _Add Folder to Workspace_.
- Tallenna workspace valikosta _Save workspace as_. Tee tallennus projektin hakemiston juureen (eli samaan paikkaan missä tiedosto pyproject.toml) on.
- Valitse oikea Pythonin versio. Klikkaa editorin alareunasta:
![]({{ "./images/lh2-vscode2.png" | absolute_url }})
- ... ja tee valinta
![]({{ "./images/lh2-vscode3.png" | absolute_url }})

Näiden toimenpiteiden jälkeen voit suorittaa koodin VS Coden "play"-napilla:

![]({{ "./images/lh2-vscode4.png" | absolute_url }})

Oikein konfiguroitu VS Code mahdollistaa myös sen sisäänrakennetun debuggerin käytön. Jos debuggeri ei ole vielä tuttu, [täältä](https://ohjelmointi-23.mooc.fi/osa-4/1-vscode#debuggeri) ohje sen käyttöön.

Workspacen konfigurointi luo tiedoston _poetry-web.code-workspace_ jonka sisältä seuraava

```json
{
	"folders": [
		{
			"path": "."
		}
	],
	"settings": {}
}
```

Konfiguroinnin voi tehdä myös luomalla kyseisen sisältöisen tiedoston suoraan projektin alle. Pythonin oikea versio on kuitenkin ehkä valittava itse. Komennon _poetry install_ tulee myös olla suoritettuna jotta kaikki toimisi.

### 7. Riippuvuuksien hyödyntäminen


Ohjelmistokehittäjälle tulee usein vastaan tilanne, jossa pitäisi löytää tiettyyn käyttötarkoitukseen sopiva kirjasto. Harjoittelemme kyseistä tilannetta tässä tehtävässä.

[TOML](https://toml.io/en/) on eräs helppolukuinen datan esitysformaatti, jota käytetään usein konfiguraatiotiedostoissa, kuten Poetryn _pyproject.toml_-tiedostossa. [Kurssirepositorion]({{site.python_exercise_repo_url}}) hakemistosta _osa2/project-reader_ on pohja ohjelmalle, jonka tarkoituksena on lukea projektin tietoja annetusta osoitteesta löytyvästä _pyproject.toml_-tiedostosta.

- **Kopioi aluksi projekti palautusrepositorioon hakemiston osa2 sisälle.**

Tehtävänäsi on ensin löytää sopiva kirjasto, jonka avulla TOML-muotoisista merkkijonoista voi muodostaa Pythonin tietorakenteita. Voit hyödyntää tässä esimerkiksi [PyPI](https://pypi.org/)-sivuston hakua tai Googlea. PyPI:ssä eräs hyvä hakusana voisi olla esimerkiksi "toml". Tutustu kirjastojen kuvauksiin ja päättele sen perusteella, sopiiko kirjasto käyttötarkoitukseen. Kun löydät sopivan kirjaston, asenna se projektiin Poetryn avulla.

**HUOM:** PyPI:n asennusohjeista löytyy usein pip-asennuksen ohje `pip install <kirjasto>`. Kaikki kirjastot pystyy kuitenkin asentamaan yhtä lailla Poetryn avulla komennolla `poetry add <kirjasto>`.

Ota sen jälkeen kirjasto käyttöön projektin <i>src/project_reader.py</i>-tiedoston `ProjectReader`-luokan metodissa `get_project`. Metodin `content`-muuttujaan on tallennettu tiedoston sisältö:

```python
def get_project(self):
    # tiedoston merkkijonomuotoinen sisältö
    content = request.urlopen(self._url).read().decode("utf-8")
    print(content)

    # deserialisoi TOML-formaatissa oleva merkkijono ja muodosta Project-olio sen tietojen perusteella
    return Project("Test name", "Test description", [], [])
```

Tulosta jokainen välivaihe (tiedoston sisältö ja kirjaston avulla deserialisoitu sisältö) `print`-funktion avulla, jotta tiedät, minkä muotoista data on. Toki voit käyttää samaan tarkoitukseen myös VS Coden [debuggeria](https://ohjelmointi-23.mooc.fi/osa-4/1-vscode#debuggeri).

Muodosta tämän jälkeen tiedoista `Project`-olio antamalla sille konstruktorin kautta projektin nimi, kuvaus, lista riippuvuuksista ja lista kehityksen aikaisista riippuvuuksista. Kun ohjelma toimii halutulla tavalla, voit poistaa debuggauksessa käytetyt tulostukset.

Ohjelman voi käynnistää virtuaaliympäristössä komennolla `python3 src/index.py`. Esimerkkinä käytetyn _pyproject.toml_-tiedoston tapauksessa ohjelman tulostuksen tulisi olla seuraava:

```
Name: Ohtutesting app
Description: Sovellus joka toimii testisyötteenä ohtun osan 2 laskareihin
Dependencies: python, Flask, editdistance
Development dependencies: coverage, robotframework, robotframework-seleniumlibrary, requests
```

*HUOM* ohjelma ei saa sisältää kuin ainoastaan tiedostossa index.py olevan print-komennon, joka tuostaa `Project`-olion merkkijonoesityksen!

Laajenna ja hio vielä ratkaisua siten, että esimerkkiprojektin osalta lopputulos näyttää suunilleen seuraavalta

```
Name: Ohtutesting app
Description: Sovellus joka toimii testisyötteenä ohtun osan 2 laskareihin
License: MIT

Authors:
- Matti Luukkainen
- Kalle Ilves

Dependencies:
- python
- Flask
- editdistance

Development dependencies:
- coverage
- robotframework
- robotframework-seleniumlibrary
- requests
```

** Loput tämän viikon tehtävistä ovat vapaaehtoisia, eikä niitä arvostella. Tehtäviin kannattaa kuitenkin tutustua, erityisesti, jos branchien käyttö Gitissä on yhtään vierasta **

### 8. Git: branchit [versionhallinta]

**Tätä tehtävää ei palauteta mihinkään**

Lue brancheja käsittelevät osuudet seuraavasta <https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging>

- jos haluat lukea hieman perusteellisemman selityksen asiasta, lue <https://git-scm.com/book/en/v2:n> luku kolme kokonaisuudessaan

Kannattaa huomioida myös erittäin hyvä brancheja käsittelevä visuaalinen materiaali osoitteessa <https://learngitbranching.js.org/>

Varsin selkeältä vaikuttaa myös <https://www.atlassian.com/git/tutorials/using-branches>

**Huom:** kun liikut branchien välillä kannattaa pitää working tree ja staging -alue tyhjinä!

Tee seuraavat paikalliseen Git-repositorioosi (tehtävää ei palauteta, eli kyseessä **ei** siis tarvitse olla tehtävien palautusrepositorio)

- Luo repositorio ja committaa mainiin tiedosto **index.py** jonka sisältö on seuraava

```python
x = int(input("luku 1: "))
y = int(input("luku 2: "))
```

- Luo branch **laskut**, siirry branchiin (tämä tapahtuu esim. komennolla `git checkout -b laskut`), luo sinne tiedosto **summa.py** jolla on seuraava sisältö

```python
def summa(x, y):
    return x+y
```

- lisää ja committaa tiedosto versionhallintaan

- Siirry takaisin **main**-branchiin (komennolla`git checkout main`), tiedoston **summa.py** ei pitäisi nyt näkyä
  - **huom:** muistutus vielä siitä, että kun siirryt branchista toiseen varmista **aina** komennolla `git status` että kaikki muutokset on committoitu
- Luo tiedosto **logger.py**, jolla on seuraava sisältä

```python
from datetime import datetime

def logger(viesti):
  print(f"{datetime.now()}: {viesti}")
```

- Muuta myös tiedostoa **index.py** seuraavasti:

```python
from logger import logger

logger("aloitetaan")

x = int(input("luku 1: "))
y = int(input("luku 2: "))

logger("lopetetaan")
```

- Committaa nämä muutokset **main**-haaraan

- Mene branchiin **laskut** ja tarkasta, että **mainiin** lisätty tiedosto ei ole branchissa ja että tiedostoon **index.py** tehty muutos ei näy
- Lisää ja committaa branchiin tiedosto **erotus.py** jolla on seuraava sisältö

```python
def erotus(x, y):
    return x-y
```

- Siirry takaisin **main**-branchiin
- Tarkasta että **laskut**-branchiin lisätyt muutokset eivät ole mainissa
- Tarkastele komennolla `gitk --all` miltä repositorio ja branchit näyttävät (`gitk`-komento toimii Windowsilla ainakin GitHub for Windowsin Git Shellissä.)
  - Saat asennettua Maciin `gitk`:n [tämän ohjeen](https://www.geekbitzone.com/posts/git/gitk-for-macos/) avulla
    - jos asennus ei onnistu, on hyvä korvaaja gitk:lle [sourcetree](https://www.sourcetreeapp.com)
- Mergeä branchin **laskut** sisältö **mainiin** (tämä tapahtuu komennolla`git merge laskut`)
  - Mergeäminen aiheuttaa ns merge-commitin, ja avaa tekstieditorin mihin joudut kirjoittamaan commit-viestin
    - Jos et ole määritellyt gitille editoria viime viikon [tehtävän 2](/tehtavat1/) ohjeiden mukaan, avautuu ehkä gitin oletusarvoinen editori [vim](http://www.vim.org)
    - Vimistä poistuminen saattaa osoittautua ensikertalaiselle hankalaksi, Google auttaa tarvittaessa
- Muuta tiedostoa **index.py** seuraavasti ja commitoi muutos:

```python
from logger import logger
from summa import summa
from erotus import erotus

logger("aloitetaan")

x = int(input("luku 1: "))
y = int(input("luku 2: "))
print(f"{summa(x, y)}")
print(f"{erotus(x, y)}")

logger("lopetetaan")
```

- Katso jälleen miltä näyttää `gitk --all`-komennolla

### 9. Git: branchit ja staging-alue [versionhallinta]

**Tätä tehtävää ei palauteta mihinkään**

- Olet nyt repositoriosi main-haarassa
- Luo uusi tiedosto _README.md_, **älä** kuitenkaan lisää ja commitoi tiedostoa versionhallintaan
- Tiedoston sisällöllä ei ole merkitystä, se voi olla esim. seuraava

```
## git-harjoituksia

Harjoitellaan branchien käyttöä
```

- Komennon `git status` tulostuksen pitäisi olla seuraava

```
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	README.md

nothing added to commit but untracked files present (use "git add" to track)
```

- Siirry nyt branchiin **laskut**
- Suorita uudelleen komento `git status`
- Huomaat, että tulostus on edelleen sama, tiedosto ei edelleenkään ole versionhallinnan alla
- Eli vaikka olit main-haarassa kun loit tiedoston, ei main-haara eikä koko git tiedä tiedostosta vielä mitään ennen kuin lisäät sen versionhallinnan alaisuuteen komennolla `git add`
- Lisää tiedosto nyt versionhallinnan alaisuuteen ja commitoi se
- Tiedosto menee nykyiseen branchiisi, eli branchiin _laskut_, main ei edelleenkään tiedä tiedostosta mitään
- Luo uusi tiedosto _LICENSE_ ja lisää se versionhallintaan (komennolla add), älä kuitenkaan commitoi
- Tiedoston sisällöllä ei ole merkitystä, se voi olla esim. seuraava

```
This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or
distribute this software, either in source code form or as a compiled
binary, for any purpose, commercial or non-commercial, and by any
means.

For more information, please refer to <https://unlicense.org>
```

- Tarkasta että komennon `git status` tulos on seuraava:

```
On branch laskut
Changes to be committed:
   (use "git restore --staged <file>..." to unstage)

	new file:   LICENCE
```

- Olet siis branchissa _laskut_ ja _LICENSE_ on lisätty staging-alueelle, sitä ei kuitenkaan ole vielä committoitu
- Siirry nyt branchiin **main**
- Komennon `git status`tulos on edelleen sama, _LICENSE_ on edelleen staging-alueella mutta committoimattomana
- Staging-alue **ei kuulu** mihinkään branchiin, eli jos staging-alueella on committoimattomia muutoksia ja vaihdat branchia, säilyvät samat asiat stagingissa
- Muutokset siirtyvät stagingista branchiin ainoastaan komennolla `git commit`
- Committoi nyt staging-alueen muutokset eli _LICENSE_ mainiin
- Komennon `git status` tulos kertoo nyt että staging-alue on tyhjä:

```
On branch main
nothing to commit, working tree clean
```

- Siirry jälleen branchiin **laskut** ja huomaat, että _LICENSE_ ei ole olemassa
- Mergeä **main**-branch branchiin **laskut**
- Siirry nyt takaisin branchiin **main** ja tuhoa branchi **laskut**
  - Tuhoaminen ei onnistu suoraan komennolla `git branch -d`, jos branchin sisältö ei ole kokonaisuudessan mergetty mainiin. Jos näin on, tee ensin merge mainiin, tai jos tarkoituksena on poistaa branch silti vaikka siinä on vielä eriäviä muutoksia, käytä `git branch -D` poistaaksesi branch eriävine muutoksineen
- Tämän tehtävän ideana oli siis havainnollistaa, että working tree (muutokset joista Git ei ole tietoinen) ja staging (gitiin lisättyihin tiedostoihin tehdyt committoimattomat muutokset)
  **eivät liity** mihinkään branchiin, muutokset siirtyvät staging-alueelta branchiin ainoastaan komennon `git commit` suorituksen seurauksena

### 10. Git: konflikti! [versionhallinta]

**Tätä tehtävää ei palauteta mihinkään**

Tee paikalliseen Git-repositorioon seuraavat

- Muuta **main**-branchin tiedostoa **index.py** seuraavasti:

```py
# tehdään alussa importit

from logger import logger
from summa import summa
from erotus import erotus

logger("aloitetaan")

x = int(input("luku 1: "))
y = int(input("luku 2: "))
print(f"{summa(x, y)}")
print(f"{erotus(x, y)}")

logger("lopetetaan")
```

- alkuun on siis lisätty kommentti ja tyhjä rivi
- committaa muutos

- Tee uusi branchi **bugikorjaus**, mene branchiin ja editoi tiedoston **index.py** loppua (esim. seuraavasti ) ja committaa

```py
# tehdään alussa importit

from logger import logger
from summa import summa
from erotus import erotus

logger("aloitetaan")

x = int(input("luku 1: "))
y = int(input("luku 2: "))
print(f"{summa(x, y)}")
print(f"{erotus(x, y)}")

logger("lopetetaan ohjelma")
print("goodbye!") # lisäys bugikorjaus-branchissa
```

- Mene takaisin **main**-branchiin, editoi tiedoston **index.py** alkupuolta esim.seuraavasti (muutos on funktion logger parametrissa) ja committaa muutokset:

```py
# tehdään alussa importit

from logger import logger
from summa import summa
from erotus import erotus

logger("aloitetaan ohjelma") # muutos mainissa

x = int(input("luku 1: "))
y = int(input("luku 2: "))
print(f"{summa(x, y)}")
print(f"{erotus(x, y)}")

logger("lopetetaan ohjelma")
```

- Mergeä branchin **bugikorjaus** sisältö **mainiin**

  - Katso tiedoston **index.py**-sisältöä, sen pitäisi sisältää nyt molemmissa brancheissa tehdyt muutokset
  - **Huom:** jo tässä vaiheessa saattaa syntyä konflikti jos olet vahingossa muuttanut merkkejä väärästä kohtaa tiedostoa! Toimi tällöin ao. ohjeen mukaan.

- Olet edelleen branchissa **main**. Muuta tiedostoa print-komentojen osalta seuraavasti

```py
# tehdään alussa importit

from logger import logger
from summa import summa
from erotus import erotus

logger("aloitetaan ohjelma")

x = int(input("luku 1: "))
y = int(input("luku 2: "))
print(f"{x} + {y} = {summa(x, y)}") # muutos mainissa
print(f"{x} - {y} = {erotus(x, y)}") # muutos mainissa

logger("lopetetaan ohjelma")
print("goodbye!")
```

- Committoi muutokset

- Siirry branchiin **bugikorjaus**
- Muuta nyt tiedostoa (jälleen print-komentojen osalta) seuraavasti ja committaa

```py
# tehdään alussa importit

from logger import logger
from summa import summa
from erotus import erotus

logger("aloitetaan ohjelma")

x = int(input("luku 1: "))
y = int(input("luku 2: "))
print(f"Lukujen {x} ja {y} summa on {summa(x, y)}")  # muutos bugikorjaus-branchissa
print(f"Lukujen {x} ja {y} erotus on {erotus(x, y)}")  # muutos bugikorjaus-branchissa

logger("lopetetaan ohjelma")
print("goodbye!")
```

- Mergeä branchin **main** sisältö branchiin **bugikorjaus**
  - Nyt pitäisi aiheutua konflikti, komento aiheuttaa tulostuksen

```
Auto-merging index.py
CONFLICT (content): Merge conflict in index.py
Automatic merge failed; fix conflicts and then commit the result.
```

- Git ei siis osannut yhdistää tiedostoon tehtyjä muutoksia, koska ne kohdistuvat samoille riveille, seurauksena on konflikti.

- Ratkaise konflikti:
  - Editoi tiedoston **index.py** sisältö haluamaksesi
  - Ja toimi edellä mainitun artikkelien ohjeen mukaan, eli lisää konfliktoinut tiedosto staging-alueelle ja committoi

Jotkut editorit, esim [Visual Studio Code](https://code.visualstudio.com) sisältävät sisäänrakennetusti niin sanotun _merge toolin_, joka osaa jossain määrin helpottaa konfliktien ratkaisua:

![]({{ "/images/lh2-merge.png" | absolute_url }}){:height="350px" }

### 11. Git: branchit ja GitHub [versionhallinta]

**Tämä tehtävä tehdään palautusrepositorioon. Tekeminen on kuitenkin vapaaehtoista, eikä tehtävää arvostella**

**HUOM** tässä tehtävässä on tunnetusti välillä haastava seurata jokaista askelta siten, että päätyy aina samaan tilaan mitä tehtävä odottaa. Ei kannata stressata tästä liikaa. Pääasia tehtävässä on oppia miten branchit saadaan toimimaan lokaalisti ja GitHubissa siten, että _git push_ ja _git pull_ toimivat kaikille brancheille.

Aloita lukemalla ProGit-kirjasta luku [Remote Branches](http://git-scm.com/book/en/Git-Branching-Remote-Branches).

Lisätään seuraavaksi branch GitHubiin:

- Luo palautusrepositorion paikalliseen klooniin branch **haara1**, lisää sinne (hakemiston viikko2 juureen) tiedosto **haara1.txt** ja committaa
- Palaa haaraan **main**
- Luo toinen branch **haara2**, lisää sinnekin jokin (hakemiston viikko2 juureen) tiedosto **haara2.txt** ja committaa
- Pushaa uudet branchit GitHubiin
- Tarkastele GitHub-repositoriota selaimella, varmista että branchit syntyvät ja niillä on haluttu sisältö:

![]({{ "/images/lh2-branch1.png" | absolute_url }}){:height="350px" }

Kloonaa GitHub-repositoriosta koneellesi **toinen klooni**:

- Kuten huomaat, eivät branchit tule klooniin
- Tee paikalliseen klooniin branch joka "träkkää" GitHubissa olevan projektisi branchia **haara1** (ks. <http://git-scm.com/book/en/Git-Branching-Remote-Branches> kohta Tracking Branches)
- Lisää "träkkäävään" branchiin jokin tiedosto (hakemistoon viikko2), committaa ja pushaa branchi GitHubiin
- Tarkastele GitHub-repositoriota selaimella, varmista että branchi päivittyy

Mene GitHub-repositorion **alkuperäiseen** paikalliseen klooniin:

- Mene branchiin **haara1** ja pullaa muutokset GitHubiin vastaavasta branchista
  - huom: koska kyseessä ei ole "träkkäävä" branchi, joudut pullaamaan komennolla `git pull origin haara1`
    - Pelkän komennon `git pull` tuloste antaa ohjeen, miten saat halutessasi komennon toimimaan haaran sisältä ilman lisäparametreja
- Mene branchiin **haara2**, lisää sitten tiedosto, committaa ja pushaa branchi GitHubiin
  - Koska kyseessä ei ole "träkkäävä" branchi, ei komento `git push` riitä vaan joudut määrittelemään branchin jonne push kohdistuu eli antamaan komennon `git push origin haara2`
    - Pelkän komennon `git push` tuloste antaa ohjeen, miten saat halutessasi komennon toimimaan haaran sisältä ilman lisäparametreja


Mene jälleen **toiseen klooniin**:

- Suorita komento `git remote show origin`
- Komento kertoo "originissa" eli GitHubissa olevien branchien ja paikallisten branchien suhteen

```
* remote origin
  Fetch URL: git@github.com:mluukkai/ohtu-s22-palautukset.git
  Push  URL: git@github.com:mluukkai/ohtu-s22-palautukset.git
  HEAD branch: main
  Remote branches:
    haara1 tracked
    haara2 tracked
    main   tracked
  Local branches configured for 'git pull':
    haara1 merges with remote haara1
    main   merges with remote main
  Local refs configured for 'git push':
    haara1 pushes to haara1 (up to date)
    main   pushes to main   (up to date)
```

- Komennon tulosteesta selviää, että main ja haara1 ovat konfiguroitu toimimaan suoraan `git pull` ja `git push` -komennoilla
- Tee nyt kyseiseen klooniin (jossa siis olet nyt) myös branchia **haara2** träkkäävä branch
  - **Huom:** haaralle kannattaa myös tehdä heti `git pull`, sillä muuten lokaalin haaran tila on sama kuin sillä etärepositorion haaran tila sillä hetkellä kun kloonasit repositorion, eli vanhentunut koska haaraan on pushattu uutta sisältöä
- Suorita jälleen `git remote show origin`, mitä muutoksia huomaat?
- Tee branchiin **haara2** muutoksia ja pushaa ne GitHubiin
  - Koska kyseessä on träkkäävä branch, riittää git push
- Tarkastele GitHub-repositoriota selaimella, varmista että branchi päivittyy

Palaa vielä alkuperäiseen lokaaliin repositorioon:

- Suorita komento `git remote show origin`
- Tulostus kertoo, että lokaaleista haaroista ainoastaan _main_ on konfiguroitu komennon `git pull` osalta:

```
* remote origin
  Fetch URL: git@github.com:mluukkai/ohtu-s22-palautukset.git
  Push  URL: git@github.com:mluukkai/ohtu-s22-palautukset.git
  HEAD branch: main
  Remote branches:
    haara1 tracked
    haara2 tracked
    main tracked
  Local branch configured for 'git pull':
    main merges with remote main
  Local refs configured for 'git push':
    haara1 pushes to haara1 (up to date)
    haara2 pushes to haara2 (up to date)
    main pushes to main (up to date)
```

- Suorita `git pull` branchissä _haara2_
- Komennon tuloste antaa ohjeen, miten saat konfiguroitua `git pull` komennon toimimaan _haara2_:n sisällä ilman lisäparametreja
- Kun annat komennon, sen jälkeen haara träkkää GitHubissa olevaa haaraa ja komento `git pull` voidaan antaa ilman parametreja
  - Huom: komennossa oleva _&lt;branch&gt;_ tulee korvata branchin nimellä

Branchien kanssa työskentely voi aluksi tuntua sekavalta varsinkin jos GitHubissa on myös useita brancheja.

### Mihin brancheja käytetään?

Ohjelmistokehitystiimi voi soveltaa Gitin branchaystä hyvin monella eri tyylillä. Artikkeli
<https://www.atlassian.com/git/tutorials/comparing-workflows> esittelee tähän muutamia vaihtoehtoja. Eräs yleinen tapa branchien käyttöön ovat ns. _featurebranchit_:

> The core idea behind the Feature Branch Workflow is that all feature development should take place in a dedicated branch instead of the main branch. This encapsulation makes it easy for multiple developers to work on a particular feature without disturbing the main codebase. It also means the main branch will never contain broken code, which is a huge advantage for continuous integration environments.

Jos kiinnostaa, lue lisää yllä olevasta dokumentista.

### 12. Git: epäajantasaisen kloonin pushaaminen [versionhallinta]

Demonstroidaan vielä (viime viikon [tehtävässä 11](/tehtavat1#11-github-actions-osa-3) mainittu) usein esiintyvä tilanne, missä epäajantasaisen repositorion pushaaminen GitHubissa olevaan etärepositorioon epäonnistuu.

- Mene alkuperäisen repositorion paikallisen kloonin **main**-haaraan, tee jokin muutos, commitoi ja pushaa se GitHubiin
- Mene toisen kloonin **main**-haaraan ja tee sinne jokin muutos
- commitoi ja pushaa muutos GitHubiin
- Kaikki ei kuitenkaan mene hyvin, seurauksena on seuraavantyylinen virheilmoitus:

```
$ git push
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'git@github.com:mluukkai/ohtu-s22-palautukset.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Virheen syynä on se, että GitHubissa oleva **main**-haara oli edellä paikallisen repositorion **main**-haaraa.

Ongelma ratkeaa seuraavasti. Tee ensin komento `git pull`. Saat gitiltä pitkän valitusviestin:

```
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint:
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint:
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```

Käytännössä Git haluaa tietää minkälaisella strategialla paikallisen ja etärepositoriosi koodi tulisi yhdistää. Vaihtoehdoista kannattanee valita keskimäinen, eli anna komentorivillä komento

```
git config pull.rebase true 
```

Käytännössä valittu vaihtoehto tarkoittaa sitä, että Git suorittaa uudet lokaalit commitit etärepositoriossa olevien committien perään.

Voit nyt pullata koodin uudelleen komennolla `git pull`. Komento `git push` onnistuu nyt. Jatkossa vastaavista tilanteista selviää komennoilla `git pull` ja `git push`.

- Eli toimi näin ja varmista, että tekemäsi muutokset menevät GitHubiin


