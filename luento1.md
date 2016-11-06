# Transaktioiden hallinta

### Käsitteitä
* Transaktiopalvelin = kyselypalvelin
* Transaktionkuljetus = kyselynkuljetus (shipping)
 = funktionkuljetus
* Palvelinprosessit
   * säikeet
   * 1 säie / asiakassovellusprosessi
* Embedded SQL
`connect to mydatabase`
* Java Database Connectivity
`static Connection DriverManager.getConnection(String mydatabase)`
* Looginen tietokanta = joukko relaatioita/tauluja
* Relaatio = joukko monikkoja/rivejä
* Sama rivi monta kertaa = monijoukko (multiset)
* Relaatiokaavio _r(Z)_
  * relaation nimi _r_
  * attribuuttien nimet ja tyyppimääritykset
  * avainrajoitteet
  * viite-eheysrajoitteet
  * mahd. muut rajoitteet
* Loogisia operaatiota
  * Monikon lisäys
  * Monikon poisto
  * Attribuutin päivitys
  * Ehdon täyttävän monikon lukeminen
  * Uuden relaation luonti
  * Tyhjän relaation poisto
* Transaktion hallinnan operaatioita
  * aloituskirjaus
  * sitoutumispyyntö
  * keskeytyspyyntö
  * peruutuksen päättymiskirjaus
  * (lisäksi käänteisoperaatiot)
* DBMS:
  * DML = Data manipulation language
  * DDL = Data definition language
  * molemmat yleensä SQL
  * => tuottavat loogiset operaatiot
* Tietohakemisto = järjestelmäluettelo
  * `create table`
  * `drop table`
* Tietokantarelaatioden sisältö
  * `insert`, `delete`, `update`
* Avainrajoitteet
  * `PRIMARY KEY`, `UNIQUE`
* _Referential integrity constraint_ = viite-eheysrajoite
  * `FOREIGN KEY`
* Eheys _integral, consistent_
  * DBMS: asetetut määritykset voimassa
  * Sovellus: mahdollisia muita rajoitteita
* Atomisuus:
  * Transaktio = tietokantaoperaatioden jono
  * Halutaan olevan atominen
  * Transaktio alkaa aloituskirjauksella _begin_ (uusissa SQL)
  * Transaktio päättyy sitoutumispyyntöön _commit_
* Keskeytys- ja peruutuspyyntö _rollback_
  * Suoritetaan _commit_ sijaan
  * Muutokset eivät tallennu tietokantaan
  * (Operaatiot perutaan)
* Sitoutunut
  * suorittaminut loppuun _commit_-operaation
* Pysyvyys
  * Sitoutunut transaktio toteuduttava ja jäätävä voimaan
  * (Häiriöistä huolimatta)
* Etenemisvaihe
  * Transaktion operaatiojonon suorittamisvaihe
  * Suoritetaan operaatio kerrallaan
  * Voi keskeyttää ja peruuttaa _rollback_-komennolla
  * Voi keskeytä - järjestelmä peruuttaa transaktion elvytysvaiheessa
* Tilat
  1. Etenevä - _forward-rolling transaction_
  2. Sitoutunut - _commited_
  3. Peruuntuva - _backward-rolling phase_
  4. Peruuntunut
* Peruutusvaihe
  * Käänteisoperaatiot suoritetaan täsmälleen päinvastaisessa järjestyksessä
  * (vrt. päivitysoperaatioden m.v. suoritusjärjestys)
* Peruuntunut transaktio
  * sis. peruutuksen päättymiskirjaus _rolled-back_
  * päättynyt _terminated_


**Oikeellinen transaktio**

* Tietokantasovellus:
  * Vastuulla loogisesti oikeelliset transaktiot _logically consistent_
    * Transaktio säilyttää eheän loogisen tietokannan eheänä
    * (yksinään häiriöttömässä tilassa)


* DBMS:
  * Huomioi myös:
    * Tietokannan fyysinen rakenne
    * Samanaikaiset transaktiot
    * Järjestelmähäiriöt


**ACID**

1. Atomisuus - _atomicity_
  * järjestelmä hoitaa
2. Oikeellisuus - _consistency_
  * transaktion ohjelmoijan vastuulla
3. Pysyvyys - _durability_
  * järjestelmä hoitaa
4. Eristyvyys - _isolation_
  * järjestelmä hoitaa

**Luku-kirjoitusmalli** - _read-write model_

(yksinkertainen teoreettinen malli)

Transaktio voi sisältää seuraavat operaatiot:
**B**: begins
**R[x]**: read row with keyvalue **x** from r
**W[x,v]**: write keyvalue **x** with value **v** to r
**C**: commit (or complete rollback if aborted)
**A**: abort

**Avainvälimalli** - _key-range model_
