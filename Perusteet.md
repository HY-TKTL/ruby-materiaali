# Olio-ohjelmointia Rubyllä

Nopea johdatus Rubyyn jo ohjelmointia osaaville. Kohdeyleisönä lähinnä kurssin Web-palvelinohjelmointi Ruby on Rails -osallistujat.

Huomaa, että kurssi on vielä beta-testausvaiheessa. Jos huomaat materiaalissa typoja tai muita ongelmia, tee _pull request_, eli mene osoitteeseen https://github.com/HY-TKTL/ruby-materiaali/blob/master/Perusteet.md paina vasemmalta kynä-symboolia ja tee muutokset.

Aloita asentamalla koneellesi Ruby esim. [tämän](https://github.com/mluukkai/WebPalvelinohjelmointi2016/wiki/railsin-asennus) ohjeen mukaan.

Linkit
* [Ohjeita tehtävien tekemiseen](https://github.com/HY-TKTL/ruby-materiaali/blob/master/tehtavien_tekeminen.md)
* [Pistelista](https://ruby-scoresheet.herokuapp.com/)

## Perusteet

### Komentotulkki

Tähän mennessä olemme asentaneet Rubyn, ja olemme valmiita kokeilemaan kielen ominaisuuksia. Tärkeä työkalu Rubyllä ohjelmoidessa on interaktiivinen tulkki _irb_.

Avataan irb komennolla `irb`. Ruudulla lukee nyt:

`irb(main):001:0>`

Nuolen jälkeen kirjoitetut käskyt suoritetaan, ja koodin tulos tulostuu ruudulle. Koitetaan! Kirjoita irbiin seuraava koodinpätkä:

```ruby
irb(main):001:0> puts "Hei Maailma!"
```

Huom, komentotulkkiin kirjoitetaan vain nuolen `>` jälkeinen osa. Painamalla `enter` Ruby suorittaa koodin, ja tulostaa tuloksen:

```ruby
irb(main):001:0> puts "Hei Maailma!"
Hei Maailma!
=> nil
```

Komento tulostaa ruudulle oletuksen mukaisesti merkkijonon, mutta sen lisäksi tulostuu `=> nil`. Kyseessä on komennon puts paluuarvo `nil`, joka vastaa Javan null:ia. Toisin kuin esim. Javassa, Rubyssä jokaisella komennolla on paluuarvo, ja koska komennon `puts`ei tarvitse palauttaa mitään erityistä, se palauttaa nil:in eli "ei mitään".

Voimme myös kirjoittaa artimetiikkaa irbiin. Seuraava havainnollistaa tätä:

```ruby
irb(main):001:0> 10+2
=> 12
```

```ruby
irb(main):001:0> 10*2
=> 20
```

```ruby
irb(main):001:0> 30/3
=> 10
```

Muuttujan _määrittelyksi_ riittää sen nimen kertominen, eli esim. jos sijoitetaan muuttujaan jokin arvo  `a = 100`, tulee muuttuja määritellyksi.  Ruby on dynaamisesti tyypitetty kieli, eli muuttujille ei määritellä tyyppiä.  Samalla tavalla toimisi esim `b = "Moi"`.

```ruby
irb(main):001:0> a = 100
=> 100
irb(main):002:0> b = "Moi"
=> "Moi"
```

Toisin kuin Java, Ruby ei osaa yhdistää merkkijonoon kokonaislukua:

```ruby
irb(main):003:0> a + b
TypeError: String can't be coerced into Fixnum
        from (irb):3:in `+'
        from (irb):3
        from E:/ruby-1.9.3/bin/irb:12:in `<main>'
irb(main):004:0>
```

Voimme kuitenkin kiertää ongelman muuttamalla kokonaisluvun merkkijonoksi kutsumalla sen metodia `to_s`:

```ruby
irb(main):003:0> a.to_s + b
=> "100Moi"
```

Useimmiten Rubyssä ei muodosteta merkkijonoja metodilla + vaan käyttämällä #{}-operaattoria, jonka avulla merkkijonon sisälle voidaan sijoittaa mielivaltaisen lausekkeen arvo:

```ruby
irb(main):003:0> "Moi #{a}"
=> "Moi 100"
irb(main):004:0> "Hello #{1+2+3+4}"
=> "Hello 10"
```

### metodit

Käytimme aiemmin komentoa `puts` tulostamaan tekstiä. Puts on itseasiassa metodi. Voimme luonnollisesti määritellä myös omia metodeja. Metodi määritellään seuraavasti:

```ruby
irb(main):001:0> def tervehdi
irb(main):002:1>  puts "Moi!"
irb(main):003:2> end
=> :tervehdi
```
Ylläolevassa koodissa määrittelemme metodin `tervehdi`, joka tulostaa "Moi!" kun sitä kutsutaan. Metodin kutsuminen on helppoa:

```ruby
irb(main):001:0>tervehdi
Moi!
=> nil
```

Määritellään uusi metodi joka tuplaa annetun luvun.

```ruby
irb(main):012:0> def tuplaa(luku)
irb(main):013:1>  luku*2
irb(main):014:1> end
=> :tuplaa
irb(main):015:0> tuplaa 2
=> 4
irb(main):016:0> tuplaa 5
=> 10
irb(main):017:0>
```
Ensimmäinen rivi määrittelee metodin tuplaa, joka ottaaa *parametrin* `luku`. Rubyssä palautetaan automaattiseti metodin viimeisen operaation tulos, eli metodimme palauttaa lausekkeen `luku*2` arvon.
Olisimme voineet käyttää myös komentoa `return` mutta se on tässä tapauksessa turha:

```ruby
irb(main):012:0> def tuplaa(luku)
irb(main):013:1>  return luku*2
irb(main):014:1> end
```

Metodin kutsumiseen on kaksi tapaa, parametri voidaan lisätä Java-tyyliin sulkuihin `tuplaa(2)`, mutta kutsu onnistuu myös ilman sulkuja `tuplaa 2`.

Jos metodista on tarve poistua ennen viimeistä riviä, tarvitaan returnia. Seuraavassa parametrinsa itseisarvon palauttava metodi:

```ruby
irb(main):015:0> def itseisarvo(luku)
irb(main):016:1>   if luku<0
irb(main):017:1>     return -luku
irb(main):018:1>   end
irb(main):019:1>   luku
irb(main):020:1> end
```

Huomaa, että vain yhden rivin sisältävä `if` kirjoitetaan Rubyssä yleensä seuraavasti:

```ruby
irb(main):021:0> def itseisarvo(luku)
irb(main):022:1>   return -luku if luku<0
irb(main):023:1>   luku
irb(main):024:1> end
```

eli palauta `-luku` jos luku on negatiivinen.

Miten palauttaminen eroaa tulostamisesta? Metodikutsumme ei tulostanut yhtään mitään, mutta irb kätevästi kertoi meille metodin paluuarvon notaatiolla `=>`.

```ruby
irb(main):017:0> tuplaa 2
=> 4
```

Metodimme paluuarvo katoaa, ellemme tee sille jotain. Voimme kuitenkin yhdistää oppimiamme taitoja ja tulostaa metodin paluuarvon:

```ruby
irb(main):017:0> puts tuplaa 2
4
=> nil
```

Ylläoleva koodi *palauttaa* `nil`:in, mutta *tulostaa* luvun 4. Paljon käyttämämme metodi `puts` siis itseasiassa ottaa parametrina merkkijonon, jonka se tulostaa ruudulle.

Irb on todella tärkeä työkalu Ruby-ohjelmoijalle. Aina kun on tarvetta kokeilla jonkin koodinpätkän tai komennon toimintaa, on kätevää turvautua komentotulkkiin.

### Ruby-tiedosto

Usein haluamme kirjoittaa laajempia kokonaisuuksia, ja tähän komentotulkkimme on käytettävyydeltään liian huono. Voimmekin kirjoittaa koodimme `.rb`-päätteellä varustettuun teksitiedostoon ja suorittaa sen komennolla `ruby`.

Oletetaan että edellä määritelty metodi ja sitä kutsuva koodi olisi kirjoitettu tiedostoon `test.rb`:

```ruby
def tuplaa(luku)
  luku*2
end

# pääohjelma
luku = tuplaa(4)
puts luku
```

Voimme nyt *suorittaa* tiedoston komennolla `ruby test.rb`, joka tulostaa:

```ruby
user@cpu:~$ ruby test.rb
8
```

Huomataan, että paluuarvoa `=> nil` ei enää tulosteta. Metodin paluuarvon automaattinen tulostus onkin irb:in ominaisuus.

Emme tarvitse nyt mitään erillisä pääohjelmaa (kuten Javan main), Ruby-tulkki alkaa suorittaa komentoja siinä järjestyksessä kuin niitä tiedostossa on.

Kannattaa huomata että toisin kuin monissa kielissä (esim. Javassa) Rubyssä koodia ei _käännetä_ vaan se _tulkitaan_ rivi riviltä. Tämän takia koodiin tehdyt kirjoitusvirheetkin paljastuvat vasta koodia suoritettaessa.

Jos olisimme kirjottaneet tiedoston sisällön seuraavasti (eli metodin kutsu ennen mentodin määrittelyä), koodin suoritus olisi aiheuttanut virheen:

```ruby
luku = tuplaa(4)
puts luku

def tuplaa(luku)
  luku*2
end
```

Virhe johtuu nyt siitä, että Ruby-tulkki ei ole vielä ehtinyt metodimäärittelyyn asti siinä vaiheessa kun metodia kutsutaan. Eli toisin kuin Javassa, metodit (ja luokat) on määriteltävä koodissa ennen kuin niitä pystytään käyttämään.

### Kurssin tehtävät

Ohjeita tehtävien tekemiseen [täällä](https://github.com/HY-TKTL/ruby-materiaali/blob/master/tehtavien_tekeminen.md)

> #### [Tehtävä 1](https://github.com/HY-TKTL/ruby-tehtava1)
> Tee metodi `summa`, joka _palauttaa_ parametreinaan saamansa kahden luvun summan.

> #### [Tehtävä 2](https://github.com/HY-TKTL/ruby-tehtava2)
> Tee metodi `erotus`, joka _tulostaa_ parametreinaan saamansa kahden luvun erotuksen.


### Toistolauseet

Rubyssä on useita hieman erilaisiin tarkoituksiin soveltuvia tapoja toteuttaa toistorakenne.

Rubymäinen tapa toistaa jokin asia 5 kertaa olisi seuraava:

```ruby
5.times do
  puts "Moi"
end
```

Koodi siis toistaa `do`:n ja `end`:in välissä olevan koodin tai täsmällisemmin ilmaistuna _koodilohkon_ 5 kertaa ja lopettaa.

Jos on tarvetta tietää, monesko toiston kerroista on menossa, toimitaan seuraavasti:

```ruby
5.times do |i|
  puts "Kierros #{i}"
end
```

Koodilohkolle on nyt määritellään muuttuja `i` joka saa kaikki arvot väliltä 0..4.

Huomaa, että koodilohkon alku ja loppu voidaan merkata myös aaltosuluilla:

```ruby
5.times { |i|
  puts "Kierros #{i}"
}
```

Rubyssä on myös while- ja for-toistolauseet. Sama esimerkki for:illa:

```ruby
for i in 0..4
  puts "Kierros #{i}"
end
```

Tässä `0..4` vastaa suljettua väliä [0,4] ja for iteroi välin läpi siten, että muuttuja `i` saa arvokseen yhden kerrallaan välin arvoista. Tulostus on sama kuin edellisessä.

Muitakin vaihtoehtoja toiston toteuttamiseen on, mutta emme käy niitä nyt läpi. Tärkeintä on osata muutama omaa käyttötarkoitusta varten, loput voi katsoa [esimerkiksi täältä](http://www.tutorialspoint.com/ruby/ruby_loops.htm).

Rubyssä käytetään erittäin harvoin for- ja while-toistolausekkeita. Jos huomaat käyttäväsi niitä, tiedät että koodisi ei ole kovin Rubymaistä.

> #### [Tehtävä 3](https://github.com/HY-TKTL/ruby-tehtava3)
> Tee metodi `kertoma`, joka palauttaa annetun luvun kertoman.
> Jos et muista, tarkista Wikipediasta mikä on kertoma

### Taulukko

Ehkä monikäyttöisin tietorakenne Rubyssä on taulukko eli `Array`.
Taulukko käyttäytyy pitkälti kuten Javan ArrayList, mutta tarjoaa kuitenkin huomattavasti laajemman operaatiovalikoiman. Rubyn dynaamisen tyypityksen ansiosta taulukkoon talletettavat arvot voivat olla mitä tahansa tyyppiä.

Taulukon luominen helppoa:

```ruby
taulukko = [1, "kaksi", 3, false]
```

Taulukkoa voidaan indeksoida `[]` operaattorilla tai komennolla `at`. Jälleen huomaamme, että saman asian tekemiseen on Rubyssä monta tapaa.

```ruby
irb(main):001:0> taulukko = [1, "kaksi", 3, false]
=> [1, "kaksi", 3, false]
irb(main):002:0> taulukko[1]
=> "kaksi"
irb(main):003:0> taulukko.at(3)
=> false
```

Taulukon loppuun lisäämiseen voidaan käyttää metodia `push` tai operaattoria `<<`, ja mielivaltaiseen kohtaan lisäämiseen metodia `insert`.

```ruby
irb(main):001:0> taulukko = [1, "kaksi", 3, false]
=> [1, "kaksi", 3, false]
irb(main):002:0> taulukko << 2
=> [1, "kaksi", 3, false, 2]
irb(main):003:0> taulukko.insert(2, "kolme")
=> [1, "kaksi", "kolme", 3, false, 2]
```

Jos halutaan tietää, sisältyykö jokin asia taulukkoon, voidaan käyttää metodia `include?`. Rubyssä on konventiona, että kaikki metodit, joiden on tarkoitus palauttaa totuusarvo päättyvät kysymysmerkkiin.

```ruby
irb(main):004:0> taulukko.include? "kolme"
=> true
```

> #### [Tehtävä 4](https://github.com/HY-TKTL/ruby-tehtava4)
> Tee metodi `tulosta`, joka saa parametrina taulukon ja tulostaa sen jokaisen alkion. Kaikki alkiot tulostetaan samalle riville.

> #### [Tehtävä 5](https://github.com/HY-TKTL/ruby-tehtava5)
>Tee metodi `puuttuva`, joka saa parametrina taulukon, jonka pituus on n. Taulukossa on kertaalleen yhtä lukuunottamatta jokainen välin 0..n luku. Metodin tulee palauttaa puuttuva luku.
>
>Esim. parametrilla `[1,2,4,0]` metodin kuuluisi tulostaa 3.


Ruby tarjoaa taulukkojen läpikäyntiin ja käsittelyyn useita eri metodeja, ks. [http://ruby-doc.org/core-2.3.0/Array.html](http://ruby-doc.org/core-2.3.0/Array.html)

Yksi näistä on `map`, joka saa parametrikseen _koodilohkon_. Metodi palauttaa uuden taulukon, jonka sisältönä ovat alkiot, jotka ovat tuloksena koodilohkon suorittamisesta alkuperäisen taulukon alkioille.

Tarkastellaan esimerkkiä.

```ruby
taulu = [2,4,6,8]
taulu2 = taulu.map { |alkio| alkio + 3 }
puts taulu2
```
Tulostuu

```ruby
5
7
9
11
```

Huomaa, että metodin parametrina oleva koodilohko olisi voitu kirjoittaa myös aloittaen `do`:lla ja päätten `end`:iin.

```ruby
taulu = [2,4,6,8]
taulu2 = taulu.map do |alkio|
  alkio + 3
end
puts taulu2
```

Konventiona Rubyssä on kirjoittaa lyhyet, samalle riville metodikutsun kanssa kirjoitettavat koodilohkot aaltosuluilla ja muut 'do/end':illä erotettuina.

> #### [Tehtävä 6](https://github.com/HY-TKTL/ruby-tehtava6)
> Tee metodi `monista`, joka saa parametrina kokonaislukutaulukon ja palauttaa merkkijonotaulukon, jonka alkiot on muodostettu parametrina olevan taulukon alkioiden perusteella siten, että luku 1 muuttuu merkkijonoksi "1", luku 2 merkkijonoksi "22" jne.
>
> Esim. kutsuttaessa `monista([1,5,2])` palauttaa metodi taulukon `["1","55555", "22"]`
>
>
>Käytä metodia `map`.


Jos halutaan valita taulukosta vain jonkin ehdon täyttävät alkiot, voidaan käyttää metodia `select`, joka ottaa parametrinaan koodilohkon totuusarvon palauttavan koodilohkon. Metodi karsii taulukosta ne, joille koodilohko epätosi (false tai `nil`).

Kokeillaan:

```ruby
irb(main):001:0> taulu = [1, 2, "kolme", 4]
=> [1, 2, "kolme", 4]
irb(main):002:0> taulu.select{ |x| x.is_a? String}
=> ["kolme"]
```

Usein on tarve laskea taulkon alkioiden perusteella jokin yksittäinen arvo, esim. alkioiden summa. Näihin tilanteisiin sopii hyvin metodi `inject`. Inject kokoaa yhteen kaikki alkiot metodin parametrinaan saaman koodilohkon määräämällä tavalla.

Taulukon summan voisi laskea seuraavasti:

```ruby
irb(main):001:0> a = [3,9,11,2]
=> [3, 9, 11, 2]
irb(main):002:0> a.inject(0){ |tulos, x| tulos + x }
=> 25
```

Koodissa muuttujaan `tulos` summataan yksi kerrallaan jokainen taulukon alkioista. Jokainen koodilohkon suoritus palauttaa jo laskettujen alkioiden summan. Muuttuja `tulos` saa alkuarvokseen 0, eli edellisen esimerkin tilanteessa koodilohkossa lasketaan aluksi 0+3. Seuraavalla iteraatiolla muuttujan `tulos` arvona on 3 ja koodilohko palauttaa laskun 3+9 arvon, jne.

Koodi voitaisiin kirjoittaa myös seuraavalla, hieman "epärubymaisella" tavalla:

```ruby
def summa(taulu)
  summa = 0
  taulu.each do |alkio|
    summa += alkio
  end
  summa
end
```

Aiempi ratkaisu on kuitenkin huomattavasti elegantimpi.

> #### [Tehtävä 7](https://github.com/HY-TKTL/ruby-tehtava7)
> Tee metodi `keskiarvo`, joka saa parametriksi kokonaislukuja sisältävän taulukon ja palauttaa taulukon lukujen keskiarvon. Käytä metodia `inject`

> #### [Tehtävä 8](https://github.com/HY-TKTL/ruby-tehtava8)
> Tee metodi `osa`, joka saa parametriksi kokonaislukuja sisältävän taulukon ja palauttaa uuden taulukon, jonka sisältää alkuperäisen taulukon alkioista ne, jotka ovat suurempia kuin alkuperäisen taulukon ensimmäinen luku. Käytä metodia `reject`
ks. [http://ruby-doc.org/core-2.3.0/Array.html](http://ruby-doc.org/core-2.3.0/Array.html)

Palataan vielä summan laskevaan metodiin. Metodi on mahdollista tehdä vielä edellä ollutta helpommin, käyttämällä valmista plus-operaattoria.

```ruby
irb(main):001:0> [2,3,5,9].inject(:+)
=> 19

```

Koodissa annetaan plus-operaattorin _symbooli_ parametriksi injectille, joka osaa päätellä että haluamme tehdä jokaiselle alkiolle juuri lisäysoperaation. `:`-operaattori tarkoittaakin symboolia, joka on eräänlainen merkkijono-vakio. Symboolin voi luoda helposti:

```ruby
irb(main):001:0> :vakio
=> :vakio
irb(main):002:0> :vakio[2]
=> "k"
```

Toisin kuin merkkijonoa, symboolia ei voi muuttaa:

```ruby

irb(main):002:0> a = "omena"
=> "omena"
irb(main):003:0> a[0,1] = "no"
=> "no"
irb(main):004:0> a
=> "nomena"
irb(main):005:0> b = :omena
=> :omena
irb(main):006:0> b[2] = "n"
NoMethodError: undefined method `[]=' for :omena:Symbol
  from (irb):6
  from /home/user/.rbenv/versions/2.2.0/bin/irb:11:in `<main>'
```

Symbooli onkin huomattavasti tehokkaampi kuin merkkijono, eli `String`, koska samaan symboliin viittaaminen viittaa koko ohjelman suorituksen ajan samaan olioon. On siis hyödyllistä käyttää symbooleita, jos ei tarvita muita merkkiojonojen ominaisuuksia, kuten esimerkiksi muuttamista.

### hash

On useita tilanteita, joissa taulukko ei ole optimaalinen tietorakenne. Jos halutaan tallentaa avain-arvo pareja, on huomattavasti helpompi ratkaisu käyttää tietorakennetta nimeltä `hash`, ks. [http://ruby-doc.org/core-2.3.0/Hash.html](http://ruby-doc.org/core-2.3.0/Hash.html)

Hash luodaan seuraavasti:

```ruby
irb(main):001:0> hash = {}
=> {}
```

Hashin tehtävä on siis tallentaa avaimelle jokin arvo. Rubyssä arvot ja avaimet voivat olla mitä tahansa tyyppiä.

```ruby
irb(main):002:0> hash["viisi"] = 5
=> 5
irb(main):003:0> hash[2] = "kaksi"
=> "kaksi"
irb(main):004:0> hash[3] = ["kolme", 3]
=> ["kolme", 3]
```

Kuten huomataam, hashin arvo voi olla myös taulukko.

Avainta vastaavan arvon haku hash-rakenteesta helppoa:

```ruby
irb(main):005:0> hash["viisi"]
=> 5
irb(main):006:0> hash[3]
=> ["kolme", 3]
```

> #### [Tehtävä 9](https://github.com/HY-TKTL/ruby-tehtava9)
> Tee metodi `esiintymat`, joka saa parametriksi taulukon ja palauttaa hashin, jonka avaimina ovat taulukon alkiot ja arvoina alkioiden esiintymislukumäärät taulukossa

> #### [Tehtävä 10](https://github.com/HY-TKTL/ruby-tehtava10)
> Tee metodi `avaimien_summa`, joka saa parametriksi hashin, jonka kaikki avaimet ovat kokonaislukuja, ja palauttaa sen avaimien summan. Etsi sopivia metodeja [Hashin apikuvauksesta](http://ruby-doc.org/core-2.3.0/Hash.html)

> #### [Tehtävä 11](https://github.com/HY-TKTL/ruby-tehtava11)
> Tee metodi `arvojarjestys`, joka saa parametriksi hashin, jonka kaikki arvot ovat kokonaislukuja, ja palauttaa taulukon, jossa on sen nollaa suuremmat arvot suuruusjärjestyksessä. Sopivia metodeja löytyy taulukon ja hashin apikuvauksesta.

Merkkijonojen sijaan hashin avaimina kannattaa käyttää aiemmin esiteltyjä symbooleja:

```ruby
irb(main):001:0> h = {}
 => {}
irb(main):002:0> h[:luku] = 10
 => 10
irb(main):003:0> h[:merkkijono] = "abba"
 => "abba"
 irb(main):003:0 > h
 => {:luku=>10, :merkkijono=>"abba"}
```

Joissain tilanteissa voi olla myös tarve muuttaa merkkijono symboliksi:

```ruby
> "a".to_sym
 => :a
```

> #### [Tehtävä 12](https://github.com/HY-TKTL/ruby-tehtava12)
> Tee metodi `luokittelu`, joka saa parametriksi kokonaislukutaulukon ja palauttaa taulukon arvojen perusteella muodostettavan hashin, jolla on seuraavat avaimet ja arvot
* :negatiivinen, negatiivisten lukujen lista
* :positiivinen, positiivisten lukujen lista
* :parillinen, parillisten lukujen lista
* :pariton, parittomien lukujen lista
* :summa, taulukon lukujen summa
>
> lukujen tulee olla suuruusjärjestyksessä metodien palauttamilla listoilla
