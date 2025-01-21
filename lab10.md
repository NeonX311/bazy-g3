# bazy danych lab10
## zadanie 1
### 1.
```
SELECT imie, nazwisko, YEAR(data_urodzenia) AS rok_urodzenia FROM pracownik;
```
### 2.
```
SELECT imie, nazwisko, (YEAR(CURRENT_DATE()) - YEAR(data_urodzenia)) AS wiek FROM pracownik;
```
### 3.
```
SELECT d.nazwa, COUNT(*) AS ilosc_pracownikow_dzialu FROM dzial AS d
INNER JOIN pracownik AS p ON p.dzial = d.id_dzialu
GROUP BY d.nazwa
```
### 4.
```
SELECT k.nazwa_kategori, COUNT(*) AS ilosc_towaru FROM kategoria AS k
INNER JOIN towar AS t ON t.kategoria = id_kategori
GROUP BY k.nazwa_kategori
```
### 5.
```
SELECT k.nazwa_kategori, GROUP_CONCAT(t.nazwa_towaru) AS towary FROM kategoria AS k
INNER JOIN towar AS t ON t.kategoria = k.id_kategori
GROUP BY k.nazwa_kategori
```
### 6.
```
SELECT ROUND(AVG(pensja),2) AS srednia_zaobkow FROM pracownik;
```
### 7.
```
SELECT ROUND(AVG(pensja), 2) FROM pracownik
WHERE (YEAR(CURRENT_DATE) - YEAR(data_zatrudnienia)) >= 5;
```
### 8.1.
```
SELECT t.nazwa_towaru, COUNT(*) AS ilosc FROM zamowienie AS z
INNER JOIN status_zamowienia AS s ON s.id_statusu_zamowienia = z.status_zamowienia
INNER JOIN pozycja_zamowienia AS p ON p.zamowienie = z.id_zamowienia
INNER JOIN towar AS t ON t.id_towaru = p.towar
WHERE s.nazwa_statusu_zamowienia = 'zrealizowane'
GROUP BY t.nazwa_towaru
ORDER BY ilosc DESC
LIMIT 10;
```
### 8.2.
nie działa tak jak powinno chyba
```
SELECT t.nazwa_towaru, (COUNT(*) * p.ilosc) AS ilosc FROM zamowienie AS z
INNER JOIN status_zamowienia AS s ON s.id_statusu_zamowienia = z.status_zamowienia
INNER JOIN pozycja_zamowienia AS p ON p.zamowienie = z.id_zamowienia
INNER JOIN towar AS t ON t.id_towaru = p.towar
WHERE s.nazwa_statusu_zamowienia = 'zrealizowane'
GROUP BY t.nazwa_towaru
ORDER BY ilosc DESC
LIMIT 10;
```
### 9.
```
SELECT z.id_zamowienia, SUM(p.ilosc * p.cena) AS wartosc FROM zamowienie AS z
INNER JOIN pozycja_zamowienia AS p ON p.zamowienie = z.id_zamowienia
GROUP BY z.id_zamowienia
```
### 10.
```
SELECT p.id_pracownika, p.imie, p.nazwisko, ROUND(SUM(pz.ilosc * pz.cena), 2) AS wartosc_zamowien FROM pracownik AS p
INNER JOIN zamowienie AS z ON z.pracownik_id_pracownika = p.id_pracownika
INNER JOIN pozycja_zamowienia AS pz ON pz.zamowienie = z.id_zamowienia
GROUP BY p.id_pracownika
ORDER BY wartosc_zamowien DESC;
```
### 11.
```
SELECT d.nazwa, MIN(p.pensja) AS minimalna_pensja, MAX(p.pensja) AS maksymalna_pensja, ROUND(AVG(p.pensja), 2) AS srednia_pensja FROM dzial AS d
INNER JOIN pracownik AS p ON p.dzial = d.id_dzialu
GROUP BY d.nazwa
ORDER BY d.nazwa ASC;
```
### 12.
```
SELECT k.pelna_nazwa, ROUND(p.ilosc * p.cena) AS wartosc_zamowienia FROM klient AS k
INNER JOIN zamowienie AS z ON z.klient = k.id_klienta
INNER JOIN pozycja_zamowienia AS p ON p.zamowienie = z.id_zamowienia
WHERE nazwa_statusu_zamowienia = 'zrealizowane'
ORDER BY wartosc_zamowienia DESC
LIMIT 10;
```
### 13.
```
SELECT YEAR(z.data_zamowienia) AS rok, ROUND(SUM(p.ilosc * p.ilosc), 2) AS przychod FROM zamowienie AS z
INNER JOIN status_zamowienia AS s ON s.id_statusu_zamowienia = z.status_zamowienia
INNER JOIN pozycja_zamowienia As p ON p.zamowienie = z.id_zamowienia
WHERE nazwa_statusu_zamowienia = 'zrealizowane'
GROUP BY rok
ORDER BY przychod DESC;
```
### 14.
```
SELECT ROUND(SUM(p.ilosc * p.cena), 2) AS wartosc_anulowanych_zamowien FROM zamowienie AS z
INNER JOIN status_zamowienia AS s ON s.id_statusu_zamowienia = z.status_zamowienia
INNER JOIN pozycja_zamowienia As p ON p.zamowienie = z.id_zamowienia
WHERE nazwa_statusu_zamowienia = 'anulowane';
```
### 15.
```
SELECT a.miejscowosc, COUNT(*) AS ilosc_zamowien, ROUND(SUM(p.ilosc * p.cena), 2) AS wartosc_zamowien FROM zamowienie AS z
INNER JOIN klient AS k ON k.id_klienta = z.klient
INNER JOIN adres_klienta AS a ON a.klient = k.id_klienta
INNER JOIN typ_adresu AS t ON t.id_typu = a.typ_adresu
INNER JOIN pozycja_zamowienia As p ON p.zamowienie = z.id_zamowienia
WHERE t.nazwa = 'podstawowy'
GROUP BY a.miejscowosc
ORDER BY a.miejscowosc ASC;
```
### 16.
```
SELECT ROUND(SUM(p.ilosc * p.ilosc), 2) AS dochod FROM zamowienie AS z
INNER JOIN status_zamowienia AS s ON s.id_statusu_zamowienia = z.status_zamowienia
INNER JOIN pozycja_zamowienia As p ON p.zamowienie = z.id_zamowienia
WHERE nazwa_statusu_zamowienia = 'zrealizowane';
```
### 17.
```
SELECT YEAR(z.data_zamowienia) AS rok, ROUND(((SUM(p.ilosc * p.cena)) - (SUM(t.cena_zakupu * p.ilosc))) , 2) AS dochod FROM zamowienie AS z
INNER JOIN status_zamowienia AS sz ON sz.id_statusu_zamowienia = z.status_zamowienia
INNER JOIN pozycja_zamowienia As p ON p.zamowienie = z.id_zamowienia
INNER JOIN towar AS t ON t.id_towaru = p.towar
WHERE nazwa_statusu_zamowienia = 'zrealizowane'
GROUP BY rok;
```
### 18.
```
SELECT k.nazwa_kategori, (SUM(sm.ilosc) - SUM(p.ilosc)) AS ilosc FROM stan_magazynowy AS sm
INNER JOIN towar AS t ON t.id_towaru = sm.towar
INNER JOIN pozycja_zamowienia AS p ON p.towar = t.id_towaru
INNER JOIN zamowienie AS z ON z.id_zamowienia = p.zamowienie
INNER JOIN status_zamowienia AS sz ON sz.id_statusu_zamowienia = z.status_zamowienia
INNER JOIN kategoria AS k ON k.id_kategori = t.kategoria
WHERE nazwa_statusu_zamowienia = 'zrealizowane'
GROUP BY k.nazwa_kategori
ORDER BY k.nazwa_kategori ASC;
```
### 19.
```
SELECT sub.miesiac, sub.liczba_pracownikow FROM
(SELECT MONTHNAME(data_urodzenia) AS miesiac, MONTH(data_urodzenia) AS miesiac_l, COUNT(*) AS liczba_pracownikow FROM pracownik
GROUP BY miesiac, miesiac_l
ORDER BY miesiac_l) AS sub;
```
