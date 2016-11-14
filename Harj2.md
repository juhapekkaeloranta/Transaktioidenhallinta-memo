Teht 5

Voiko tietokannan hallintajärjestelmä samalla kertaa soveltaa sekä älä varasta -käytäntöä
että pakota-käytäntöä?

_Älä varasta_ ja _pakota_ käytännöt määrittävät päivitetyn (likaisen) sivun p levylle kirjoitusajankohdan.

* Älä varasta
  * Levylle vain commit jälkeen
  * (ei yhtäkään aktiivista T joka kohdistuu sivuun p
* Pakota
  * Pakota levylle juuri ennen commitin voimaan tuloa

=> Ristiriita: Pakota-käytännössä likainen sivu viedään levylle sillä sitä päivittänyt T ei ole (aivan) vielä sitoutunut.

