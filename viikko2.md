## 2. Fyysinen tietokanta

### Tietokokoelmat
* **Pysyvät (2kpl)**
 * Tietolevyt
 * Lokilevyt
* **Katoavia**
  * buffer pool
    * sivujen käsittely
  * log buffer
    * lokien käsittely
  * lock table
    * samanaikaisuuden hallinta
  * query-plan cache
    * suoritussuunnitelmia muistissa
    
### Prosessit transaktiopalvelimella
* **Server** process
  * sovellusprosessien pyyntöjen palvelu
* **DB-writer** process
  * puskuri => levy
* **log-writer** process
  * loki: puskuri => levy
* **Checkpoint** process
  * tarkistuspisteitä silloin tällöin
* **Process-monitor** process
  * valvoo muita, suorittaa elvytykset, keskeyttää, peruuttaa..
* **Lock-manager** prosess
  * lukon varaus- ja vapautus, lukkiumavahti

**Samanaikaisuudesta**
* Tehokkuutta lisää? => _server process_ käsittelee suoraan _locktablea_ eikä _lock-managerin_ kautta
* => Pitää olla semafori (tms) _locktablella_ ja muilla yhteismuistirakenteilla

### Levyjaksot
* **Sektori**
  * pienin osoitetta yksikkö levyllä
* **Levyjakso/-lohko/-block**
  * peräkkäisiä sektoreita
* **Sivu**
  * 2/4/8/16/32 KB
  * jaksossa/peräkkäisissä jaksoissa
* **Tietokanta**
  * Kokoelma sivuja

## Sivut
* **Tietosivu/datapage**
  * monikkoja
* **Hakemistosivu/indexpage**
  * hakemistot
* **Muita sivuja**
  * tietokannan hallinnan tarpeisiin

### Sivun osat

* **Otsikkotietue**
  * Tyyppi
    * _relation data page_
    * _cluster data page_
    * _index page_
    * _free space_
    * _data-dictionary page_
    * _storage-map page_
  * Tunniste
  * Hakemiston koko (n)
  * Vapaa tila
    * pisin yhtenäinen
    * yhteensä
  * Page-LSN
    * (page log-sequence number)
    * ("latest log id")

* **Tietuealue**
  * varsinainen sisältö (monikot)
  
* **Tietuehakemisto**
  * taulukko m
  * m[i] = i:n "indeksi" (tavuina sivun alusta)
  * Sijoitettu sivun loppuun (kasvaa lopusta päin)

### Tunnisteista

* **PID** sivutunniste 
  * Sivun numero ja järj.nro. tiedostossa
  * lisäksi site/node jos hajautettu
* **File descriptor** tiedostokuvaaja
  * Tiedostolle varattu alue levyllä
  * Missä sivu? -> file descriptor -> levyjakson osoite
* **RID** record identifier
  * (p,i) p=PID i=monikon indeksi sivulla

### Puskureista
  
* **Buffer pool**
  * koostuu: **buffer frame**istä
  * sivu oltava puskurissa käsittelyä varten
  * siirto puskuriin: naulinta (fix, pin)
  * naulinnan vapautuksen tekee kutsunut prosessi!
  * sivu poistuu puskurista LRU mukaisesti
* **Buffer control block**
  * keskusmuistirakenne
  * sivun tunniste, sijainti, pin-counter, ...
  * **Puskurinhallitsin** ylläpitää
* **Pin**
  * read:
    * read-latch sivulle (pin ajaksi)
    * write-lock monikolle (commit asti)
  * update:
    * write-latch sivulle
    * write-lock monikolle

### Tietokannan tila
 
* Levyversio
* Puskuriversio
* Nykyversio

* Elvytys - **restart recovery**
  * Puskuri (ram) häviää
  * Lokissa tallessa sitoutuneet transaktiot
  * Luodaan uudestaan transaktiot
 
* Tarkistuspiste - **checkpoint**
  * esim 5-10 min
  * osa sivuista levylle
  * täydellinen = kaikki levylle
  * indirect/fuzzy checkpoint: osa levylle
 
* Tiedoston vapaa tila
  * harva B-puu: indeksointiattribuutti määrittää uuden sivun sijoitussivun
  * sivu täynnä? => sivu halki
  * tyhjien sivujen ketju
  * space map

* Tilan eheys
  * Sisäinen tietorakenne eheä
  * Sivut muodostavat tiedostorakenteen mukaisen verkkorakenteen
    * puun tasapainoehdot jne
  * Eheysvaatimukset vain nykyversiolle

* Salpauskäytäntö
  * Read-latch
    * kaikki saavat lukea
    * kukaan ei voi kirjoittaa
  * Write-latch
    * Lukitsija voi kirjoittaa
    * Muut eivät voi edes lukea
  * Unlatch _Latcher must release it's latches_
  * Toteutus: semafori
    * jonotus
  * Ei lukkiutumien havaitsemista
  * Täytyy estää lukkiumat
  
* Konttaus - **Latch-coupling**
  * "Lukuketju" esim B-puussa
  * siirtymähetkellä 2 latchiä

* Rakennemuutokset
