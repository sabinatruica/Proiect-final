# Proiect final curs ITFactory

Scopul acestui proiect este de a folosi in practica cunostintele in domeniul bazelor de date si a limbajului sql dobandite ca urmare a participarii la cursul de Software Testing

Cu ajutorul  programului MySQL Workbench, am creat o baza de date numita 'Magazin online' care contine trei tabele: ‘clienti’, ‘comenzi’, ‘produse’

## 1.	Database Schema

Mai jos am atasat schema generata de MySQL prin optiunea Reverse Engineer in care se pot vedea cele trei tabele si relatiile intre ele

![git hub](https://github.com/sabinatruica/Proiect-final/assets/130207490/16d703ab-0662-418e-a67b-6e2c68eacc9b)


Tabelul ‘comenzi’ a fost legat de tabelul ‘clienti’ printr-o relatie de ‘one to many’ care a fost implementata prin coloanele: clienti.id reprezentand cheia primara si comenzi.comanda_id reprezentand cheia secundara

## 2.	Database Queries

### DDL (Data Definition Language)

Pentru a creea baza de date ne-am folosit de instructiunea create database; pentru a stoca informatii si relatii in baza de date am creeat cele trei tabele

CREATE database project;
CREATE table Clienti (
id INT PRIMARY KEY,
prenume VARCHAR(50),
nume VARCHAR(50),
email VARCHAR(200),
numar_telefon VARCHAR(20)
);

CREATE table Comenzi (
comanda_id INT PRIMARY KEY,
client_id INT,
data_comanda DATE,
FOREIGN KEY (client_id) REFERENCES Clienti(id)
);

CREATE table Produse (
produs_id INT PRIMARY KEY,
nume_produs VARCHAR(100),
pret DECIMAL(10, 2),
cantitate_in_stoc INT
);

Ulterior, am modificat tabelul 'clienti' si am adaugat o noua coloana 'localitate'
ALTER table clienti ADD Localitate VARCHAR(50);

Am exemplificat instructiunea de drop, fara a o rula
DROP table produse;

### DML(Data Manipulation Language)

Pentru a putea folosi aceasta baza de date am populat tabelele cu date pentru a putea executa interogari si manipula baza de date folosindu-ne de instructiunea insert

INSERT INTO Clienti VALUES 
	(1, 'Ana', 'Pop', 'anapop@email.com', '0723464512'),
    (2, 'Ema', 'Tinc', 'ematinc@email.com', '0745771233'),
    (3, 'Ioana', 'Mare', 'ioanam@email.com', '0763112006'),
    (4, 'Matei', 'Balan', 'mateibalan@email.com', '0722457665'),
    (5, 'Dan', 'Ivan', 'daniv@email.com', '0736551009');

INSERT INTO Comenzi VALUES
	(1, 1, '2023-01-15'),
    (2, 3, '2023-02-20'),
    (3, 2, '2023-03-10'),
    (4, 4, '2023-03-10'),
    (5, 5, '2023-05-22');

INSERT INTO Produse VALUES
(1, 'Laptop', 500.50, 10),
(2, 'Telefon', 200.00, 20),
(3, 'Casti_audio', 120.10, 50),
(4, 'Tableta', 340.00, 15),
(5, 'Memorie_externa', 135.25, 30);

Pentru a adauga date in coloana 'localitate' adaugata ulterior , ne-am folosit de instructiunea update

UPDATE Clienti SET Localitate = 'Baia Mare' Where id = 1;
UPDATE Clienti SET Localitate = 'Baia Mare' WHERE id = 2;
UPDATE Clienti SET Localitate = 'Baia Sprie' WHERE id = 3;
UPDATE Clienti SET Localitate = 'Seini' WHERE id = 4;
UPDATE Clienti SET Localitate = 'Baia Mare' Where id = 5;

### DQL(Data Query Language)

Pentru a interoga baza de date am creat urmatoarele instructiuni care sa reflecte posibile probleme ce pot aparea in situatii reale potentiale utilizand instructiuni select (queries)
Extragerea informatiilor din bazele de date se face utilizand instructiuni select (queries)

Pentru a extrage toate informatiile din tabelul 'clienti' am folosit simbolul '*' care ne ajuta sa evitam enumerarea tuturor coloanelor
SELECT * FROM Clienti;

Sau putem selecta din tabel doar doua coloane (impreuna cu toate randurile)
SELECT prenume, nume FROM Clienti;

Pentru a filtra rezultatele pe care vrem sa le obtinem ne folosim de clauza 'where' care verifica valorile din coloane pentru a determina daca vor fi incluse in rezultat sau nu
Putem, de exemplu, extrage doar prenumele clientului cu id 3
SELECT prenume FROM Clienti WHERE id = 3;

Selectam informatii despre clienti restrictionand cautarea la intrari cu valoarea 'Baia Mare' din coloana 'localitate'
SELECT prenume, nume, localitate FROM clienti WHERE localitate <> 'Baia Mare';

Putem construi query-uri mai complexe folosind cuvintele cheie 'AND' si 'OR', precum si operatori ca de exemplu '='
Am extras informatii din tabel folosind doua conditii: anumite valori pentru coloanele 'localitate' si 'prenume' 
SELECT id, nume, prenume, localitate FROM clienti WHERE localitate = 'Baia Mare' AND prenume = 'Ana';

In urmatorul exemplu am folosit operatorul 'LIKE' pentru a cauta un anumit pattern in coloana 'numar_telefon'
SELECT id, nume, prenume, numar_telefon FROM clienti WHERE prenume = 'Ana' OR numar_telefon LIKE '%11%';

Pentru query-uri in care avem mai mult de doua criterii de selectie folosim paranteza pentru a indica ordinea de executie a operatiilor
SELECT id, nume, prenume, numar_telefon, localitate FROM clienti WHERE (prenume = 'Ana' OR numar_telefon LIKE '%11%') AND localitate = 'Baia Mare';

Am folosit operatorul 'BETWEEN' pentru a selecta date dintr-un anumit interval de valori
SELECT * FROM produse WHERE pret BETWEEN 100 AND 200;

In afara instructiunii de 'select', SQL suporta si o serie de functii agregate care returneaza o singura valoare calculata pe baza unui set de valori
Folosind functia 'AVG' am calculat media valorilor de pe o anumita coloana
SELECT AVG(cantitate_in_stoc), nume_produs FROM produse;

Functia 'MIN' ne-a returnat valoarea minima din coloana 'cantitate_in_stoc'
SELECT MIN(cantitate_in_stoc) FROM produse;

In acest query am extras suma valorilor de pe coloana 'cantitate_in_stoc' intr-o coloana pe care am redenumit-o 'stoc' folosind comanda 'AS' care ii ofera un alias, dupa care am grupat rezultatele dupa 'nume_produs'; am mai adaugat o constrangere folosind instructiunea 'HAVING' si operatorul '>='
SELECT nume_produs, SUM(cantitate_in_stoc) AS stoc FROM produse GROUP BY nume_produs HAVING SUM(cantitate_in_stoc) >= 15;

Pentru a obtine informatii din mai multe tabele ne folosim de instructiunea 'JOIN' care combina inregistrarile din cele doua tabele daca exista valori in campuri comune
Am extras lista de comenzi cu informatiile clientilor acolo unde exista valori corespondente
SELECT comenzi.comanda_id, clienti.prenume, clienti.nume, comenzi.data_comanda FROM Comenzi INNER JOIN Clienti ON Comenzi.client_id = Clienti.id;

Daca ne intereseaza informatiile din tabelul 'clienti' la care vrem sa adaugam informatiile despre comenzi acolo unde acestea exista folosim instructiunea 'LEFT JOIN'
SELECT clienti.id, clienti.prenume, clienti.nume, comenzi.comanda_id, comenzi.data_comanda FROM clienti LEFT JOIN Comenzi ON clienti.id = comenzi.comanda_id;

Daca, in schimb, ne intereseaza informatiile din tabelul 'comenzi' impreuna cu clientii corespunzatori folosim instructiunea 'right join'
SELECT comenzi.comanda_id, clienti.prenume, clienti.nume, comenzi.data_comanda FROM comenzi RIGHT JOIN clienti ON comenzi.client_id = clienti.id;

Instructiunea 'crossjoin' ne-a returnat toate posibilele combinatii de clienti si comenzi sub forma unui produs cartezian
SELECT * FROM clienti CROSS JOIN comenzi;

## 3.	Concluzii

Acest proiect m-a ajutat sa pun in practica cunostintele despre baze de date si limbajul SQL dobandite pe parcursul cursului 
                                                                  



