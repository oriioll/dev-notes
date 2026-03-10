# APUNTS POSTGRESQL
#### TODO: DQL (QUERIES) having, group by, orderby, functions(avg, count, max, min, sum)
## ÍNDEX DE CONTINGUTS

* [1.  MetaComandes Bàsiques de Postgres](#metacomandes-bàsiques-postgres)
* [2. Tipus de Dades](#tipus-de-dades)
* [3. SQL DDL (Definició de Dades)](#ddl---data-definition-language)
    * [Creació taules (CREATE TABLE)](#creació-taules-create-table)
    * [Constraints (Restriccions)](#constraints)
    * [Modificació de dades (ALTER TABLE)](#modificacio-de-dades-alter-table)
    * * [ON DELETE CASCADE](#on-delete-cascade)
    * [Schemas](#schemas)
    * [Roles i Usuaris (DCL)](#roles)
    * [Vistes (VIEWS)](#vistes)
    * [Index i sequencies](#index-i-sequencies)
* [4. SQL DML (Manipulació de Dades)](#dml-data-manipulation-language)
    * [INSERT (Afegir registres)](#insert-afegir-registres)
    * [UPDATE (Modificar Registres)](#update-modificar-registres)
    * [DELETE (Esborrar Registres)](#delete-esborrar-registres)
* [5. SQL DQL (Consultes)](#dql-data-query-language)

## METACOMANDES BÀSIQUES POSTGRES

Llistar Bases de Dades al gestor (nivell servidor)
````` sql
\l
`````

Llistar taules de la base de dades a la que estàs connectat (pots afegir una taula per veure els camps)
````` sql
\d
`````

LListar Schemas dins de la base de dades
````` sql
\dn
`````

Veure Schema on estas situat actualment
````` sql
\ds
-- o tambe
SELECT current_schema;
`````
### Crear base de dades amb usuari

````` sql
--Crear base de dades institut
CREATE DATABASE institut;

--Crear usuari institut i permisos
CREATE USER institut WITH SUPERUSER CREATEROLE ENCRYPTED PASSWORD 'institut';
ALTER DATABASE institut OWNER TO institut;
GRANT ALL PRIVILEGES ON DATABASE institut TO institut;

--Sortir del client
\q

--Connectarse a la base de dades institut amb el seu usuari
psql -U institut -W -d institut

--Veure usuari connectat
SELECT current_user;
`````
-----

## TIPUS DE DADES

### Dades de caràcters (Text)

-   **VARCHAR($n$):** Cadena de caràcters de longitud variable amb un màxim de $n$ caràcters.
    
-   **CHAR($n$):** Cadena de caràcters de longitud fixa de $n$ caràcters. S'omple amb espais si el valor és més curt.
    
-   **TEXT:** Cadena de caràcters de longitud variable per a grans quantitats de text.
    

### Dades numèriques

-   **INT / INTEGER:** Nombre enter.
    
-   **SMALLINT:** Nombre enter petit.
    
-   **BIGINT:** Nombre enter gran.
    
-   **NUMERIC($p, s$) / DECIMAL($p, s$):** Nombre amb precisió i escala. $p$ és el nombre total de dígits, $s$ és el nombre de dígits després del punt decimal.
    
-   **FLOAT / REAL:** Nombre en coma flotant (aproximat).
    
-   **DOUBLE PRECISION:** Nombre en coma flotant de doble precisió (aproximat).
    

### Dades de data i hora

-   **DATE:** Emmagatzema només la data (any, mes, dia).
    
-   **TIME:** Emmagatzema només l'hora (hora, minut, segon).
    
-   **TIMESTAMP:** Emmagatzema la data i l'hora.
    
-   **INTERVAL:** Emmagatzema un període de temps.
    

### Dades booleanes

-   **BOOLEAN:** Pot emmagatzemar $\text{TRUE}$, $\text{FALSE}$ o $\text{NULL}$.

-----

## DDL - Data Definition Language
### Creació taules (CREATE TABLE)
````` sql
CREATE TABLE IF NOT EXISTS [nomTaula] (
    [nomCamp] VARCHAR(9) NOT NULL,
    [nomCamp] VARCHAR(50),
);
`````
---

### CONSTRAINTS
#### Restriccions de clau:
  
##### PRIMARY KEY:
````` sql
CONSTRAINT [PK_TAULA] PRIMARY KEY ([DNI])
`````
  

*PK_TAULA és el nom per a identificar la primary key, s’acostuma a posar PK_NOMTAULA

*DNI és el camp de la taula que serà la primary key

  

##### FOREIGN KEY:
````` sql
CONSTRAINT [FK_TAULA_TAULAREF] FOREIGN KEY ([DNI]) REFERENCES [CLIENT(DNI)]
`````
  

*PK_TAULA_TAULAREF és el nom per a identificar la foreign key, s’acostuma a posar PK_NOMTAULA_TAULAQUEREFERENCIA

*DNI és el camp de la taula que serà la foreign key

*CLIENT(DNI) Client es la taula a la que fa referència la clau forànea i dni es el camp de la taula a la que fa referència (dni està relacionat amb dni de la taula client)

  

#### Restriccions Check:

Per comprovar si el valor a introduir compleix unes regles definides

  

##### Comparació i rang (<, >, <=, >=, =, BETWEEN):

Comparar un valor per veure si està en un rang o si és major o menor que (valors numèrics)
`````sql
CONSTRAINT CK_TAULA CHECK (preu>0)
`````
  

##### Conjunt de valors (IN, NOT IN):

El valor ha de ser un determinat en una llista especifica
`````sql
CONSTRAINT CK_TAULA CHECK (color IN (‘Vermell’, ‘Verd’, ‘Blau’’))
`````
  

##### Manipulació de Strings (UPPER, LOWER, o LENGTH):

El valor ha de ser en majúscules, minúscules o d’una llargària determinada
`````sql
CONSTRAINT CK_TAULA CHECK (nom = UPPER(nom))
`````
  
  

##### Operacions matemàtiques (+, -, *, /):

Assegurar lògica matemàtica entre valors de diferents columnes
````` sql
CONSTRAINT CK_TAULA CHECK (marge > (cost * 0.10))
`````
  

##### Operadors Lògics (AND, OR, NOT):

Combinar condicions en una regla
`````sql
CONSTRAINT CK_TAULA CHECK ((metode = 'aeri' AND pes< 5) OR (metode = 'maritim'))
`````
  

##### Format i patró (~):

Validar formats complexes segons regles
`````sql
CONSTRAINT CK_TAULA CHECK (DNI ~ '^[0-9]{8}[A-Z]$')
`````
*el camp DNI ha de ser de 8 caracters entre 0 i 9 i una lletra

  
  

#### Restricció UNIQUE:

Garanteix que el valor no és repeteix en el camp (no pot aparèixer el mateix DNI dos vegades)
`````sql
CONSTRAINT [UQ_DNI] UNIQUE [email]
`````
  
  

#### Restricció NOT NULL:

Garanteix que el valor no pot ser null, i per tant sempre ha de haver un valor. (Sense Constraint)

`````sql
campExemple NUMERIC(100) NOT NULL
`````
  
  

#### Restricció DEFAULT:

Especifica el valor per defecte que té aquell camp (Sense Constraint)
`````sql
estat VARCHAR(20) DEFAULT ‘ACTIU’
`````
---

### Modificacio de dades (ALTER TABLE)
Pots modificar, afegir o esborrar camps, restriccions o tipus de dades.
També pots cambiar noms de taules i moure taules entre schemas
`````sql
-- Afegir columna
ALTER TABLE [nomSchema].[nomTaula] ADD COLUMN [nomColumna] VARCHAR(50);

-- Esborrar columna
ALTER TABLE [nomSchema].[nomTaula] DROP COLUMN [nomColumna];

-- Modificar tipus de dades
ALTER TABLE [nomSchema].[nomTaula] ALTER COLUMN [nomColumna] TYPE NUMERIC(8,2);

-- Afegir restricció (constraint)
ALTER TABLE [nomSchema].[nomTaula] ADD CONSTRAINT CK_Upper_Prov CHECK (provincia = UPPER(provincia));

-- Esborrar restricció
ALTER TABLE [nomSchema].[nomTaula] DROP CONSTRAINT [nomConstraint];

-- Canviar nom d'una taula o columna
ALTER TABLE [nomSchema].[nomTaula] RENAME TO [nomNouTaula];
ALTER TABLE [nomSchema].[nomTaula] RENAME COLUMN [nomColumna] TO [nomNouTaula];

--Canviar nom d'una restricció
ALTER TABLE [nomSchema].[nomTaula] RENAME CONSTRAINT [nomConstraint] TO [nouNomConstraint];

-- Moure taula d'Schema
ALTER TABLE [nomSchema].[nomTaula] SET SCHEMA [schemaDesti];
`````

---
### On delete cascade
Si no es pot actualizar o esborrar (on update, on delete), s'ha d'esborrar la FK i tornar-la a crear amb cascade
````` sql
UPDATE BICICLETA
SET idbici = 2233456
WHERE idbici = 4233456;

--No es pot ja que idbici tambe esta a la taula lloguer com a clau foranea

--Per fer-ho s'ha de posar la dada com a ON UPDATE cascade

--Canvi a la estructura de la taula
ALTER TABLE LLOGUER DROP CONSTRAINT FK_LLOGUER_BICICLETA ;
ALTER TABLE LLOGUER ADD CONSTRAINT FK_LLOGUER_BICICLETA FOREIGN KEY (bici) REFERENCES BICICLETA(idbici) ON UPDATE CASCADE;
`````
---

### SCHEMAS:

Un esquema es un grup de taules que les pot agrupar en tipus. 
Per exemple una base de dades d'un laboratori pot tindre un Schema d'administració i de clinica.

Crear Schema:
`````sql
CREATE SCHEMA [nom-schema];
`````
 
Situar-se a un Schema (al schema que et situis, totes les instruccions que facis (com CREATE TABLE) es faran dins d'aquell Schema, al fer CREATE TABLE es crearà la taula dins de l'Schema):
`````sql
SET search_path TO [nom-schema];
`````

Moure una taula d'un Schema a un altre
`````sql
ALTER TABLE nom-schema.nomTaula SET SCHEMA [schema-desti];
`````
---

### ROLES
Grups que pots assignar als usuaris amb x permisos sobre taules o Schemas

Crear role
`````sql
CREATE ROLE [nom-role] INHERIT;
`````

Assignar permisos a un rol sobre schemas
`````sql
GRANT USAGE, CREATE ON SCHEMA [nom-schema], [nom-schema] TO [nom-role];
`````
(depen si vol us, creacio o creacio i us)

Crear usuaris amb un role
`````sql
CREATE ROLE [nomUsuari] LOGIN PASSWORD 'password' INHERIT;
GRANT [nom-rol] TO [nomUsuari];
`````
---
### Index i sequencies
L'index serveix per crear una llista ordenada de les dades que vulguis i fer consultes més rapides:
`````sql
CREATE INDEX [nomIndex]
ON [nomTaula([nomCamp])];

-- Tambe es pot fer un index UNIQUE
CREATE UNIQUE INDEX Telefon_index
ON PERSONA(TELEFON);
`````

Les sequencies es un generador de numeros que pots customitzar per quin comença, acaba i el seu salt
`````sql
CREATE SEQUENCE [nomSequencia]
INCREMENT 10 --En aquest cas va de 10 en 10
START WITH 100 -- Comença en 100
MAXVALUE 999999; --Valor maxim 999999

-- Per utilizarla (NEXTVAL('[nomSequencia]'):
INSERT INTO ARXIU (ID) VALUES (NEXTVAL('ARXIUID_SEQ'));
`````
- També es pot utilizar el tipus de dades serial per a que vagi d'un a un i es posi automatic
`````sql
CREATE TABLE users(
   id SERIAL PRIMARY KEY,
   nom VARCHAR
),
--A l'insert no cal posar l'id ja que es un serial i es posa automaticament
INSERT INTO users(nom) VALUES('oriol');
INSERT INTO users(nom) VALUES('joan');
`````
** Si falla un update o insert pot ser pq el tipus de dades de la sequencia es diferent del del camp

---

### Vistes
"Taules" creades a partir d'una consulta, s'actualitza automaticament cada cop que es modifica el contingut de la taula o taules les quals es consulten
`````sql
CREATE VIEW [nomVista] AS
(SELECT NOM, COGNOM1, TELEFON FROM PERSONA); --Consulta de la qual es fa la taula

--Per veure contingut d'una vista es pot fer un select
SELECT * FROM [nomVista];

--Per crear vista amb camps de diferents taules
CREATE VIEW DADES_PROF
AS (SELECT PERSONA.NOM,PERSONA.COGNOM1, PROFESSOR.ESPECIALITAT --Agafa dades de taula persona i professor
FROM PERSONA, PROFESSOR --Les taules les quals es consulten
WHERE PERSONA.DNI = PROFESSOR.DNI_PROF); --Condicio consulta
`````

## DML Data Manipulation Language
### INSERT (afegir registres)
Es per insertar dades dins de les taules i els camps (columnes)
`````sql
INSERT INTO [nomTaula] ([nomCamp], [nomCamp], [nomCamp]) VALUES ([dadaAfegir], [dadaAfegir], [dadaAfegir])
-- Pots no afegir els noms dels camps a insertar i s'interpreterà que estàs afegint tots els camps en ordre

-- Insertar múltiples files a la vegada
INSERT INTO [nomTaula] ([nomCamp], [nomCamp) 
VALUES 
    ('[dadaAfegir]', [dadaAfegir]),
    ('[dadaAfegir]', [dadaAfegir]),
    ('[dadaAfegir]', [dadaAfegir]);

--Si es vol que actui el default es posa DEFAULT i si es vol que sigui null (no hi ha default), es posa NULL
`````

### UPDATE (modificar registres)
Es per modificar les dades d'una taula o columna
`````sql
--Per modificar una dada concreta
UPDATE [nomTaula]
SET [nomCamp a modificar] = [nomValor]
WHERE [nomCamp] = [condicio]; --Aqui tambe podem posar diferents condicions si volem modificar diferents valors a l'hora. Per modificar una fila concreta podem fer la condicio amb la PK (WHERE ID = 5)
--Sense el WHERE ES MODIFIQUEN TOTS ELS REGISTRES:
UPDATE [nomTaula]
SET [nomCamp a modificar] = ['caca']; --A tots els registres de la taula el camp posat tindrà el valor 'caca'

-- Modificar diversos camps en el mateix registre
UPDATE [nomTaula]
SET [camp1] = [nouValor1],
    [camp2] = [nouValor2]
WHERE [condicio];
`````
### DELETE (esborrar registres)
Es per esborrar DADES de les taules i columnes

`````sql
--Per esborrar una fila sencera
DELETE FROM [nomTaula] WHERE id = 5; --Pots posar condicions com WHERE salari<1000 per esborrar totes les files a les quals el salari es menor que 1000
`````

### TRUNCATE (esborrar totes les dades de la taula)
Es per esborrar totes les DADES d'una taula mantenint la estructura intacta
`````sql
TRUNCATE TABLE [nomTaula];
`````



## DQL Data Query Language
### Select
Consulta estandard per seleccionar dades d'una o moltes taules

`````sql
--Es seleccionen els camps que es volen veure a la consulta
SELECT p.nom AS nom-professor, a.nom AS nom-alumne -- a i p son alies de taules i puc pusar alies per que es vegi millor el nom del camp
--Dic les taules a les quals he de consultar i puc afegir alies
FROM professor AS p, alumne AS a --Si utilitzo el as li dono un alies a la taula profesor i alumne
--Condicio(no obligatoria) per filtrar dades (En aquest cas nomes es mostraran el nom de a i p (alumne i professor) els quals idprof sigui igual a id -> es a dir, busca quin és el profe de cada alumne (alumne.idProfe)i mira quin profe te aquest id a la seva taula i posa el nom)
WHERE a.idProf = p.id;

--Com a resultat es veuran tots als alumnes i el nom del seu profe
`````
### Funcions i agrupacions (GROUP BY)
#### Funcions
##### SUM()
Funcio que retorna la suma dels valors del camp per parametre
``````sql
--PER CADA CLIENT, MOSTRA LA SUMA DE LES SEVES FACTURES -> AGRUPA PER CADA CLIENT I MOSTRA LA SUMA DE TOTAL_FACTURA DE CADA GRUP
SELECT c.nom, SUM(p.total_factura) 
FROM clients c
JOIN comandes p ON c.id_client = p.id_client
GROUP BY c.nom
ORDER BY total_gastat DESC;
``````
##### MAX()
Funcio que retorna el valor màxim dels valors del camp per parametre
``````sql
--PER CADA CATEGORIA, MOSTRA EL VALOR MAXIM DE PREU
SELECT cat.nom_categoria, MAX(prod.preu)
FROM categories AS cat
JOIN productes AS prod ON cat.id_categoria = prod.id_categoria
GROUP BY cat.nom_categoria
HAVING MAX(prod.preu) > 100;
``````
##### MIN()
Funcio que retorna el valor més petit dels valors del camp per parametre
``````sql
--PER CADA CATEGORIA, MOSTRA EL VALOR MES PETIT DE PREU
SELECT cat.nom_categoria, MIN(prod.preu)
FROM categories AS cat
JOIN productes AS prod ON cat.id_categoria = prod.id_categoria
GROUP BY cat.nom_categoria
HAVING MAX(prod.preu) > 100;
``````
##### AVG()
Funcio que retorna la mitja dels valors del camp passat per parametre
`````sql
--PER CADA ASSIGNATURA MOSTRA LA MITJA DE LA NOTA
SELECT a.nom_assignatura, AVG(n.nota) AS mitja_assignatura
FROM assignatures AS a
JOIN notes AS n ON a.id_assignatura = n.id_assignatura
GROUP BY a.nom_assignatura
HAVING COUNT(n.id_alumne) > 5;
`````
#### Agrupacio i ordre
##### COUNT()
Funcio que retorna el numero de registres del camp pasat per parametre -> també pot calcular els num de registres per grup

### Consultes mutitaula (WHERE = || JOIN)
Per fer consultes multitaula amb WHERE has de relacionar amb = el camp pel que es relacionen les taules:
`````sql
SELECT [taula.camp], [taula.camp]
FROM taula1, taula2
WHERE [taula1-PK] = [taula2-FK] --o la relacio que tinguin, si estan relacionades per department_id(pk de taula 1 y fk de taula2) seria t1.dep_id = t2.dep_id;
`````

Per fer-les amb JOIN és igual pero no cal el where i al from només i haura una taula, les relaciones amb JOIN nomTaula ON igualtat
`````sql
SELECT [taula.camp], [taula.camp]
FROM taula1 as t1
JOIN taula2 as t2 2 ON t1.dep_id = t2.dep_id;
`````
