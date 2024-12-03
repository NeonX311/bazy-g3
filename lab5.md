# bazy danych lab5
## zadanie 1
### 1.
```
CREATE TABLE ekwipunek
AS (SELECT * FROM wikingowie.ekwipunek);

CREATE TABLE zasob
AS (SELECT * FROM wikingowie.zasob);

CREATE TABLE kreatura
AS (SELECT * FROM wikingowie.kreatura);
```
### 2.
```
SELECT * FROM zasoby;
```
### 3.
```
SELECT * FROM zasob WHERE rodzaj = 'jedzenie';
```
### 4.
```
SELECT z.idZasobu, z.ilosc FROM zasob AS z
INNER JOIN ekwipunek AS e ON e.idZasobu = z.idZasobu
WHERE e.idKreatury IN (1, 3, 5);
```
## zadanie 2
### 1.
```
SELECT * FROM kreatura
WHERE rodzaj != 'wiedzma' AND udzwig > 49;
```
### 2.
```
SELECT * FROM zasob WHERE waga BETWEEN 2 AND 5;
```
### 3.
```
SELECT * FROM kreatura
WHERE nazwa LIKE '%or%' AND udzwig BETWEEN 30 AND 70;
```
## zadanie 3
### 1.
```
SELECT * FROM zasob
WHERE MONTH(dataPozyskania) IN (7,8);
```
### 2.
```
SELECT * FROM zasob
WHERE rodzaj IS NOT NULL
ORDER BY waga ASC;
```
### 3.
```
SELECT * FROM kreatura
WHERE dataUr IS NOT NULL
ORDER BY dataUr ASC LIMIT 5;
```
## zadanie 4
### 1.
```
SELECT DISTINCT(rodzaj) FROM zasob;
```
### 2.
```
SELECT CONCAT(nazwa, ' ', rodzaj) AS 'nazwa - rodzaj' FROM kreatura
WHERE rodzaj LIKE 'wi%';
```
### 3.
```
SELECT nazwa, (ilosc * waga) AS 'calkowita waga' FROM zasob
WHERE YEAR(dataPozyskania) BETWEEN 2000 AND 2007;
```
## zadanie 5
### 1.
```
SELECT nazwa, (waga * 0.7) AS 'masa wlasciwa', (waga * 0.3) AS 'waga odpadow' FROM zasob
WHERE rodzaj = 'jedzenie';
```
### 2.
```
SELECT * FROM zasob
WHERE rodzaj IS NULL;
```
### 3.
```
SELECT DISTINCT(rodzaj) FROM zasob
WHERE nazwa LIKE 'Ba%' OR nazwa LIKE '%os';
```
