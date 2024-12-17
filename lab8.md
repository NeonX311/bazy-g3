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

CREATE TRIGGER uczestnicy_after_insert
AFTER INSERT ON uczestnicy
FOR EACH ROW
BEGIN
    DECLARE postac varchar(100);
    DECLARE has_postac bool;
    SET postac = 'Tesciowa';

    IF postac in (
	SELECT nazwa from kreatura WHERE idKreatury in 
	    (SELECT id_uczestnika from uczestnicy where id_wyprawy=NEW.id_wyprawy AND sektor.nazwa = 'Chatka dziadka')) 
    THEN 
        SET has_postac = true;
	END IF;
    
    IF has_postac
    THEN 
	INSERT INTO system_alarmowy
        VALUES ('Teściowa nadchodzi !!!')
END IF;

END//
DELIMITER ;
```
## zadanie 5
### 1.
do zrobienia
### 2.
do zrobienia
