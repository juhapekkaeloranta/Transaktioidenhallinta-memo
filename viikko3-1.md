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
  * 
* Pakota / Älä pakota
* Kurssilla:
  * Varasta ja älä pakota
