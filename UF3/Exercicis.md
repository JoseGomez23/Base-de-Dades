<h1>Exercicis UF3</h1>
<a href="https://github.com/JoseGomez23/Base-de-Dades/blob/main/UF3/Database.md">(Click aqui per anar a la base de dades)</a>

____________________________________________________________________________________________________________________________________________________________________
Exercici 1 - Fes una funció anomenada spData, tal que donada una data en format
MySQL ( AAAA-MM-DD ) ens retorni una cadena de caràcters en format DD-MM-AAAA
Exemple : SELECT spData('1988-12-01') => 01-12-1988


Exercici 2 - Fes una funció anomenada spPotencia, tal que donada una base i un
exponent, ens calculi la seva potència. Intenta no utilitzar la funció POW.
Exemple : SELECT spPotencia(2,3) => 8<


Exercici 3 - Fes una funció anomenada spIncrement que donat un codi d’empleat i un
% de increment, ens calculi el salari sumant aquest percentatge.
Per exemple, suposem que l’ empleat amb id_empleat = 124 té un salari de 1000
Exemple: SELECT spIncrement(124,10) obtindriem -> 1100


 Exercici 4 - Fes una funció anomenada spPringat, tal que li passem un codi de
departament, i ens torni el codi d’empleat que guanya menys d’aquell departament.


Exercici 5 - Utilitzant la funció spPringat fes una consulta per obtenir de cada
departament, l’empleat pringat. Mostra el codi i nom del departament, i el codi d’empleat.


Exercici 6 - Fes una funció anomenada spCategoria, tal que donat un codi d’empleat,
ens digui en quina categoria professional està. El criteri que volem seguir per determinar
la categoria professional és en funció dels anys que porta treballant a l’empresa:

Entre 0 i 1 anys -> Auxiliar
Entre 2 i 10 anys -> Oficial de Segona
Entre 11 i 20 Anys -> Oficial de Primera
Més de 20 anys -> Que es jubili!

Exercici 7 - Fes una consulta utilitzant la funció anterior perquè mostri mostri de cada
empleat, el codi d’empleat, el nom, els anys treballats i la categoria professional a la que
pertany.


Exercici 8 - Fes una funció anomenada spEdat, tal que donada una data per paràmetre
ens retorni l'edat d'una persona. Les dates posteriors a la data d'avui han de retornar 0.

  
Exercici 9 - Fes una funció que ens retorni el número de directors (caps) diferents tenim.


Exercici 10 - Quina instrucció utilitzarem si volem veure el contingut de la funció
spPringat?


<h1>Exercicis procediments</h1>

Exercici 1 - Fes un procediment que permeti obtenir la data i hora del sistema i l’usuari
actual.


Exercici 2 - Fes un procediment que intercanvii el sou de dos empleats passats per
paràmetre.


Exercici 3 - Fes un procediment que donat dos Ids d'empleat assigni el codi de
departament del primer en el segon.

```mysql
CREATE PROCEDURE spCambiarDep (IN pEmpleatId1 INT, IN pEmpleatId2 INT)
BEGIN

DECLARE vDepEmp1 INT;
DELIMITER//
-- Comprova que els 22 empleats existeixin
IF spEmpleatExisteix (pEmpleatId1) = 1
    AND spEmpleatExisteix (pEmpleatId2) = 1 THEN
-- Obtenir departament_id de pEmpleatId1
SELECT departament_id INTO vDepEmp1
    FROM empleats
WHERE empleat_id = pEmpleatId1;

-- Modificar departament_id de pEmpleatId2
UPDATE empleats 
    SET departament_id = vDepEmp1
WHERE empleat_id = pEmpleatId2;

END IF; 
END ;
//
```


Exercici 4 - Fes un procediment que donat dos codis de departament assigni tots els
empleats del segon en el primer. Un cop executat el procediment el departament que
correspont en el segon paràmetre ha de quedar desert/sense cap empleat.

