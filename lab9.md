# bazy danych lab9
## zadanie 1
### 1.
```
SELECT nazwisko FROM pracownik
ORDER BY nazwisko ASC;
```
### 2.
```
SELECT imie, nazwisko, pensja FROM pracownik
WHERE YEAR(data_urodzenia) > 1979;
```
### 3.
```
SELECT * FROM pracownik
WHERE pensja BETWEEN 3500 AND 5000;
```
### 4.
```
SELECT * FROM stan_magazynowy
WHERE ilosc > 10;
```
### 5.
```
SELECT * FROM towar
WHERE nazwa_towaru LIKE 'A%' OR nazwa_towaru LIKE 'B%' OR nazwa_towaru LIKE 'C%'
```
### 6.
```
SELECT * FROM klient
WHERE czy_firma = 0;
```
### 7.
```
SELECT * FROM zamowienie
ORDER BY data_zamowienia DESC 
LIMIT 10;
```
### 8.
```
SELECT * FROM pracownik
ORDER BY pensja ASC
LIMIT 5;
```
### 9.
```
SELECT * FROM towar
WHERE nazwa_towaru NOT LIKE '%a%'
ORDER BY cena_zakupu DESC
LIMIT 10;
```
### 10.
```
SELECT t.* FROM towar AS t
INNER JOIN stan_magazynowy AS s ON s.towar = t.id_towaru
INNER JOIN jednostka_miary AS j ON j.id_jednostki = s.jm
WHERE j.nazwa = 'szt'
ORDER BY t.nazwa_towaru, t.cena_zakupu DESC;
```
### 11.
```
CREATE TABLE towary_powyzej_100 SELECT * FROM __firma_zti.towar WHERE cena_zakupu >=100;
```
### 12.
```
CREATE TABLE pracownik_50_plus LIKE __firma_zti.pracownik;

INSERT INTO pracownik_50_plus
SELECT * FROM __firma_zti.pracownik
WHERE (YEAR(CURRENT_DATE()) - YEAR(data_urodzenia)) > 49;
```
## zadanie 2
### 1.
```
SELECT p.imie, p.nazwisko, d.nazwa FROM pracownik AS p
INNER JOIN dzial AS d ON id_dzialu = p.dzial;
```
### 2.
```
SELECT t.nazwa_towaru, k.nazwa_kategori, s.ilosc FROM towar AS t
INNER JOIN kategoria AS k ON k.id_kategori = t.kategoria
INNER JOIN stan_magazynowy AS s ON s.towar = t.id_towaru
ORDER BY s.ilosc DESC;
```
### 3.
```
SELECT z.* FROM zamowienie AS z
INNER JOIN status_zamowienia AS s ON s.id_statusu_zamowienia = z.status_zamowienia
WHERE s.nazwa_statusu_zamowienia = 'anulowane';
```
### 4.
```
SELECT k.* FROM klient AS k
INNER JOIN adres_klienta AS a ON a.klient = k.id_klienta
INNER JOIN typ_adresu AS t ON t.id_typu = a.typ_adresu
WHERE a.miejscowosc = 'Olsztyn' AND t.nazwa = 'podstawowy';
```
### 5.
```
SELECT j.nazwa FROM jednostka_miary AS j
LEFT JOIN stan_magazynowy AS s ON j.id_jednostki = s.jm
WHERE s.jm IS NULL;
```
### 6.
```
SELECT z.numer_zamowienia, t.nazwa_towaru, p.ilosc, p.cena FROM zamowienie AS z
INNER JOIN pozycja_zamowienia AS p ON p.zamowienie = z.id_zamowienia
INNER JOIN towar AS t ON t.id_towaru = p.towar
WHERE YEAR(data_zamowienia) = 2018;
```
### 7.
```
CREATE TABLE towary_full_info SELECT t.nazwa_towaru, t.cena_zakupu, k.nazwa_kategori, s.ilosc, j.nazwa
                              FROM __firma_zti.towar AS t 
                              LEFT JOIN __firma_zti.kategoria AS k ON k.id_kategori = t.kategoria
                              LEFT JOIN __firma_zti.stan_magazynowy AS s ON s.towar = t.id_towaru
                              LEFT JOIN __firma_zti.jednostka_miary AS j ON j.id_jednostki = s.jm;
```
### 8.
```
SELECT p.* FROM pozycja_zamowienia AS p
INNER JOIN zamowienie AS z ON p.zamowienie = z.id_zamowienia
ORDER BY data_zamowienia ASC LIMIT 5;
```
### 9.
```
SELECT  z.* FROM zamowienie AS z
INNER JOIN status_zamowienia AS s ON s.id_statusu_zamowienia = z.status_zamowienia
WHERE s.nazwa_statusu_zamowienia != 'zrealizowane'
```
### 10.
```
SELECT * FROM adres_klienta
WHERE kod NOT REGEXP '[0-9]{2}-[0-9]{3}';
```
