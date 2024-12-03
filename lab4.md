# bazy danych lab4
## Zadanie 1
### 1.
```
DELETE FROM postac WHERE rodzaj = 'wiking' and nazwa != 'Bjorn'
ORDER BY wiek DESC LIMIT 2;
```
### 2.
```
ALTER TABLE przetwory DROP FOREIGN KEY przetwory_ibfk_1;

ALTER TABLE walizka DROP FOREIGN KEY walizka_ibfk_1;

ALTER TABLE postac MODIFY COLUMN id_postaci INT NOT NULL;

ALTER TABLE postac DROP PRIMARY KEY;
```
## Zadanie 2
### 1.
```
ALTER TABLE postac ADD COLUMN pesel CHAR(11) NOT NULL FIRST;

UPDATE postac SET pesel = '64734515748' + id_postaci;
```
### 2.
```
ALTER TABLE postac MODIFY COLUMN rodzaj ENUM('wiking', 'ptak', 'kobieta', 'syrena');
```
### 3.
```
INSERT INTO postac (pesel, id_postaci, nazwa, rodzaj, data_ur, wiek, funkcja, statek)
VALUES (64734515757, 9, 'Gertruda Nieszczera', 'syrena', '1922-04-05', 88, NULL, NULL);
```
## Zadanie 3
### 1.
```
UPDATE postac SET statek = 'Latajacy Holender' WHERE nazwa LIKE '%a%';
```
### 2.
```
UPDATE statek SET max_ladownosc = max_ladownosc*0.7 WHERE data_wodowania > '1901-01-01';
```
### 3.
```
SELECT * FROM postac WHERE wiek > 1000;
```
## Zadanie 4
### 1.
```
ALTER TABLE postac MODIFY COLUMN postac ENUM('wiking', 'ptak', 'kobieta', 'syrena', 'waz');

INSERT INTO postac (pesel, id_postaci, nazwa, rodzaj, data_ur, wiek, funkcja, statek)
VALUES ('46745854895', 10, 'Waz Loko', 'waz', '1853-07-14', 157, DEFAULT, DEFAULT);
```
### 2.
```
CREATE TABLE marynarz (
  pesel char(11) PRIMARY KEY NOT NULL,
  id_postaci int NOT NULL,
  nazwa varchar(40) DEFAULT NULL,
  rodzaj enum('wiking','ptak','kobieta','syrena') DEFAULT NULL,
  data_ur date DEFAULT NULL,
  wiek tinyint unsigned DEFAULT NULL,
  funkcja varchar(30) DEFAULT NULL,
  statek varchar(50) DEFAULT NULL,
  FOREIGN KEY (statek) REFERENCES statek (nazwa_statku)
);

INSERT INTO marynarz (pesel, id_postaci, nazwa, rodzaj, data_ur, wiek, funkcja, statek)
SELECT pesel, id_postaci, nazwa, rodzaj, data_ur, wiek, funkcja, statek FROM postac WHERE statek IS NOT NULL;
```
## Zadanie 5
### 1.
```
UPDATE marynarz SET statek = NULL;

UPDATE postac SET statek = NULL;
```
### 2.
```
DELETE FROM marynarz WHERE id_postaci = 6;

DELETE FROM postac WHERE id_postaci = 6;
```
### 3.
```
ALTER TABLE postac DROP FOREIGN KEY postac_ibfk_1;

DELETE FROM statek WHERE nazwa_statku IS NOT NULL;
```
### 4.
```
ALTER TABLE marynarz DROP FOREIGN KEY marynarz_ibfk_1;

DROP TABLE statek;
```
### 5.
```
CREATE TABLE zwierz (
id TINYINT PRIMARY KEY NOT NULL AUTO_INCREMENT,
nazwa VARCHAR(40),
wiek SMALLINT UNSIGNED
);
```
### 6.
```
INSERT INTO zwierz (id, nazwa, wiek)
VALUES (DEFAULT, 'Drozd', 22),
(DEFAULT, 'Waz Loko', 157);
```
