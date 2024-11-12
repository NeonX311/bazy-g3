# bazy danych lab3
## Zadanie 1
### 1.
```CREATE TABLE postac (
  id_postaci SMALLINT PRIMARY KEY AUTO_INCREMENT,
  nazwa VARCHAR(40),
  rodzaj ENUM('wiking', 'ptak', 'kobieta'),
  data_ur DATE,
  wiek SMALLINT UNSIGNED
); ```

### 2.
```INSERT INTO postac ('id_postaci', 'nazwa', 'rodzaj', data_ur', 'wiek')
VALUES (DEFAULT, 'Bjorn', 'wiking', '1967-09-15', 60),
(DEFAULT, 'Drozd', 'ptak', '2013-07-23', 13),
(DEFAULT, 'Tesciowa', 'kobieta', '1944-02-28', 80);```

### 3.
```UPDATE postac SET wiek = 88 WHERE id_postaci = 3;```

## Zadanie 2
### 1.
```CREATE TABLE walizka (
  id_walizki SMALLINT PRIMARY KEY AUTO_INCREMENT,
  pojemnosc TINYINT UNSIGNED,
  kolor ENUM('rozowy', 'czerwony', 'teczowy', 'zolty'),
  id_wlasciciela SMALLINT NOT NULL,
  FOREIGN KEY (id_wlasciciela) REFERENCES postac(id_postaci) ON DELETE CASCADE
);```

### 2.
```ALTER TABLE walizka 
CHANGE COLUMN kolor kolor ENUM('rozowy', 'czerwony', 'teczowy', 'zolty') DEFAULT 'rozowy' ;```

### 3.
```INSERT INTO walizka(id_walizki, pojemnosc, kolor, id_wlasciciela)
VALUES (default, 30, 'czerwony', 1),
(default, 25, 'zolty', 2);```

## Zadanie 3
### 1.
```CREATE TABLE izba (
  adres_budynku VARCHAR(45) NOT NULL,
	nazwa_izby VARCHAR(45) NOT NULL,
	metraz SMALLINT UNSIGNED,
	wlasciciel INT,
	PRIMARY KEY(adres_budynku, nazwa_izby),
	FOREIGN KEY (wlasciciel) REFERENCES postac(id_postaci) ON DELETE SET NULL
);```

### 2.
```ALTER TABLE izba
ADD COLUMN kolor VARCHAR(30) AFTER metraz;```

### 3.
```INSERT INTO izba(adres_budynku, nazwa_izby, metraz, kolor, wlasciciel)
VALUES ('Olsztyn ul.Sloneczna 54', 'spizarnia', 35, 'brazowy', 1);```

### 4.
```LTER TABLE izba 
CHANGE COLUMN kolor kolor VARCHAR(30) NULL DEFAULT 'czarny' ;```

## Zadanie 4
### 1.
```CREATE TABLE przetwory(
	id_przetworu TINYINT NOT NULL PRIMARY KEY,
    rok_produkcji SMALLINT DEFAULT(1654),
    id_wykonawcy INT NOT NULL,
    zawartosc VARCHAR(45),
    dodatek VARCHAR(45) DEFAULT('papryczka chili'),
    id_konsumenta INT NOT NULL
);

ALTER TABLE przetwory ADD FOREIGN KEY (id_wykonawcy) REFERENCES postac(id_postaci);
ALTER TABLE przetwory ADD FOREIGN KEY (id_konsumenta) REFERENCES postac(id_postaci);```

### 2.
```INSERT INTO przetwory(id_przetworu, rok_produkcji, id_wykonawcy, zawartosc, dodatek, id_konsumenta)
VALUES (1, DEFAULT, 3, 'bigos', DEFAULT, 1);```

## Zadanie 5
### 1.
```INSERT INTO postac(id_postaci, nazwa, rodzaj, data_ur, wiek)
VALUES (DEFAULT, 'Wiking_1', 'wiking', '1973-04-25', 37),
(DEFAULT, 'Wiking_2', 'wiking', '1973-04-25', 37),
(DEFAULT, 'Wiking_3', 'wiking', '1973-04-25', 37),
(DEFAULT, 'Wiking_4', 'wiking', '1973-04-25', 37),
(DEFAULT, 'Wiking_5', 'wiking', '1973-04-25', 37);```

### 2.
```CREATE TABLE statek(
	nazwa_statku VARCHAR(50) NOT NULL PRIMARY KEY,
    rodzaj_statku ENUM('slup','brygantyna','galeon'),
    data_wodowania date,
    max_ladownosc SMALLINT UNSIGNED
);```

### 3.
```INSERT INTO statek(nazwa_statku, rodzaj_statku, data_wodowania, max_ladownosc)
VALUES ('Czarna Perla', 'brygantyna','1713-07-13', 30),
('Latajacy Holender', 'galeon', '1693-03-30', 45);```

### 4.
```ALTER TABLE postac ADD COLUMN funkcja VARCHAR(30);```

### 5.
```UPDATE postac SET funkcja = 'kapitan' WHERE (id_postaci = '1');```

### 6.
```ALTER TABLE postac ADD COLUMN statek VARCHAR(50) NOT NULL;

ALTER TABLE postac ADD FOREIGN KEY (statek) REFERENCES statek(nazwa_statku);```

### 7.
```UPDATE postac SET statek = 'Latajacy Holender' WHERE rodzaj != 'kobieta';```

### 8.
```DELETE FROM izba WHERE nazwa_izby = 'spizarnia';```

### 9.
```DROP TABLE izba;```