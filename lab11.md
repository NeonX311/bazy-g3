# bazy danych lab11
## zadanie 1
### 1.
```
CREATE TABLE student
(
    id_studenta INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    imie VARCHAR(100) NOT NULL,
    nazwisko VARCHAR(100) NOT NULL,
    rok_studiow INT UNSIGNED,
    data_urodzenia DATE
);
```
### 2.
```
CREATE TABLE kierunek
(
    id_kierunku INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    nazwa_kierunku VARCHAR(200) NOT NULL
);
```
### 3.
```
CREATE TABLE student_na_kierunku
(
    student INT UNSIGNED,
    kierunek INT UNSIGNED,
    FOREIGN KEY (student) REFERENCES student(id_studenta),
    FOREIGN KEY (kierunek) REFERENCES kierunek(id_kierunku),
    PRIMARY KEY (student,kierunek)
);
```
### 4.
```
CREATE TABLE przedmiot
(
    id_przedmiotu INT PRIMARY KEY AUTO_INCREMENT,
    nazwa_przedmiotu VARCHAR(200) NOT NULL,
    opis LONGTEXT
);
```
### 5.
```
CREATE TABLE kierunek_has_przedmiot
(
    kierunek INT UNSIGNED,
    przedmiot INT,
    obowiazkowy BOOL DEFAULT(1),
    FOREIGN KEY (kierunek) REFERENCES kierunek(id_kierunku),
    FOREIGN KEY (przedmiot) REFERENCES przedmiot(id_przedmiotu),
    PRIMARY KEY (kierunek, przedmiot)
);
```
### 6.
```
INSERT INTO student (imie, nazwisko, rok_studiow, data_urodzenia)
VALUES ('Patryk', 'Sek', 1, '2005-01-01'),
('Rafal', 'Kowalski', 1, '2004-07-15'),
('Wiktoria', 'Skorniewska', 1, '2003-12-24'),
('Kacper', 'Lipka', 1, '2004-04-20');

INSERT INTO kierunek (nazwa_kierunku)
VALUES ('informatyka'),
('sztuka');

INSERT INTO student_na_kierunku (student, kierunek)
VALUES (1, 1),
(2, 1),
(3, 2),
(4, 1);

INSERT INTO przedmiot (nazwa_przedmiotu, opis)
VALUES ('podstawy logiki i teorii mnogosci', NULL),
('bazy danych', NULL),
('etyka', NULL),
('rzezba', NULL);

INSERT INTO  kierunek_has_przedmiot (kierunek, przedmiot, obowiazkowy)
VALUES (1, 1, 1),
(1, 2, 1),
(1, 3, 0),
(2, 4, 1),
(2, 3, 0);
```
### 7.
```
ALTER TABLE przedmiot CHANGE COLUMN opis opis VARCHAR(500) DEFAULT 'Brak opisu';
```
### 8.
```
ALTER TABLE student CHANGE COLUMN id_studenta id_studenta INT UNSIGNED;

ALTER TABLE student DROP PRIMARY KEY;

ALTER TABLE student ADD COLUMN indeks INT UNSIGNED NOT NULL AFTER data_urodzenia;

UPDATE student SET indeks = 100000 + id_studenta;

ALTER TABLE student ADD PRIMARY KEY (indeks);
```
### 9.
```
ALTER TABLE kierunek_has_przedmiot ADD COLUMN semestr VARCHAR(5) NOT NULL;

ALTER TABLE kierunek_has_przedmiot ADD COLUMN rok_studiow INT UNSIGNED NOT NULL;*

UPDATE kierunek_has_przedmiot SET semestr = '2024Z';

UPDATE kierunek_has_przedmiot SET rok_studiow = 1;
```
### 10.
```
SELECT s.indeks, s.imie, s.nazwisko, k.nazwa_kierunku, khp.semestr, p.nazwa_przedmiotu FROM student AS s
INNER JOIN student_na_kierunku AS snk ON snk.student = s.id_studenta
INNER JOIN kierunek AS k ON k.id_kierunku = snk.kierunek
INNER JOIN kierunek_has_przedmiot as khp ON khp.kierunek = k.id_kierunku
INNER JOIN przedmiot AS p ON p.id_przedmiotu = khp.przedmiot
ORDER BY s.indeks;
```
