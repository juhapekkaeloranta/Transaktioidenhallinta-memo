## Lokin ylläpito ja puskurihallinta

### Loki

1. Transaktioiden peruutus
2. Elvytys

### Lokin sisältö
* Transaktion
  * **päivitys**operaatiot: I, D ja käänt.
  * **ohjaus**operaatiot: B, C, A ja checkpointit
* **LSN** = **L**og **S**equence **N**umber
  * nouseva numerointi (keskitetyssä)
  * samalla osoite lokikirjaukseen
* Muotoa:
  * `n: [T, o[x], n']`
    * `n`: LSN
    * `T`: transaktio
    * `o[x]`: kirjattava operaatio + argumentit
    * `n'`: T:n edellinen/seuraava lokikirjaus
      * edellinen: normaalissa etenemisoperaatiossa
      * seuraava: käänteisoperaatiossa - "seuraavaksi peruutettava"

### Fyysinen / Looginen lokikäytäntö

* Fyysinen = arvoperustainen = value logging = physical logging
  * Sivunumero (päivitettävän sivun)
  * Sijainti (muutettujen tavujen)
  * **Before image** = sisältö ennen (muutettujen tavujen)
  * **After image** = Sisältö jälkeen (muutettujen tavujen)
* Looginen = logical logging
  * loogiset operaatiot
  * I[x,v], D[x,v] jne.
  * Esim: `n: T, I, x, v, n'`
  * _Huom! Tehoton_

* Fyysis-looginen = fysiologinen = **psysiological logging**
  * Sivut: fyysinen loki
  * Monikko: looginen loki
  * _Suoritusjärjestys kriittinen!_
  * Peruutus:
    * fyysinen ei aina mahdollinen - sivu muuttunut (täyttynyt)
    * tällöin **looginen peruutus**

* **Active-transaction table** = transaktiotaulu
  * Nopea: keskusmuistissa
  * Sisältö:
    * T id
    * T state (fw/bw -rolling)
    * Undo-next-LSN = peruutuspinon huipun osoite
  * Myös:
    * linkki transaktion lukkoihin
    * muuta tarpeellista transaktion käsittelyyn liittyvää
  * Alustus:
    * Perustetaan ja alustetaan järjestelmän käynnistyessä
  * 1. transaktio:
    * aloituskirjaus: `[T, B]`
    * transaktiotauluun: `(T, fw-rolling, n)`
      * missä `n` = aloituskirjaukset Undo-Next-LSN

* Lokikirjaustyypit
  * **redo-undo log record** = toisto- ja peruutuskelpoisia
  * **redo-only log record** = vain toisto: käänteisoperaatiot
    * Käänteisoperaatiota ei koskaan peruta
    * => nimitys: korvaava lokikirjaus = **compensation log record** = CLR
 
* Avainvälimallin lokikirjaukset
  * (T, I, p, r(x, v), n)
    * T = Transaktio
    * I = operaatiot
    * p = page
    * r = relaatio (taulu)
    * x, v = attribuutit
    * n = seuraava/edellinen lokikirjaus
    
* Rakennemuutosten loki
  * [allocate-page-for-heap, s, f, p, q]

* WAL-käytäntö
