### Tehtävä 1

Mistä monikkokohtaisista operaatioista transaktio koostuu?

```
exec sql select max(empnr) into :e from employee;
exec sql select max(deptnr) into :d from department;
exec sql insert into department values (:d +1, ’Research’, :e);
exec sql insert into employee values (:e+1, ’Jones, Mary’, ’Sisselenk. 2, Helsinki’,
’research director’, 3500, :d +1);
exec sql update department set managernr = :e+1 where deptnr = :d +1;
exec sql insert into employee values (:e+2, ’Smith, John’, ’Rouvienp. 11, Helsinki’,
’researcher’, 2500, :d +1);
exec sql commit.
```

Ilman hakemistoja:
Riveittäin:
1. Käy läpi kaikki employerin rivit - lukee ja vertaa.
2. Käy läpi kaikki employerin rivit - lukee ja vertaa.
3. Lisää uuden rivin (uusi osasto)
4. Lisää uuden rivin (uusi työntekijä)
5. Käy läpi `deparment` taulun monikkoja, kunnes löytää äsken lisätyn.
  Päivittää taulun attribuuttia `managernr`
6. Lisää uuden rivin
7. (commit)

Hakemistoilla:
Riveittäin:
1. Hakee maksimin enint. k-askeleella, missä k hakupuun korkeus
2. Hakee maksimin enint. k-askeleella, missä k hakupuun korkeus
3. Lisää uuden rivin (uusi osasto)
4. Lisää uuden rivin (uusi työntekijä)
5. Hakee halutun `deptnr` k-askeleella `deparment` taulusta
  Päivittää taulun attribuuttia `managernr`
6. Lisää uuden rivin
7. (commit)

_Loogisesti oikeellisia?_
Kyllä:
* Uudet pääavaimet uniikkeja (max + 1, max + 2)
* Vierasavaimet ok
* Attribuuttien arvot ok
* `NOT NULL` rajoitteet ok

### Tehtävä 2

_"Miten seuraavien SQL-ohjelmanosien tuottamat, relaatioon r(X,V) kohdistuvat transaktiot voidaan
kuvata luennoilla esitetyssä luku-kirjoitusmallissa? Entä avainvälimallissa?"_

a) `update r set V = V +1 where X = x; update r set V = V +1 where X = y; commit`

b) `update r set V = V +1 where X = x; update r set V = V +1 where X = y; rollback`

lukukirjoitus:
a) BW[x, V, V+1]W[y, V, V+1]C

b) BW[x, V, V+1]W[y, V, V+1]AW<sup>-1</sup>[y, V, V+1]W<sup>-1</sup>[x, V, V+1]C

avainvälimalli:
a) BR[X, >= x, v]D[X]I[X, v+1]R[X, >= y, v]D[X]I[X, v+1]C
b) BR[X, >= x, v]D[X]I[X, v+1]R[X, >= y, v]D[X, v]I[X, v+1]
  D[X, v+1]I[X, v]

### Tehtävä 3

1. Aloita
2. Insert (x1, v1) relaatioon r
3. Aseta peruutuspiste P1
4. Insert (x2, v2) relaatioon r
5. Aseta peruutuspiste P2
6. Insert (x3, v3)
7. Aloita osittaisperuutus pisteeseen P2
8. Peruuta insert (x3, v3)
9. Päätä osittaisperuutus P2
10. Insert (x4, v4)
11. Aloita osoittaisperuutus pisteeseen P1
12. Peruuta insert (x4, v4)
13. Peruuta insert (x2, v2)
14. Päätä osittaisperuutus P1
15. Insert (x5, v5)
16. Keskeytä transaktio
17. Peruuta insert (x5, v5)
18. Peruuta insert (x1, v1)
19. Commit

```
INSERT VALUES (x1, v1) INTO r;
SET SAVEPOINT p1;
INSERT VALUES (x2, v2) INTO r;
SET SAVEPOINT p2;
INSERT VALUES (x3, v3) INTO r;
ROLLBACK TO SAVEPOINT p2;
INSERT VALUES (x4, v4) INTO r;
ROLLBACK TO SAVEPOINT p1;
INSERT VALUES (x5, v5) INTO r;
ROLLBACK;
COMMIT;
```
