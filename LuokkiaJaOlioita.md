# Luokkia ja olioita

## Luokka

Luokka ja sen metodit määritellään Rubylla seuraavasti:

```ruby
class Henkilo
  def tervehdi
    puts "Hello world"
  end

  def laske(x, y)
    puts "#{x} + #{y} on #{x+y}"
  end
end
```

Luokasta luodaan instansseja metodilla `new`, ja instanssien metodeja kutsutaan normaaliin tapaan pistenotaatiolla:

```ruby
h = Henkilo.new
h.tervehdi
h.laske(10, 5)
```

Muutetaan luokkaa lisäämällä sille konstruktori (eli metodi `initialize`), ja iän sekä nimen kertovat oliomuuttujat:

```ruby
class Henkilo
  def initialize(nimi)
    @ika = 0
    @nimi = nimi
  end

  def vanhene
    @ika += 1
  end

  def tervehdi
    puts "olen #{@nimi} ikäni on #{@ika}"
  end

end
```

Oliomuuttujat siis täytyy nimetä alkaen merkillä `@`. Kaikki muut muuttujat luokan sisällä tulkitaan metodien paikallisiksi muuttujiksi (`@@`:lla alkavat muuttujat ovat luokkamuuttujia).

> **Huom:** Railsissa `ActiveRecord`:in perivät, tietokantatauluja vastaavat _model_-luokat poikkeavat tästä, niissä tietokantataulun sarakkeita vastaavien oliomuuttujien nimissä ei ole merkkiä `@`. Teknisesti ottaen ne eivät olekaan oliomuuttujia.


Olioille voidaan määritellä merkkijonomuoto metodilla `to_s`:

```ruby
class Henkilo
  # ...

  def to_s
    "#{@nimi} ikä #{@ika} vuotta"
  end

end

h = Henkilo.new "Chang"
puts h
```

Huomaa, että Rubyssä konventiona on kirjoittaa metodikutsujen ja konstruktorien parametrit ilman sulkuja, erityisesti jos parametreja on vain yksi.

Luokkametodit määritellään seuraavasti:

```ruby
class Henkilo
  # ...

  def self.luokkametodi
    # ...
  end

end

# nyt voidaan kutsua

Henkilo.luokkametodi

# mutta seuraava ei toimi

h = Henkilo.new "Chang"
h.luokkametodi
```

Oliomuuttujien getterit ja setterit saadaan tarvittaessa generoitua automaattisesti:

``` ruby
class Henkilo
  attr_reader :nimi
  attr_accessor :ika, :osoite

  def initialize(nimi, osoite)
    @ika = 0
    @nimi = nimi
    @osoite = osoite
  end

  def vanhene
    @ika += 1
  end

  def to_s
    "#{@nimi} ikä #{@ika} vuotta, osoite #{@osoite}"
  end
end
```

`attr_accessor` määrittelee oliomuuttujille setterin ja getterin,
`attr_reader` pelkän getterin. Nyt oliomuuttujiin on mahdollista viitata pistenotaatiolla:

``` ruby
2.2.1 :020 > h = Henkilo.new "chang", "alppila"
 => #<Henkilo:0x007f8724189e28 @ika=0, @nimi="chang", @osoite="alppila">
2.2.1 :021 > h.nimi
 => "chang"
2.2.1 :022 > h.ika
 => 0
2.2.1 :023 > h.ika += 10
 => 10
2.2.1 :024 > puts h
chang ikä 10 vuotta, osoite alppila
 => nil
2.2.1 :025 > h.nimi = "arto"
NoMethodError: undefined method `nimi=' for #<Henkilo:0x007f8724189e28>
  from (irb):25
  from /Users/mluukkai/.rvm/rubies/ruby-2.2.1/bin/irb:11:in `<main>'
2.2.1 :026 >
```

Oliomuuttujaa `@ika` vastaava getteri on siis metodi `ika`, ja setteri metodi `ika=(param)`. Jos getterit ja setterit on määritelty, voidaan niitä käyttää myös olion sisällä:


``` ruby
class Henkilo
  # ...

  def vanhene
    ika = @ika + 1
  end

  def to_s
    "#{nimi} ikä #{ika} vuotta, osoite #{osoite}"
  end
end
```

Sijoitusmerkin oikealla puolella setteriä ei kuitenkaan voi käyttää, eli seuraava ei olisi toiminut:

``` ruby
class Henkilo
  # ...

  def vanhene
    ika = ika + 1
  end
end
```

Koodissa Ruby tulkitsisi että metodissa viitataan `ika`-nimiseen _paikalliseen_ muuttujaan. Jos getteriä haluttaisiin käyttää, onnistuisi se viittaamalla getteriin Rubyn `this`:in, eli olioon itseensä viittaavan `self`-viitteen avulla:

