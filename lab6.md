# bazy danych lab6
## zadanie 1
### 1.
```
SELECT AVG(waga) FROM kreatura;
```
### 2.
```
SELECT rodzaj, COUNT(rodzaj) AS ilosc, AVG(waga) AS srednia_waga FROM kreatura
GROUP BY rodzaj;
```
### 3.
```
SELECT rodzaj, AVG(YEAR(NOW())-YEAR(dataUr)) AS sredni_wiek FROM kreatura
GROUP BY rodzaj;
```
## zadanie 2
### 1.
```
SELECT rodzaj, AVG(waga) AS srednia_waga FROM zasob
GROUP BY rodzaj;
```
### 2.
```
SELECT nazwa, AVG(waga) FROM zasob
WHERE ilosc > 3
GROUP BY nazwa
HAVING SUM(waga) > 10;
```
### 3.
```
SELECT rodzaj, COUNT(nazwa) FROM zasob
WHERE ilosc > 1
GROUP BY rodzaj;
```
## zadanie 3
### 1.
```
SELECT k.nazwa, SUM(e.ilosc) AS ilosc_przedmiotow_w_ekwipunku FROM kreatura AS k
INNER JOIN ekwipunek AS e ON k.idKreatury = e.idKreatury
GROUP BY nazwa;
```
### 2.
```
SELECT k.nazwa, z.nazwa FROM kreatura AS k
INNER JOIN ekwipunek AS e ON k.idKreatury = e.idKreatury
INNER JOIN zasob AS z ON z.idZasobu = e.idZasobu
ORDER BY k.nazwa;
```
### 3.
```
SELECT k.nazwa FROM kreatura AS k
LEFT JOIN ekwipunek AS e ON k.idKreatury = e.idKreatury
WHERE idEkwipunku IS NULL;
```
## zadanie 4
### 1.
```
SELECT k.nazwa, z.nazwa, YEAR(dataUr) AS rok_ur FROM kreatura AS k
INNER JOIN ekwipunek AS e ON k.idKreatury = e.idKreatury
INNER JOIN zasob AS z ON z.idZasobu = e.idZasobu
WHERE YEAR(dataUr) LIKE "167_"
ORDER BY k.nazwa;
```
### 2.
```
*SELECT k.nazwa FROM kreatura AS k
INNER JOIN ekwipunek AS e ON k.idKreatury = e.idKreatury
INNER JOIN zasob AS z ON z.idZasobu = e.idZasobu
WHERE z.rodzaj = "jedzenie"
ORDER BY dataUr ASC LIMIT 5;
```
### 3.
```
SELECT CONCAT(k1.nazwa, ' ', k1.idKreatury), CONCAT(k2.nazwa, ' ', k2.idKreatury) FROM kreatura AS k1
INNER JOIN kreatura AS k2 ON k1.idKreatury + 5 = k2.idKreatury;
```
## zadanie 5
### 1.
```
SELECT k.rodzaj, AVG(z.waga*e.ilosc) AS sreadnia_waga_zasobow_w_ekwipunku FROM kreatura AS k
INNER JOIN ekwipunek AS e ON k.idKreatury = e.idKreatury
INNER JOIN zasob AS z ON z.idZasobu = e.idZasobu
WHERE k.rodzaj NOT IN ("waz","malpa") AND e.ilosc <30
GROUP BY k.rodzaj;
```
### 2.
Jeszcze do poprawy
```
SELECT k1.rodzaj,
(SELECT nazwa FROM kreatura AS k2
WHERE k2.idKreatury = k1.idKreatury
GROUP BY rodzaj
ORDER BY dataUr DESC LIMIT 1) AS nazwa_najmlodszej,
(SELECT dataUr FROM kreatura AS k2
WHERE k2.idKreatury = k1.idKreatury
GROUP BY rodzaj
ORDER BY dataUr DESC LIMIT 1) AS data_urodzenia_najmlodszej,
(SELECT nazwa FROM kreatura AS k3
WHERE k3.idKreatury = k1.idKreatury
GROUP BY rodzaj
ORDER BY dataUr ASC LIMIT 1) AS nazwa_najstarszej,
(SELECT dataUr FROM kreatura AS k3
WHERE k3.idKreatury = k1.idKreatury
GROUP BY rodzaj
ORDER BY dataUr ASC LIMIT 1) AS data_urodzenia_najstarszej 
FROM kreatura AS k1
GROUP BY rodzaj;
```
