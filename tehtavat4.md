---
layout: page
title: Viikko 4
inheader: no
permalink: /tehtavat4/
---


Tehtävissä tutustutaan riippuvuuksien "mockaamiseen" yksikkötesteissä. 


### Typoja tai epäselvyyksiä tehtävissä?

{% include typo_instructions.md %}

{% include norppa.md %}

{% include poetry_ongelma.md %}


**Kaikki tämän viikon tehtävät palautetaan** jo edellisillä viikoilla käyttämääsi **palautusrepositorioon**, sinne tehtävän hakemiston _osa4_ sisälle. 

### VS Coden konfigurointi

Osaatko konfiguroida VS Coden oikein? Jos ei, lue [tämä](/tehtavat2/#bonus-vs-coden-konfigurointi)!

### 1. Yksikkötestaus ja riippuvuudet: mock-kirjasto, osa 1

Useimmilla luokilla on riippuvuuksia toisiin luokkiin. Esim. [viikon 2](/tehtavat2#5-riippuvuuksien-injektointi-osa-2-nhl-tilastot) laskarien NHL-tilastot-tehtävässä luokka `StatisticsService` riippuu luokasta `PlayerReader`. Riippuvuuksien injektion avulla saimme mukavasti purettua riippuvuudet luokkien väliltä.

Vaikka luokilla ei olisikaan riippuvuuksia toisiin luokkiin, on tilanne edelleen se, että luokan oliot käyttävät joidenkin toisten luokkien olioiden palveluita. Tämä tekee yksikkötestauksesta välillä hankalaa. Miten esim. luokkaa `StatisticsService` tulisi testata? Tuleeko testeissä olla mukana toimivat versiot kaikista sen riippuvuuksista?

Eräs ratkaisu on ohjelmoida riippuvuuden korvaava "tynkäkomponentti" `PlayerReaderStub` ja injektoida tämä tuotannossa käytettävän PlayerReader luokan paikalle:

```python
import unittest
from statistics_service import StatisticsService
from player import Player

class PlayerReaderStub:
    def get_players(self):
        return [
            Player("Semenko", "EDM", 4, 12),
            Player("Lemieux", "PIT", 45, 54),
            Player("Kurri",   "EDM", 37, 53),
            Player("Yzerman", "DET", 42, 56),
            Player("Gretzky", "EDM", 35, 89)
        ]

class TestStatisticsService(unittest.TestCase):
    def setUp(self):
        # annetaan StatisticsService-luokan oliolle "stub"-luokan olio
        self.stats = StatisticsService(
            PlayerReaderStub()
        )

    # ...
```

Pythonille kuten kaikille muillekin kielille on tarjolla myös valmiita kirjastoja tynkäkomponenttien, toiselta nimeltään _mock-olioiden_ luomiseen.

Kuten pian huomaamme, mock-oliot eivät ole pelkkiä "tynkäolioita", mockien avulla voi myös varmistaa, että testattava metodi tai funktio kutsuu olioiden metodeja asiaankuuluvalla tavalla.

Tutustumme nyt unittest-moduulin [mock](https://docs.python.org/3/library/unittest.mock.html)-kirjastoon. Kirjastosta voidaan tuoda luokka [Mock](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock). Katsotaan mitä luokalla voi tehdä käynnistämällä interaktiivinen Python-terminaali komennolla `python3` (virtuaaliympäristölle ei ole tarvetta, koska emme käytä ulkoisia riippuvuuksia):

```python
>>> from unittest.mock import Mock
>>> mock = Mock()
>>> mock
<Mock id='4568521696'>
```

Anna syötteet terminaaliin yksi kerrallaan. Enter-painikkeen painallus suorittaa annetun syötteen. Muuttuja `mock` sisältää siis `Mock`-luokan olion. `Mock`-luokan olioilla on se mielenkiintoinen piirre, että niiden kaikki mahdolliset attribuutit ja metodit on toteutettu. Mitä tällä tarkoitetaan? Kokeillaan:

```python
>>> mock.foo
<Mock name='mock.foo' id='4568521648'>
>>> mock.foo.bar()
<Mock name='mock.foo.bar()' id='4570560112'>
```

Kaikki annetut operaatiot palauttavat siis uuden `Mock`-olion. Voimme antaa olion metodeille haluttuja paluuarvoja [return_value](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.return_value)-attribuutin avulla:

```python
>>> mock.foo.bar.return_value = "Foobar"
>>> mock.foo.bar()
'Foobar'
```

Voimme myös antaa metodeille haluttuja toteutuksia [side_effect](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.side_effect)-attribuutin avulla:

```python
>>> mock.foo.bar.side_effect = lambda name: f"{name}: Foobar"
>>> mock.foo.bar("Kalle")
'Kalle: Foobar'
```

Attribuutin `side_effect` arvo pitää olla kutsuttavissa, kuten funktio, metodi, tai lambda. Huomaa, että `Mock`-oliota voi käyttää myös funktion kaltaisesti:

```python
>>> get_name_mock = Mock(return_value = "Matti")
>>> get_name_mock()
'Matti'
```

Mockeille voidaan määritellä toteutuksien lisäksi oletuksia. Voimme esimerkiksi olettaa, että `Mock`-oliota on kutsuttu:

```python
>>> mock.foo.bar.assert_called()
>>> mock.foo.doo.assert_called()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Users/kalleilv/.pyenv/versions/3.9.0/lib/python3.9/unittest/mock.py", line 876, in assert_called
    raise AssertionError(msg)
AssertionError: Expected 'doo' to have been called.
```

Voimme siis kutsua tarkasteltavalle metodille [assert_called](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.assert_called)-metodia. Huomaa, että `mock.foo.bar`-metodia on kutsuttu, mutta `mock.foo.doo`-metodia sen sijaan ei ole. Voimme myös tarkistaa, että metodia on kutsuttu oikeilla argumenteilla käyttämällä [assert_called_with](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.assert_called_with)-metodia.

Kun `Mock`-oliot ovat tulleet tutuksi, voit sulkea terminaalin komennolla `exit()`.

**Hae seuraavaksi [kurssirepositorion]({{site.python_exercise_repo_url}}) hakemistossa _osa4/mock-demo_ oleva projekti.**
- Tässä tehtävässä ei tehdä mitään koodia, joten projektia ei ole tarvetta välttämättä palauttaa 
- Voit halutessasi kopioida projetin palatusrepositorioosi, hakemiston osa4 sisälle.

Projekti on yksinkertainen verkkokauppa, jonka sovelluslogiikan totutettaa luokka `Kauppa`. Luokalla on riippuvuus `Pankki`- ja `Viitegeneraattori`-olioihin.

Kaupan toimintaperiaate on yksinkertainen:

```python
my_net_bank = Pankki()
viitteet = Viitegeneraattori()
kauppa = Kauppa(my_net_bank, viitteet)

kauppa.aloita_ostokset()
kauppa.lisaa_ostos(5)
kauppa.lisaa_ostos(7)
kauppa.maksa("1111")
```

Ostokset aloitetaan tekemällä metodikutsu `aloita_ostokset`. Tämän jälkeen "ostoskoriin" lisätään tuotteita, joiden hinta kerrotaan metodin `lisaa_ostos` parametrina. Ostokset lopetetaan kutsumalla metodia `maksa` joka saa parametriksi tilinumeron jolta summa veloitetaan.

Kauppa tekee veloituksen käyttäen tuntemaansa luokan `Pankki` olioa. Viitenumerona käytetään luokan `Viitegeneraattori` generoimaa numeroa.

Projektiin on kirjoitettu kuusi `Mock`-luokkaa hyödyntävää testiä. Testit testaavat, että kauppa tekee ostoksiin liittyvän veloituksen oikein, eli että se kutsuu `Pankki`-luokan metodia `maksa` oikeilla parametreilla, ja että jokaiselle laskutukselle on kysytty viitenumero `Viitegeneraattori`-luokan metodilta `uusi`. Testit siis eivät kohdistu kauppa-olion tilaan vaan sen muiden olioiden kanssa käymän interaktion oikeellisuuteen. Testeissä kaupan riippuvuudet (`Pankki` ja `Viitegeneraattori`) on määritelty `Mock`-olioina.

Seuraavassa testi, joka testaa, että kauppa kutsuu pankin metodia oikealla tilinumerolla ja summalla:

```python
def test_kutsutaan_pankkia_oikealla_tilinumerolla_ja_summalla(self):
    pankki_mock = Mock()
    viitegeneraattori_mock = Mock(wraps=Viitegeneraattori())

    kauppa = Kauppa(pankki_mock, viitegeneraattori_mock)

    kauppa.aloita_ostokset()
    kauppa.lisaa_ostos(5)
    kauppa.lisaa_ostos(5)
    kauppa.maksa("1111")

    # katsotaan, että ensimmäisen ja toisen parametrin arvo on oikea
    pankki_mock.maksa.assert_called_with("1111", 10, ANY)
```

Testi siis aloittaa luomalla kaupan riippuvuuksista mock-oliot:

```python
pankki_mock = Mock()
viitegeneraattori_mock = Mock(wraps=Viitegeneraattori())

kauppa = Kauppa(pankki_mock, viitegeneraattori_mock)
```

`Mock`-luokan [konstruktorin](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock) `wraps`-parametrin avulla voimme määritellä, minkä olion `Mock`-olio toteuttaa. Tämä mahdollistaa sen, ettei esimerkiksi `uusi`-metodille tarvitse määritellä toteutusta, vaan voimme käyttää sen oikeaa toteutusta.

Eli nyt viitegeneraattori on olio, jonka metodi `uusi` palauttaa arvot 1, 2, 3...

Testi tarkastaa, että kaupalle tehdyt metodikutsut aiheuttavat sen, että pankin `Mock`-olion metodia `maksa` on kutsuttu oikeilla parametreilla. Kolmanteen parametriin, eli viitenumeroon ei kiinnitetä huomiota:

```python
pankki_mock.maksa.assert_called_with("1111", 10, ANY)
```

Kuten edellisissä esimerkeissä tuli ilmi, `Mock`-olioille tehtyjen metodikutsujen paluuarvot on mahdollista määrittää. Seuraavassa määritellään, että viitegeneraattori palauttaa arvon `55` kun sen metodia `uusi` kutsutaan:

```python
def test_kaytetaan_maksussa_palautettua_viitetta(self):
    pankki_mock = Mock()
    viitegeneraattori_mock = Mock()

    # palautetaan aina arvo 55
    viitegeneraattori_mock.uusi.return_value = 55

    kauppa = Kauppa(pankki_mock, viitegeneraattori_mock)

    kauppa.aloita_ostokset()
    kauppa.lisaa_ostos(5)
    kauppa.lisaa_ostos(5)
    kauppa.maksa("1111")

    # katsotaan, että kolmannen parametrin arvo on oikea
    pankki_mock.maksa.assert_called_with(ANY, ANY, 55)
```

Testin lopussa varmistetaan, että pankin `Mock`-oliota on kutsuttu oikeilla parametrinarvoilla, eli kolmantena parametrina tulee olla viitegeneraattorin palauttama arvo.

Tutustu projektiin ja sen kaikkiin testeihin. Asenna projektin riippuvuudet komennolla `poetry install` ja suorita sen jälkeen testit virtuaaliympäristössä komennolla `pytest`. Riko jokin testi, esimerkiksi jokin edellä mainituista, muuttamalla sen ekspektaatiota esim. seuraavasti:

```python
pankki_mock.maksa.assert_called_with(ANY, ANY, 1000)
```

Ja varmista, että testit eivät mene läpi. Katso miltä virheilmoitus näyttää.

Voit tutustua aiheeseen tarkemmin lukemalla mock-kirjaston [dokumentaatiota](https://docs.python.org/3/library/unittest.mock.html).

### 2. Yksikkötestaus ja riippuvuudet: mock-kirjasto, osa 2

Hae [kurssirepositorion]({{site.python_exercise_repo_url}}) hakemistossa _osa4/maksukortti-mock_ oleva projekti.
- Kopioi projekti palatusrepositorioosi, hakemiston osa4 sisälle.

Tässä tehtävässä on tarkoitus testata ja täydentää luokkaa `Kassapaate`.

**Maksukortin koodiin ei tehtävässä saa koskea ollenkaan! Testeissä ei myöskään ole tarkoitus luoda konkreettisia instansseja maksukortista, testien tarvitsemat kortit tulee luoda mock-kirjaston avulla.**

Projektissa on valmiina kaksi testiä:

```python
import unittest
from unittest.mock import Mock, ANY
from kassapaate import Kassapaate, HINTA
from maksukortti import Maksukortti

class TestKassapaate(unittest.TestCase):
    def setUp(self):
        self.kassa = Kassapaate()

    def test_kortilta_velotetaan_hinta_jos_rahaa_on(self):
        maksukortti_mock = Mock()
        maksukortti_mock.saldo = 10

        self.kassa.osta_lounas(maksukortti_mock)

        maksukortti_mock.osta.assert_called_with(HINTA)

    def test_kortilta_ei_veloteta_jos_raha_ei_riita(self):
        maksukortti_mock = Mock()
        maksukortti_mock.saldo = 4

        self.kassa.osta_lounas(maksukortti_mock)

        maksukortti_mock.osta.assert_not_called()
```

Ensimmäisessä testissä varmistetaan, että jos kortilla on riittävästi rahaa, kassapäätteen metodin `osta_lounas` kutsuminen veloittaa summan kortilta.

Testi ottaa siis kantaa ainoastaan siihen miten kassapääte kutsuu maksukortin metodeja. **Maksukortin saldoa ei erikseen tarkasteta**, sillä oletuksena on, että maksukortin omat testit varmistavat kortin toiminnan.

Toinen testi varmistaa, että jos kortilla ei ole riittävästi rahaa, kassapäätteen metodin `osta_lounas` kutsuminen _ei_ veloita kortilta rahaa.

**Testit eivät mene läpi. Korjaa kassapäätteen metodi `osta_lounas`.**

**Muistutus** Maksukortin koodiin ei tehtävässä saa koskea ollenkaan! Maksukortin tilaa ei myöskään ole tarkoitus tutkia suoraan, koska Maksukortti on mock ei attribuuttien arvojen katsominen edes ole mahdollista/mielekästä.

**Tee tämän jälkeen samaa periaatetta noudattaen seuraavat testit:**

- Kassapäätteen metodin `lataa` kutsu lisää maksukortille ladattavan rahamäärän käyttäen kortin metodia `lataa` jos ladattava summa on positiivinen
- Kassapäätteen metodin `lataa` kutsu ei tee maksukortille mitään jos ladattava summa on negatiivinen

**Huomio:** 
- Testeissä ei ole tarkoitus luoda konkreettisia instansseja maksukortista, testien tarvitsemat kortit tulee luoda mock-kirjaston avulla. 
- Testit eivät myöskään testaa suoraan maksukortin tilaa, ainoastaan sitä onko maksukortin metodeja kutsuttu oikein.

Korjaa kassapäätettä siten, että testit menevät läpi.


### Mock-olioiden käytöstä

Mock-oliot saattoivat tuntua hieman monimutkaisilta edellisissä tehtävissä. Mockeilla on kuitenkin paikkansa. Jos testattavana olevan olion riippuvuutena oleva olio on monimutkainen, kuten esimerkiksi verkkokauppaesimerkissä luokka `Pankki`, kannattaa testattavana oleva olio testata ehdottomasti ilman todellisen riippuvuuden käyttöä testissä. Valeolion voi toki tehdä myös "käsin", mutta tietyissä tilanteissa mock-kirjastoilla tehdyt mockit ovat käsin tehtyjä valeolioita kätevämpiä, erityisesti jos on syytä tarkastella testattavan olion riippuvuuksille tekemiä metodikutsuja.

### 3. Retrospektiivitekniikat

Wikipedian mukaan retrospektiivi on _"a meeting held by a project team at the end of a project or process (often after an iteration) to discuss what was successful about the project or time period covered by that retrospective, what could be improved, and how to incorporate the successes and improvements in future iterations or projects."_

Tutustu [täällä](http://retrospectivewiki.org/index.php?title=Retrospective_Plans) esiteltyihin retrospektiivitekniikoihin [Start, Stop, Continue, More of, Less of Wheel](http://retrospectivewiki.org/index.php?title=Start,_Stop,_Continue,_More_of,_Less_of_Wheel) ja [Glad, Sad, Mad](http://retrospectivewiki.org/index.php?title=Glad,_Sad,_Mad).

Tee aiheesta noin 0.25 sivun (eli noin 125 sanaa) tiivistelmä palautusreporitorion hakemistoon _viikko4_ sijoitettavaan tiedostoon _retro.md_.

Pidä huoli siitä, että miniprojektitiimisi pitää toisen sprintin lopussa jompaa kumpaa tekniikkaa noudattavan retrospektiivin!

### Tehtävien palauttaminen

Pushaa mock-tehvät GitHubiin palautusrepositorioosi ja merkkaa tekemäsi tehtävät [Timiin](https://tim.jyu.fi/view/kurssit/tie/tjta330/ohjelmistotuotanto-k2024/tehtavat/konfigurointitehtavat-osa-4). Retrospektiivitehtävä palautetaan suoraan Timissä olevalle lomakkeelle.
