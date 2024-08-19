# SQL-project
 pet_Shelter data for training



create schema pet_shelter;

use pet_shelter; 
CREATE TABLE pet (
    id INT UNSIGNED PRIMARY KEY AUTO_INCREMENT NOT NULL,
    kind VARCHAR(50) NOT NULL,
    color VARCHAR(50) NOT NULL,
    birth_date DATE, 
    arrival_date DATE NOT NULL
);



CREATE TABLE owner (
    id INT UNSIGNED PRIMARY KEY AUTO_INCREMENT NOT NULL,
    fname VARCHAR(50),
    lname VARCHAR(50),
    gender CHAR(1) CHECK (gender IN ('M', 'F'))
);

DELIMITER $$
CREATE TRIGGER check_gender_insert
BEFORE INSERT ON owner
FOR EACH ROW
BEGIN
    IF NEW.gender NOT IN ('M', 'F') THEN
        SIGNAL SQLSTATE value '40000'
        SET MESSAGE_TEXT = 'You have to enter a valid gender letter (M OR F)';
    END IF;
END $$
DELIMITER ;

DELIMITER $$
CREATE TRIGGER check_gender_update
BEFORE UPDATE ON owner
FOR EACH ROW
BEGIN
    IF NEW.gender NOT IN ('M', 'F') THEN
        SIGNAL SQLSTATE value '40000'
        SET MESSAGE_TEXT = 'You have to enter a valid gender letter (M OR F)';
    END IF;
END $$
DELIMITER ;



CREATE TABLE adoption (
    owner_id INT UNSIGNED NOT NULL,
    pet_id INT UNSIGNED UNIQUE NOT NULL,
    adoption_date DATE,
    
	CONSTRAINT pk_owner_pet_id
    PRIMARY KEY (owner_id, pet_id),
    
    CONSTRAINT fk_owner_id
        FOREIGN KEY (owner_id) REFERENCES owner(id)
        ON UPDATE CASCADE
        ON DELETE RESTRICT,
        
    CONSTRAINT fk_pet_id 
        FOREIGN KEY (pet_id) REFERENCES pet(id)
        ON UPDATE CASCADE
        ON DELETE RESTRICT
);

INSERT INTO pet VALUES
(1, 'dog', 'brown', '2018-01-01', '2018-06-01'),
(2, 'cat', 'black', '2019-07-05', '2019-07-05'),
(3, 'dog', 'gray', '2018-05-01', '2019-05-01'),
(4, 'bird', 'green', '2015-03-24', '2019-06-12'),
(5, 'dog', 'black', '2017-05-05', '2017-05-05'),
(6, 'dog', 'yellow', '2002-02-25', '2002-02-25'),
(7, 'dog', 'white', '2010-04-14', '2015-06-03'),
(8, 'cat', 'white', '2017-04-03', '2017-07-05'),
(9, 'cat', 'black', '2016-02-01', '2017-07-04'),
(10, 'cat', 'gray', '2014-07-17', '2014-07-17'),
(11, 'cat', 'brown', '2015-12-10', '2016-02-14'),
(12, 'cat', 'black', '2012-08-03', '2013-09-10')
;

INSERT INTO owner VALUES
(1, 'adam', 'anderson', 'M'),
(2, 'david', 'viko', 'M'),
(3, 'viky', 'almond', 'F'),
(4, 'rachel', 'soky', 'F'),
(5, 'niel', 'lahos', 'M'),
(6, 'fernanda', 'fox', 'F'),
(7, 'venesa', 'lanwolf', 'F'),
(8, 'damin', 'manson', 'M'),
(9, 'kevin', 'kline', 'M')
;

INSERT INTO adoption VALUES
(1,5, '2019-01-01'),
(3,4, '2019-07-01'),
(4,2, '2019-09-01'),
(4,8, '2017-10-12'),
(4,9, '2018-11-15'),
(5,10,'2016-04-04'),
(5,1, '2018-06-02'),
(7,12,'2017-07-06')
;
