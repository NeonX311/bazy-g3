# bazy danych lab8
## zadanie 1
```
DELIMITER //

CREATE TRIGGER kreatura_before_update

BEFORE UPDATE ON kreatura

FOR EACH ROW

BEGIN
	IF NEW.waga < 0
		THEN
		SET NEW.waga = 0;
	END IF;
END
//

DELIMITER ;

DELIMITER //

CREATE TRIGGER kreatura_before_insert

BEFORE INSERT ON kreatura

FOR EACH ROW

BEGIN
	IF NEW.waga < 0
		THEN
		SET NEW.waga = 0;
	END IF;
END
//

DELIMITER ;
```
## zadanie 2
```
CREATE TABLE archiwum_wypraw
LIKE wyprawa;

DELIMITER //

CREATE TRIGGER wyprawa_before_delete

BEFORE DELETE ON wyprawa

FOR EACH ROW

BEGIN
	INSERT INTO archiwum_wypraw SELECT * FROM wyprawa WHERE id_wyprawy=OLD.id_wyprawy;
END;
//
DELIMITER ;
```
## zadanie 3
### 1.
```
DELIMITER //

CREATE PROCEDURE eliksir_sily(IN id INT)

BEGIN
	UPDATE kreatura SET udzwig = 1.2 * udzwig WHERE idKreatury = id;
END
//
DELIMITER ;
```
### 2.
```
DELIMITER //
CREATE FUNCTION to_upper(input_txt VARCHAR(255))
	RETURNS VARCHAR(255)
BEGIN
	RETURN UPPER(input_txt);
END;
//

DELIMITER ;
```
## zadanie 4
### 1.
```
CREATE TABLE system_alarmowy(
	id_alarmu INT AUTO_INCREMENT,
	wiadomosc VARCHAR(50)
);
```
### 2.
```
DELIMITER //

CREATE TRIGGER wyprawa_after_insert
AFTER INSERT ON wyprawa
FOR EACH ROW
BEGIN
    DECLARE is_matching INT;

    SELECT COUNT(*)
    INTO is_matching
    FROM uczestnicy AS u
    INNER JOIN kreatura AS k ON k.idKreatury = u.id_uczestnika
    INNER JOIN etapy_wyprawy AS e ON e.idWyprawy = NEW.id_wyprawy
    INNER JOIN sektor AS s ON s.id_sektora = e.sektor
    WHERE k.nazwa = 'tesciowa'
      AND s.nazwa = 'Chatka dziadka';

    IF is_matching > 0 THEN
        INSERT INTO system_alarmowy (wiadomosc) 
        VALUES ('Teściowa nadchodzi !!!');
    END IF;
END;
//
DELIMITER ;
```
## zadanie 5
### 1.
do zrobienia
### 2.
do zrobienia