``` ruby
class Henkilo
  # ...

  def vanhene
    ika = self.ika + 1
  end
end
```

### Nimentä

Rubyssä konventiona on, että luokan nimet kirjoitetaan [CamelCasella](https://en.wikipedia.org/wiki/CamelCase). Metodit ja muuttujannimet taas [snake_casella](https://en.wikipedia.org/wiki/Snake_case).

Railsissa jokainen luokka on tapana määritellä omaan tiedostoonsa. Tiedostojen nimet kirjoitetaan snakecasella, eli luokka `Henkilo` määriteltäisiin tiedostossa _henkilo.rb_ ja esim. luokka `PersonController` tiedostossa *person_controller.rb*

## Vihjeitä tehtävien tekoon

Oletetaan että olemme määritelleet luokan `Henkilo` tiedostossa _henkilo.rb_

``` ruby
# tiedosto henkilo.rb
class Henkilo
  # ...
end
```

Jos haluamme käyttää luokkaa jostain muusta tiedostosta, on se _sisällytettävä_ toiseen tiedostoon komennolla `require`:

``` ruby
# tiedosto koodi.rb
require './henkilo.rb'

h = Henkilo.new 'chang', 'alppila'
```

Olioihin liittyvien tehtävien virheilmoituksista ei välttämättä selviä miten testattava olio on luotu ja mitä metodeja sille on kutsuttu ennen testattavaa asiaa. Kannattaakin tarkastaa asia testit sisältävästä tiedostosta *testi_spec.rb*

> ## [Tehtävä 13](https://github.com/HY-TKTL/ruby-tehtava13)
>
> Tee luokka `Pelaaja` (tiedostoon pelaaja.rb). Pelaajalle asetetaan konstruktorissa nimi ja pituus. Pelaajalla on myös maalimäärä. Pelaajalla on seuraavat metodit
> * maaleja, palauttaa maalien määrän
> * lisaa_maali, lisää yhden maalin
> * getteri ja setteri pituudelle
> * getteri nimelle
> * to_s, palauttaa merkkijonoesityksen, joka on muotoa
>
>   Arto (179 cm) maaleja 4

> ## [Tehtävä 14](https://github.com/HY-TKTL/ruby-tehtava14)
>
> Kopioi edellisessä tehtävässä tekemäsi tiedosto pelaaja.rb tämän tehtävän hakemistoon. Tee luokka `Joukkue` (tiedostoon joukkue.rb). Joukkue sisältää joukon pelaajia. Joukkueelle asetetaan nimi konstruktorissa. Joukkueella on seuraavat metodit:
> * lisaa_pelaaja(pelaaja) lisää joukkueeseen parametrina annetun pelaajan
> * maaleja_yhteensä, palauttaa joukkueen pelaajien yhteenlasketun maalimäärän
> * paras_maalintekija, palauttaa joukkueen eniten maaleja tehneen pelaajan

## Perintä

Perintä tapahtuu seuraavasti:

``` ruby
class Opiskelija < Henkilo
  def initialize(params)
    @nimi = params[:nimi]
    @opiskelijanumero  = params[:opnro]
    @ika = params[:ika] || 0
    @opintopisteet = params[:op] || 0
  end

  def opiskele
    @opintopisteet += 1
  end

  def to_s
    "#{nimi} (#{@opiskelijanumero}) #{@opintopisteet} op"
  end

end
```

Toisin kuin esim. Javassa, myös konstruktori perityy. Määrittelimme kuitenkin luokalle oman konstruktorin. Konstruktori saa vain yhden parametrin. Parametri on nyt `hash`, jonka avulla luotava olio konfiguroidaan:

``` ruby
2.2.1 :084 > c = Opiskelija.new nimi:"Chang", opnro:"12345"
2.2.1 :085 > c.to_s
 => "Chang (12345) 0 op"
2.2.1 :086 > a = Opiskelija.new nimi:"Arto", opnro:"01111", op:450, ika:31
2.2.1 :087 > a.to_s
 => "Arto (01111) 450 op"
```

Olion luonnissa käytetty syntaksi on mielenkiintoinen:

``` ruby
Opiskelija.new nimi:"Chang", opnro:"12345"
```

Normaalistihan hash määritellään aaltosulkeissa `{ nimi:"Chang", opnro:"12345" }`, metodikutsun viimeisenä parametrina oleva hash voidaan kuitenkin määritellä ilman aaltosulkeita ja näin Ruby-konvention mukaan lähes aina tehdäänkin.

Edellä olleesta esimerkistä huomasimme, että iän ja opintopisteiden määritteleminen olion luonnin yhteydessä oli vapaaehtoisa. Tämän takia konstruktorissa käytettiin operaattoria `||`:

``` ruby
  def initialize(params)
    # ...
    @ika = params[:ika] || 0
    @opintopisteet = params[:op] || 0
  end
```

Operaattori toimii siten, että lausekkeen `params[:op] || 0` arvona on sen vasemman puolen arvo jos se on määritelty (ja muuta kuin `false`). Jos vasen puoli ei ole määritelty, lausekkeen arvoksi tulee oikean puolen arvo eli 0. Operaattorin `||` käyttö vastaavissa tapauksissa on hyvin yleistä Rubyssä.

> ## [Tehtävä 15](https://github.com/HY-TKTL/ruby-tehtava15)
>
> Tehtävän projektissa on valmiina luokka `Piste`. Toteuta Pisteen perivät luokat `Varipiste` ja `Piste3d` (tiedostoihin varipiste.rb ja piste3d.rb). Hyödynnä luokissa mahdollisimman paljon yläluokasta perittävää koodia. Huomaa, että yliluokan metodia tai konstruktoria kutsutaan viitteen `super` avulla. Luokkien haluttu toiminnallisuus selviää testeistä.

## Moduulit

Perintä ei ole ainoa tapa liittää luokkaan muualla määriteltyä toiminnallisuutta. Jos sama toiminnallisuus pitää liittää useampaan, muuten toisistaan riippumattomiin luokkiin, kannattaa käyttää _moduuleja_.

Moduuli määritellään seuraavasti:

``` ruby
module Kasvattaja
  def lisaa_ikaan(vuotta)
    @ika += vuotta
  end
end
```

Näin määritelty moduuli on itsessään käyttökelvoton, mutta moduulin määrittelemä toiminnallisuus voidaan _sisällyttää_ luokkaan:

``` ruby
class Henkilo
  include Kasvattaja

  # ...
end
```

Sisällytetty metodi toimii täsmälleen kuten luokan omat oliometodit:

``` ruby
2.2.1 :119 > h = Henkilo.new "Arto", "Espoo"
 => #<Henkilo:0x007f93ba0934b8 @ika=0, @nimi="Arto", @osoite="Espoo">
2.2.1 :120 > h.lisaa_ikaan 20
 => 20
2.2.1 :121 > h.to_s
 => "Arto ikä 20 vuotta, osoite Espoo"
2.2.1 :122 > h.lisaa_ikaan 14
 => 34
2.2.1 :123 > h.to_s
 => "Arto ikä 34 vuotta, osoite Espoo"
2.2.1 :124 >
```

Moduulin määrittelemää toiminnallisuutta voidaan hyödyntää muissakin luokissa olettaen että, luokissa on määriteltynä oliometodi `@ika`:

``` ruby
class Elain
  include Kasvattaja

  # ...
end

class Auto
  include Kasvattaja

  # ...
end
```

> ## [Tehtävä 16](https://github.com/HY-TKTL/ruby-tehtava16)
>
> Tee moduuli `Siirrettava`, joka olettaa että luokassa, johon moduuli liitetään on, sen sijainnin kordinaatit kertovat oliomuuttujat `@x` ja `@y`. Moduuli määrittelee metodit:
> * siirraPisteeseen(x, y) joka muuttaa sijainin parametreina olevaan pisteeseen
> * siirraSuuntaan(dx, dy) joka muuttaa sijainnin siten, että se lisää parametrina olevat arvot vanhaan sijaintiin

## Moduuli nimiavaruutena

Moduuleja voidaan käyttää myös muodostamaan Javan pakkausrakennetta vastaavia _nimiavaruuksia_.

``` ruby
module Yliopisto
  class Henkilo
    # ...
  end

  class Laitos
    # ...
  end

  class Kurssi
    # ...
  end
end
```

Moduulin sisällä määriteltyihin luokkiin viitataan seuraavasti:

``` ruby
tktl = Yliopisto::Laitos.new nimi:"TKTL"
```

Saman moduulin sisällä olevassa koodissa luokkien nimiin voi viitata myös suoraan:

``` ruby
module Yliopisto
  class Laitos
    def perusta_kurssi(nimi)
      kurssit[nimi] = Kurssi.new nimi
      # ...
  end

  class Kurssi
    # ...
  end
end
```

## hieman haastavampia tehtäviä

Seuraavassa vielä muutama hieman haastavampi, omatoimista tiedonhakua edellyttävä tehtävä.

> ## [Tehtävä 17](https://github.com/HY-TKTL/ruby-tehtava17)
>
> Tee moduuli `Debugattava` joka määrittelee metodin `tila`. Metodi tulostaa olion jokaisen oliomuuttujan nimen ja arvon.
>
> Esim. jos liität moduulin luokkaan `Henkilo`, ja kutsut metodia  henkilöoliolle seuraavassa tilanteessa
>
> ``` ruby
> class Henkilo
>   include Debugattava
>    # ...
>
> end
>
> h = Henkilo.new 'Chang', 'Alppila'
> h.tila
> ```
> tulostuu
> ```
> nimi Chang
> osoite Alppila
> ika 0
> ```
>
> Rubyn luokka [Object](http://ruby-doc.org/core-2.3.0/Object.html), jonka kaikki luokat perivät tarjoavat sopivat metodit, joiden avulla pääset oliomuuttujiin ja niiden arvoihin käsiksi. Ks. [http://ruby-doc.org/core-2.3.0/Object.html](http://ruby-doc.org/core-2.3.0/Object.html)

> ## [Tehtävä 18](https://github.com/HY-TKTL/ruby-tehtava18)
>
> Tee luokka `Lukija`, joka saa konstruktorin parametrina tiedoston nimen. Tiedosto sisältää tuntikirjauksia, ja on muotoa:
>
>```
>2.9.15 7h
>3.9.15 3h
>4.9.15 8h
>5.9.15 4h
>6.9.15 5h
>7.9.15 4h
>8.9.15 1h
>11.9.15 2h
>```
>
> eli tiedoston yksittäinen rivi sisältää päivämäärän ja tuntikirjauksen. Luokalla on metodi `tilasto`, joka palauttaa syötteen perusteella muodostettavan hashin, jolla on seuraavat avaimet ja arvot
* :yhteensa, tuntikirjausten summa
* :keskiarvo, keskimääräinen tuntikirjaus
* :alku, tuntikirjausten ensimmäinen päivä
* :loppu, tuntikirjausten viimeinen päivä
>
> Huomaa, että kirjaukset eivät ole tiedostossa välttämättä järjestyksessä

## Koodilohko

Olemme jo moneen kertaan käyttäneet _koodilohkoja_, esim. metodin `each` yhteydessä

```ruby
t = [1, 2, 3, 4]
t.each do |alkio|
  puts alkio
end
```

Esimerkissä metodin each _parametrina_ on lohko, joka suoritetaan jokaiselle taulukon alkiolle.

Itseasiassa Rubyssä _jokaiselle_ metodille voi antaa parametriksi lohkon. Jos olisimme määritelleet

```ruby
def metodi
  puts "hello world"
end
```

voitaisiin sitä kutsua seuraavasti

```ruby
metodi do
  puts "olen lohko"
end
```

tai käyttämättä vaihtoehtoista syntaksia lohkon määrittelylle


```ruby
metodi {
  puts "olen lohko"
}
end
```

Lohko ei nyt kuitenkaan vaikuta mitenkään metodin suoritukseen.
Metodissa voidaan _suorittaa_ parametrina oleva lohko komennolla `yield`. Jos määritellään

```ruby
def metodi2
  puts "metodin omaa koodia"
  yield
  puts "lisää metodin koodia"
end
```

ja kutsutaan
```ruby
metodi2 do
  puts "olen lohko"
end
```

tulostuu

```
metodin omaa koodia
olen lohko
lisää metodin koodia
```

eli komennon `yield` kutsu suorittaa lohkon koodin.

Lohkolle voidaan myös välittää kutsussa yksi tai useampia parametreja:

```ruby
def metodi3
  puts "metodin omaa koodia"
  yield 10
  yield 3
  yield 7
  puts "lisää metodin koodia"
end
```

Kun kutsutaan
```ruby
metodi3 do |x|
  puts "olen lohko, parametri #{x}"
end
```

tulostuu

```
metodin omaa koodia
olen lohko, parametri 10
olen lohko, parametri 3
olen lohko, parametri 7
lisää metodin koodia
```

> ## [Tehtävä 19](https://github.com/HY-TKTL/ruby-tehtava19)
>
> Tee (tiedostoon koodi.rb) metodi `tulosta(x)`, joka tulostaa parametrinsa `x` siten, että parametrille on suoritettu ensin metodin parametrina oleva _koodilohko_. Jos metodille ei ole annettu koodilohkoa parametriksi, se tulostaa ainoastaan parametrin `x`. Vihje: metodi voi tarkistaa onko sen parametrina koodilohko kutsumalla `block_given?`
>
> Esim. jos metodia kutsutaan seuraavasti:
> ```ruby
> tulosta 5 do |luku|
>   "x"*luku
> end
> ```
>
> Tulostuu
> ```
> xxxxx
> ```

> ## [Tehtävä 20](https://github.com/HY-TKTL/ruby-tehtava20)
>
> Tee luokka `Pino`, jolla on seuraavat metodit
> * `push(x)` laittaa x:n pinoon
> * `pop` ottaa palauttaa pinon päälimmäisen alkion, jos pino on tyhjä heitetään `RuntimeError` poikkeus
> Tämän lisäksi pinon tulee sisällyttää moduuli `Enumerable`
> Sisälläytyksen ansiosta pino siis saa kaiken [toiminnallisuuden](http://ruby-doc.org/core-2.3.0/Enumerable.html) mitä Enumerable-moduuli määrittelee