```mysql
DROP PROCEDURE IF EXISTS spMoureEmpleats;
DELIMITER //
CREATE PROCEDURE spMoureEmpleats (IN pDepId1 INT , IN pDepId2 INT)
BEGIN

IF pDepId1 IS NOT NULL THEN
	UPDATE empleats
		SET departament_id = pDepId1
	WHERE departament_id = pDepId2;

END IF;

END ;
//
DELIMITER ;
```


Exercici 5 - Fes un procediment per mostrar un llistat dels empleats. Volem veure el
id_empleat, nom_empleat, nom_departament i el nom de la localització del departament.

```mysql
DROP PROCEDURE IF EXISTS spMostrarLlistat;
DELIMITER //
CREATE PROCEDURE spMostrarLlistat ()
BEGIN

	SELECT e.empleat_id, e.nom, d.nom, l.ciutat AS nom
		FROM empleats e
        INNER JOIN departaments d ON d.departament_id = e.departament_id
        INNER JOIN localitzacions l ON l.localitzacio_id = d.localitzacio_id;

END ;
// 
DELIMITER ; 

```


Exercici 6 - Fes un procediment que donat un codi d’empleat, ens doni la informació de
l’empleat ( agafa la informació que creguis rellevant).


Exercici 7 - Volem fer un registre dels usuaris que entren al nostre sistema. Per fer-ho
primer caldrà crear una taula amb dos camps, un per guardar l’usuari i l’altre per guardar
la data i hora de l’accés.


Exercici 8 - A continuació feu un procediment sense arguments, de manera que cada
vegada que el crideu, insereixi en aquesta taula l’usuari actual i la data i hora en que s’ha
executat el procediment.


Exercici 9 - Fes un procediment que ens permeti afegir un nou departament però amb la
següent particularitat: En cas que la localització no existeixi a la taula localitzacions, ens
posarà un NULL en el camp id_localtizacio de la taula departaments. Al procediment li
hem de passar el codi de departament, el nom del departament i el codi de la localització.


Exercici 10 - Fes un procediment que donat un codi d’empleat, ens posi en paràmetres
de sortida el nom i el cognom. Indica com ho pots fer per comprovar si el procediment et
funciona.


Exercici 11 - Fes un procediment que ens permeti modificar el nom i cognom d’un
empleat.

Exercici 12 - Crea una taula d’auditoria anomenada logs_usuaris. Aquesta taula la
utilitzarem per monitoritzar algunes de les accions que fan els usuaris sobre les dades,
per exemple si actualitzen dades, eliminen registres.
La taula ha de tenir els següents camps:

usuari VARCHAR(100) Usuari que ha realitzat l’acció
data DATETIME Data-Hora en que s’ha realitzat l’acció
taula VARCHAR(50) Taula sobre la que es realitza l’acció
accio VARCHAR(20) “ELIMINAR”,”AFEGIR”,”MODIFICAR”,”INSERIR”
valor_pk VARCHAR(200) valor que identifica el registre que ha patit
l’acció

Fes un procediment amb nom spRegistrarLog que rebrà com a paràmetres el nom de la
taula, l’acció i el valor_pk.

Aquest procediment només cal que insereixi un registre en la taula logs_usuaris amb les
dades rebudes, tenint en compte l’usuari actual i la data-hora del sistema.


Exercici 13 - Fes un procediment que ens permeti eliminar un departament determinat.
El departament s’ha d’eliminar de la taula departaments. Utilitza a més dins d’aquest
procediment, el procediment creat anteriorment (spRegistrarLog) per guardar també un
registre del que ha realitzat l’usuari. Ens ha de quedar clar que l’usuari actual, en data X
ha eliminat de la taula DEPARTAMENTS el codi departament Y.

Per exemple, si fem una crida al procediment per eliminar un departament:
CALL eliminarDept(300);

El departament 300 s’ha d’eliminar de la taula departaments, i a més si fem una SELECT
de la taula logs_usuaris, veuriem per exemple el següent:

uauri data taula accio valor_pk
root@localhost 2012-04-30 12:34:00 DEPARTAMENTS ELIMINAR 300


Exercici 14 - Fes un procediment que ens posi en paràmetres de sortida, el número
d’empleats que tenim, el número de departaments i el número de localitzacions.