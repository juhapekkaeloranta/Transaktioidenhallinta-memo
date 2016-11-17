## WAL-käytäntö

**Loki levylle ennen päivitettyä sivua!**
* Sivulla p Page-LSN, lokeilla LSN
  * lokit joiden LSN pienempi p:n Page-LSN levylle juuri ennen p:n committia
```
commit(T) {
  log(n [T, C]);
  flush-the-log();
  vapauta T:n varaamat lukot;
  poista T transaktiotaulusta;
}
```

## Puskurinhallintakäytännöt
* Varasta / Älä varasta (no-steal policy)
  * Aktiivisia transaktioita pois puskurista?
* Pakota / Älä pakota
  * Sitoutuva transaktio levylle?
* Kurssilla:
  * Varasta ja älä pakota

## Jaettu puskuri
* Pitkäaikais ja lyhytaikais
  * molemmissa omat LRU:t
  * esim mySql innoDB

## Liitos
* 2 taulun luonnollinen liitos, **m**x**n**
* 2 sisäk. for silmukkaa
* käytettään 2 kehystä
  * ulommain for:in sivu pysyy n askelta samassa ruudussa
  * sisäsempi vaihtuu kokoajan

## Osittaisperuutus
* Ks algoritmi materiaalista

## Järjestelmähäiriöt, romahdus
* Laitteiston toimintahäiriö, sähkökatko, ohjelmistovirhe (dbms tai os)
* RAM tyhjenee
* Sitoutuneet:
  * **Toistetaan** lokin perusteella sitoutuneet transaktiot (jotka ei levyllä)
* Aktiiviset (ei-vielä sitoutuneet):
  * **Perutaan** lokin perusteella mahdolliset sen tehneet muutokset

_Selvitä:_
- Kaavio transaktiosta levy ja lokikirjauksineen ja tähän skenariot mahdollisista romahduspisteistä
- Mitä dbms palauttaa jos T ei pääse commit asti? Tällöin sovellusohjelman ajettava T uudestaan
- Rec-LSN lisäominaisuus vai kriittinen?

## Sumea tarkistuspiste
* = epäsuora tarkistuspiste = fuzzy checkpoint
* minimaalinen:
  * ei yhtään sivua levylle
  * mutta levylle: päivitettyjen sivujen taulu, transaktiotaulu

## ARIES
* **A**lgorith for **R**ecovery and **I**solation **E**xploiting **S**emantics
1. Analyysivaihe
2. Toistovaihe
3. Peruutusvaihe
4. Tarkistuspisteen otto

1. Analysis pass
  * Häiriöhetkellä; mitkä:
    * Transaktiot aktiivisina
    * Sivut päivitettyinä puskurissa
  * Luetaan lokia / T-taulua / p-taulua
  * Ei kirjoiteta levylle

2. Toistovaihe

## Koetehtävä:
Simuloi ARIES-algoritmia lokin perusteella
