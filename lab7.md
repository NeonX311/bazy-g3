# bazy danych lab7
## zadanie 1
### 1.
```
CREATE TABLE uczestnicy
AS (SELECT * FROM wikingowie.uczestnicy);

CREATE TABLE etapy_wyprawy
AS (SELECT * FROM wikingowie.etapy_wyprawy);

CREATE TABLE sektor
AS (SELECT * FROM wikingowie.sektor);

CREATE TABLE wyprawa
AS (SELECT * FROM wikingowie.wyprawa);
```
### 2.
```
SELECT nazwa FROM kreatura AS k
LEFT JOIN uczestnicy AS u ON k.idKreatury = u.id_uczestnika
WHERE id_uczestnika IS NULL;
```
### 3.
```
SELECT nazwa, SUM(ilosc) AS ilosc_ekwipunku_uczestnikow FROM wyprawa AS w
INNER JOIN uczestnicy AS u ON  u.id_wyprawy = w.id_wyprawy
INNER JOIN ekwipunek AS e ON e.idKreatury = u.id_uczestnika
GROUP BY nazwa;
```
## zadanie 2
### 1.
```
SELECT w.nazwa, COUNT(DISTINCT(idKreatury)) AS liczba_uczestnikow, GROUP_CONCAT(k.nazwa SEPARATOR ', ') FROM wyprawa As w
INNER JOIN uczestnicy AS u ON  u.id_wyprawy = w.id_wyprawy
INNER JOIN kreatura AS k ON k.idKreatury = u.id_uczestnika
GROUP BY w.nazwa;
```
### 2.
jeszcze do zrobienia
## zadanie 3
### 1.
```
SELECT s.nazwa, COUNT(e.sektor) AS ilosc_odwiedzen_sektora FROM sektor AS s
LEFT JOIN etapy_wyprawy AS e ON e.sektor = s.id_sektora
GROUP BY s.nazwa;
```
### 2.
do poprawy (wyświetla tylko kreatury, które poszły na wyprawę)
```
SELECT k.nazwa, IF(COUNT(w.nazwa) IS NOT NULL, 'brał udział w syprawie', 'nie brał udziału w wyprawie') FROM kreatura As k
LEFT JOIN uczestnicy AS u ON k.idKreatury = u.id_uczestnika
LEFT JOIN wyprawa AS w ON  u.id_wyprawy = w.id_wyprawy
GROUP BY k.nazwa
```
## zadanie 4
### 1.
```
SELECT w.nazwa, SUM(LENGTH(e.dziennik)) AS dlugosc_dziennika FROM wyprawa AS w
RIGHT JOIN etapy_wyprawy AS e ON w.id_wyprawy = e.idWyprawy
GROUP BY w.nazwa
HAVING dlugosc_dziennika < 400
```
### 2.
```
SELECT w.nazwa, (SUM(z.waga * e.ilosc)/COUNT(DISTINCT(u.id_uczestnika))) FROM wyprawa AS w
INNER JOIN uczestnicy AS u ON w.id_wyprawy = u.id_wyprawy
INNER JOIN kreatura AS k ON u.id_uczestnika = k.idKreatury
INNER JOIN ekwipunek AS e ON k.idKreatury = e.idKreatury
INNER JOIN zasob AS z ON e.idZasobu = z.idZasobu
GROUP BY w.nazwa
```
## zadanie 5
```
SELECT k.nazwa, CONCAT(DATEDIFF(data_rozpoczecia, dataUr), ' lat') AS wiek_w_momecie_rozpoczecia_wyprawy FROM kreatura AS k
INNER JOIN uczestnicy AS u ON u.id_uczestnika = k.idKreatury
INNER JOIN wyprawa AS w ON w.id_wyprawy = u.id_wyprawy
INNER JOIN etapy_wyprawy AS e ON e.idWyprawy = w.id_wyprawy
INNER JOIN sektor AS s ON s.id_sektora = e.sektor
WHERE s.nazwa = 'Chatka dziadka';
```
