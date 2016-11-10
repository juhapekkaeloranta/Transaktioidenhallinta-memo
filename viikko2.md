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
  
  
  
  
  
  
  
  
  
