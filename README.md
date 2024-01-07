# Proiect-final

1. Notiuni teoretice asimilate ca urmare a participarii la Cursul de Testare Manuala

2. Aplicatie practica: crearea unei baze de date si folosirea diverselor tipuri de interogari

CREATE database project;
CREATE table Clienti (
id INT PRIMARY KEY,
prenume VARCHAR(50),
nume VARCHAR(50),
email VARCHAR(200),
numar_telefon VARCHAR(20)
);

INSERT INTO Clienti VALUES 
	(1, 'Ana', 'Pop', 'anapop@email.com', '0723464512'),
    (2, 'Ema', 'Tinc', 'ematinc@email.com', '0745771233'),
    (3, 'Ioana', 'Mare', 'ioanam@email.com', '0763112006'),
    (4, 'Matei', 'Balan', 'mateibalan@email.com', '0722457665'),
    (5, 'Dan', 'Ivan', 'daniv@email.com', '0736551009');
    
CREATE table Comenzi (
comanda_id INT PRIMARY KEY,
client_id INT,
data_comanda DATE,
FOREIGN KEY (client_id) REFERENCES Clienti(id)
);

INSERT INTO Comenzi VALUES
	(1, 1, '2023-01-15'),
    (2, 3, '2023-02-20'),
    (3, 2, '2023-03-10'),
    (4, 4, '2023-03-10'),
    (5, 5, '2023-05-22');
    
    
CREATE table Produse (
produs_id INT PRIMARY KEY,
nume_produs VARCHAR(100),
pret DECIMAL(10, 2),
cantitate_in_stoc INT
);

INSERT INTO Produse VALUES
(1, 'Laptop', 500.50, 10),
(2, 'Telefon', 200.00, 20),
(3, 'Casti_audio', 120.10, 50),
(4, 'Tableta', 340.00, 15),
(5, 'Memorie_externa', 135.25, 30);

ALTER table clienti
ADD Localitate VARCHAR(50);

UPDATE Clienti
SET Localitate = 'Baia Mare'
Where id = 1;

UPDATE Clienti
SET Localitate = 'Baia Mare'
WHERE id = 2;

UPDATE Clienti
SET Localitate = 'Baia Sprie'
WHERE id = 3;

UPDATE Clienti
SET Localitate = 'Seini'
WHERE id = 4;

UPDATE Clienti
SET Localitate = 'Baia Mare'
Where id = 5;

-- SELECT

SELECT *
FROM Clienti;

SELECT prenume, nume
FROM Clienti;

-- WHERE

SELECT prenume
FROM Clienti
WHERE id = 3;

SELECT prenume, nume, localitate
FROM clienti
WHERE localitate <> 'Baia Mare';

-- ORDER BY

SELECT prenume
FROM clienti
ORDER BY prenume  DESC
LIMIT 2;

-- FILTRARE  CU CONSTRANGERI -- BETWEEN, IN, LIKE, NOT LIKE, IN (string exist in a list)
							-- AND, OR 
SELECT id, nume, prenume
FROM clienti
WHERE prenume LIKE 'E%';

SELECT id, nume, prenume
FROM clienti
WHERE prenume NOT LIKE 'E%';


SELECT id, nume, prenume, localitate
FROM clienti 
WHERE localitate IN ('Baia Sprie', 'Seini');

SELECT id, nume, prenume, localitate
FROM clienti
WHERE localitate = 'Baia Mare' AND prenume = 'Ana';

SELECT id, nume, prenume
FROM clienti
WHERE nume LIKE '%n%';

SELECT id, nume, prenume, numar_telefon
FROM clienti
WHERE numar_telefon LIKE '072%'; 

SELECT id, nume, prenume, numar_telefon
FROM clienti
WHERE prenume = 'Ana' AND numar_telefon LIKE '%45%';

SELECT id, nume, prenume, numar_telefon
FROM clienti
WHERE prenume = 'Ana' OR numar_telefon LIKE '%11%';

SELECT id, nume, prenume, numar_telefon, localitate
FROM clienti
WHERE (prenume = 'Ana' OR numar_telefon LIKE '%11%') AND localitate = 'Baia Mare';

SELECT * FROM produse 
WHERE pret BETWEEN 100 AND 200;

-- FUNCTII AGREGATE 

SELECT SUM(cantitate_in_stoc) AS total 
FROM produse;

SELECT AVG(cantitate_in_stoc), nume_produs
FROM produse
GROUP BY nume_produs;

SELECT MIN(cantitate_in_stoc)
FROM produse;

SELECT COUNT(id), nume, localitate
FROM clienti
GROUP BY localitate;


SELECT nume_produs, SUM(cantitate_in_stoc) AS stoc
FROM produse
GROUP BY nume_produs
HAVING SUM(cantitate_in_stoc) >= 15;

SELECT COUNT(id), localitate
FROM clienti

-- JOINS

-- Lista de comenzi cu informatii clienti

SELECT comenzi.comanda_id, clienti.prenume, clienti.nume, comenzi.data_comanda
FROM Comenzi
INNER JOIN Clienti ON Comenzi.client_id = Clienti.id;



-- LEFT JOIN  -- lista a clientilor si a comenzilor lor

SELECT clienti.id, clienti.prenume, clienti.nume, comenzi.comanda_id, comenzi.data_comanda
FROM clienti
LEFT JOIN Comenzi ON clienti.id = comenzi.comanda_id;


-- RIGHT JOIN -- lista a comenzilor si a clientilor corespunzatori

SELECT comenzi.comanda_id, clienti.prenume, clienti.nume, comenzi.data_comanda
FROM comenzi
RIGHT JOIN clienti ON comenzi.client_id = clienti.id;

-- CROSS JOIN toti clientii si toate comenzile

SELECT * FROM clienti CROSS JOIN comenzi; 


