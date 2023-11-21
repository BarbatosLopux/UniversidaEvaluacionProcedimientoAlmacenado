

* Consulta 1
``` sql

DROP PROCEDURE IF EXISTS Consulta1;
DELIMITER //

CREATE PROCEDURE Consulta1()
BEGIN
    SELECT apellido1, apellido2, nombre FROM persona
    WHERE tipo = 'alumno'
    ORDER BY apellido1, apellido2, nombre;
END //

DELIMITER ;

CALL Consulta1();

``` 
* Consulta 2
``` sql

DROP PROCEDURE IF EXISTS Consulta2;
DELIMITER //

CREATE PROCEDURE Consulta2()
BEGIN
    SELECT nombre, apellido1, apellido2
    FROM persona
    WHERE tipo = 'alumno'
    AND telefono IS NULL;
END //

DELIMITER ;

CALL Consulta2(); 
``` 
* Consulta 3
``` sql
DROP PROCEDURE IF EXISTS Consulta3;

DELIMITER //

CREATE PROCEDURE Consulta3(IN anyio YEAR)
BEGIN
    SELECT nombre, apellido1, apellido2
    FROM persona
    WHERE tipo = 'alumno'
    AND YEAR(fecha_nacimiento) = anyio;
END //

DELIMITER ;

CALL Consulta3(1999);

``` 
* Consulta 4
``` sql
DROP PROCEDURE IF EXISTS Consulta4;

DELIMITER //

CREATE PROCEDURE Consulta4()
BEGIN
    SELECT nombre, apellido1, apellido2
    FROM persona
    WHERE tipo = 'profesor'
    AND telefono IS NULL
    AND nif LIKE '%K';
END //

DELIMITER ;

CALL Consulta4();
``` 
* Consulta 5
``` sql

DROP PROCEDURE IF EXISTS Consulta5;
DELIMITER //

CREATE PROCEDURE Consulta5(IN numero INT, IN grado INT, IN numCurso INT)
BEGIN
    SELECT *
    FROM asignatura
    WHERE cuatrimestre = numero
    AND id_grado = grado
    AND curso = numCurso;
END //

DELIMITER ;

CALL Consulta5(1, 7, 3);

``` 
* Consulta 6
``` sql
DROP PROCEDURE IF EXISTS Consulta6;

DELIMITER // 

CREATE PROCEDURE Consulta6(IN nombre VARCHAR(50))
BEGIN
    SELECT p.nombre, p.apellido1, p.apellido2, p.ciudad, p.direccion, p.telefono, p.fecha_nacimiento, p.sexo
    FROM persona p
    JOIN alumno_se_matricula_asignatura am ON p.id = am.id_alumno
    JOIN asignatura a ON am.id_asignatura = a.id
    JOIN grado g ON a.id_grado = g.id
    WHERE p.tipo = 'alumno' AND p.sexo = 'M' AND g.nombre = nombre;
END //

DELIMITER ;

CALL Consulta6 ('Grado en Ingeniería Informática (Plan 2015)');
``` 
* Consulta 7
``` sql
DROP PROCEDURE IF EXISTS Consulta7;

DELIMITER //
CREATE PROCEDURE Consulta7(IN nombre VARCHAR(50))
BEGIN
    SELECT a.nombre, a.creditos, a.tipo, a.curso, a.cuatrimestre, a.id_profesor, a.id_grado
    FROM asignatura a
    JOIN grado g ON a.id_grado = g.id
    WHERE g.nombre = nombre;
END //

DELIMITER ;

CALL Consulta7 ('Grado en Ingeniería Informática (Plan 2015)'); 
``` 
* Consulta 8
``` sql
DROP PROCEDURE IF EXISTS Consulta8;

DELIMITER //

CREATE PROCEDURE Consulta8(IN nombreTipo VARCHAR(50)) 
BEGIN
    SELECT p.apellido1, p.apellido2, p.nombre, dep.nombre
    FROM persona p
    JOIN profesor pr ON p.id = pr.id_profesor
    JOIN departamento dep ON pr.id_departamento = dep.id
    WHERE p.tipo = nombreTipo
    ORDER BY dep.nombre, p.apellido1, p.apellido2, p.nombre;
END //

DELIMITER ;

CALL Consulta8('profesor'); 
``` 

* Consulta 9
``` sql

DROP PROCEDURE IF EXISTS Consulta9;

DELIMITER //
 
CREATE PROCEDURE Consulta9(IN nif VARCHAR(50))
BEGIN
    SELECT a.nombre, ce.anyo_inicio, ce.anyo_fin
    FROM asignatura a
    JOIN alumno_se_matricula_asignatura ama ON a.id = ama.id_asignatura
    JOIN curso_escolar ce ON ama.id_curso_escolar = ce.id
    JOIN persona p ON ama.id_alumno = p.id
    WHERE p.nif = nif; 
END //

DELIMITER ;

CALL Consulta9('26902806M'); 
``` 
* Consulta 10
``` sql
DROP PROCEDURE IF EXISTS Consulta10;

DELIMITER //

CREATE PROCEDURE Consulta10(IN nombre VARCHAR(50))
BEGIN
    SELECT DISTINCT dep.nombre
    FROM departamento dep
    JOIN profesor pr ON dep.id = pr.id_departamento
    JOIN persona p ON pr.id_profesor = p.id
    JOIN asignatura ag ON p.id = ag.id_profesor
    JOIN grado g ON ag.id_grado = g.id
    WHERE g.nombre = nombre;
END //

DELIMITER ;

CALL Consulta10('Grado en Ingeniería Informática (Plan 2015)')
```
* Consulta 11
``` sql
DROP PROCEDURE IF EXISTS Consulta11 ;

DELIMITER // 

CREATE PROCEDURE Consulta11(IN anyo INT)
BEGIN
    SELECT DISTINCT p.nombre, CONCAT(p.apellido1, " ", p.apellido2) AS Apellidos_alumnos
    FROM persona p
    JOIN alumno_se_matricula_asignatura ama ON p.id = ama.id_alumno
    JOIN curso_escolar ce ON ama.id_curso_escolar = ce.id
    WHERE ce.anyo_inicio = 2018 AND ce.anyo_fin = anyo;
END //

DELIMITER ;

CALL Consulta11(2019); 
```
* Consulta 12
``` sql

DROP PROCEDURE IF EXISTS Consulta12 ;

DELIMITER //

CREATE PROCEDURE Consulta12()
BEGIN
    SELECT dep.nombre AS nombre_departamento, p.apellido1, p.apellido2, p.nombre AS nombre_profesor
    FROM departamento dep
    LEFT JOIN profesor pr ON dep.id = pr.id_departamento
    LEFT JOIN persona p ON pr.id_profesor = p.id
    ORDER BY nombre_departamento, p.apellido1, p.apellido2, p.nombre;
END //

DELIMITER ;

CALL Consulta12(); 
``` 
* Consulta 13
``` sql

DROP PROCEDURE IF EXISTS Consulta13 ;

DELIMITER // 

CREATE PROCEDURE Consulta13()
BEGIN
    SELECT p.nombre, CONCAT(p.apellido1, " ", p.apellido2) AS Apellidos_profesores
    FROM persona p
    LEFT JOIN profesor pr ON p.id = pr.id_profesor
    WHERE pr.id_profesor IS NULL;
END //

DELIMITER ;

CALL Consulta13(); 
```
* Consulta 14
``` sql

DROP PROCEDURE IF EXISTS Consulta14 ;

DELIMITER // 

CREATE PROCEDURE Consulta14()
BEGIN
    SELECT p.nombre, CONCAT(p.apellido1, " ", p.apellido2) AS Apellidos_profesores
    FROM persona p
    LEFT JOIN asignatura ag ON p.id = ag.id_profesor
    WHERE ag.id_profesor IS NULL;
END //

DELIMITER ;

CALL Consulta14(); 
``` 
* Consulta 15
``` sql

DROP PROCEDURE IF EXISTS Consulta15 ;

DELIMITER // 

CREATE PROCEDURE Consulta15()
BEGIN
    SELECT ag.nombre, ag.creditos, ag.tipo, ag.curso, ag.cuatrimestre
    FROM asignatura ag
    LEFT JOIN persona p ON ag.id_profesor = p.id
    WHERE ag.id_profesor IS NULL;
END //

DELIMITER ;

CALL Consulta15(); 
``` 

* Consulta 16
``` sql

DROP PROCEDURE IF EXISTS Consulta16 ;

DELIMITER // 

CREATE PROCEDURE Consulta16()
BEGIN
    SELECT DISTINCT d.nombre AS nombre_departamento, a.nombre AS nombre_asignatura
    FROM departamento d
    JOIN asignatura a
    JOIN profesor pr ON d.id = pr.id_departamento
    LEFT JOIN alumno_se_matricula_asignatura ama ON a.id = ama.id_asignatura
    WHERE ama.id_asignatura IS NULL;
END //

DELIMITER ;

CALL Consulta16(); 
``` 

* Consulta 17
``` sql

DROP PROCEDURE IF EXISTS Consulta17 ;

DELIMITER // 

CREATE PROCEDURE Consulta17(IN tipoSex VARCHAR(50))
BEGIN
    SELECT COUNT(id) AS cantidad_alumnas
    FROM persona
    WHERE sexo = tiposex;
END //

DELIMITER ;

CALL Consulta17('M'); 
``` 
* Consulta 18
``` sql

DROP PROCEDURE IF EXISTS Consulta18 ;

DELIMITER // 

CREATE PROCEDURE Consulta18()
BEGIN
    SELECT COUNT(id) AS cantidad_alumnos
    FROM persona
    WHERE YEAR(fecha_nacimiento) = 1999;
END //

DELIMITER ;

CALL Consulta18(); 
``` 
* Consulta 19
``` sql

DROP PROCEDURE IF EXISTS Consulta19 ;

DELIMITER // 

CREATE PROCEDURE Consulta19()
BEGIN
    SELECT COUNT(pr.id_profesor) AS numero_profesores, dep.nombre AS nombre_departamento
    FROM departamento dep
    JOIN profesor pr ON dep.id = pr.id_departamento
    GROUP BY dep.nombre
    ORDER BY numero_profesores DESC;
END //

DELIMITER ;

CALL Consulta19(); 
``` 
* Consulta 20
``` sql

DROP PROCEDURE IF EXISTS Consulta20 ;

DELIMITER // 

CREATE PROCEDURE Consulta20()
BEGIN
    SELECT d.nombre AS nombre_departamento, COUNT(p.id) AS numero_profesores
    FROM departamento d
    LEFT JOIN profesor pr ON d.id = pr.id_departamento
    LEFT JOIN persona p ON pr.id_profesor = p.id
    GROUP BY d.nombre;
END //

DELIMITER ;

CALL Consulta20(); 
``` 
* Consulta 21
``` sql

DROP PROCEDURE IF EXISTS Consulta21 ;

DELIMITER // 

CREATE PROCEDURE Consulta21()
BEGIN
    SELECT g.nombre AS nombre_grado, COUNT(ag.id) AS numero_asignaturas
    FROM grado g
    LEFT JOIN asignatura ag ON g.id = ag.id_grado
    GROUP BY g.nombre
    ORDER BY numero_asignaturas DESC;
END //

DELIMITER ;

CALL Consulta21(); 
``` 

* Consulta 22
``` sql

DROP PROCEDURE IF EXISTS Consulta22 ;

DELIMITER // 

CREATE PROCEDURE Consulta22()
BEGIN
    SELECT g.nombre AS nombre_grado, COUNT(ag.id) AS numero_asignaturas
    FROM grado g
    LEFT JOIN asignatura ag ON g.id = ag.id_grado
    GROUP BY g.nombre
    HAVING COUNT(ag.id) > 40;
END //

DELIMITER ;

CALL Consulta22(); 
``` 
* Consulta 23
``` sql

DROP PROCEDURE IF EXISTS Consulta23 ;

DELIMITER // 

CREATE PROCEDURE Consulta23()
BEGIN
    SELECT g.nombre AS nombre_grado, ag.tipo, SUM(ag.creditos) AS total_creditos
    FROM grado g
    LEFT JOIN asignatura ag ON g.id = ag.id_grado
    GROUP BY g.nombre, ag.tipo
    ORDER BY total_creditos DESC;
END //

DELIMITER ;

CALL Consulta23(); 
```
* Consulta 24
``` sql

DROP PROCEDURE IF EXISTS Consulta24 ;

DELIMITER // 

CREATE PROCEDURE Consulta24()
BEGIN
    SELECT cs.anyo_inicio AS Año_inicio_escolar, COUNT(ams.id_alumno) AS Numero_de_alumnos_registrados
    FROM alumno_se_matricula_asignatura ams
    LEFT JOIN curso_escolar cs ON cs.id = ams.id_curso_escolar
    GROUP BY cs.anyo_inicio;
END //

DELIMITER ;

CALL Consulta24(); 
``` 
* Consulta 25
``` sql

DROP PROCEDURE IF EXISTS Consulta25 ;

DELIMITER // 

CREATE PROCEDURE Consulta25()
BEGIN
    SELECT p.id, p.nombre, p.apellido1, p.apellido2, COUNT(a.id) AS numero_asignaturas
    FROM persona p
    LEFT JOIN profesor pr ON p.id = pr.id_profesor
    LEFT JOIN asignatura a ON pr.id_profesor = a.id_profesor
    GROUP BY p.id, p.nombre, p.apellido1, p.apellido2
    ORDER BY numero_asignaturas DESC;
END //

DELIMITER ;

CALL Consulta25(); 
``` 
* Consulta 26
``` sql

DROP PROCEDURE IF EXISTS Consulta26 ;

DELIMITER // 

CREATE PROCEDURE Consulta26()
BEGIN
    SELECT *
    FROM persona
    WHERE fecha_nacimiento = (SELECT MAX(fecha_nacimiento) FROM persona);
END //

DELIMITER ;

CALL Consulta26(); 
``` 
* Consulta 27
``` sql

DROP PROCEDURE IF EXISTS Consulta27 ;

DELIMITER // 

CREATE PROCEDURE Consulta27()
BEGIN
    SELECT p.id AS id_profesor, p.nombre AS nombre_profesor
    FROM persona p
    LEFT JOIN profesor pr ON p.id = pr.id_profesor
    WHERE pr.id_departamento IS NULL AND p.tipo = 'profesor';
END //

DELIMITER ;

CALL Consulta27(); 
```
* Consulta 28
``` sql

DROP PROCEDURE IF EXISTS Consulta28 ;

DELIMITER // 

CREATE PROCEDURE Consulta28()
BEGIN
    SELECT d.id AS id_departamento, d.nombre AS nombre_departamento
    FROM departamento d
    LEFT JOIN profesor pr ON d.id = pr.id_departamento
    WHERE pr.id_profesor IS NULL;
END //

DELIMITER ;

CALL Consulta28(); 
``` 

* Consulta 29
``` sql

DROP PROCEDURE IF EXISTS Consulta29 ;

DELIMITER // 

CREATE PROCEDURE Consulta29()
BEGIN
    SELECT p.id AS id_profesor, p.nombre AS nombre_profesor
    FROM persona p
    INNER JOIN profesor pr ON p.id = pr.id_profesor
    LEFT JOIN asignatura a ON pr.id_profesor = a.id_profesor
    WHERE pr.id_departamento IS NOT NULL AND a.id IS NULL AND p.tipo = 'profesor';
END //

DELIMITER ;

CALL Consulta29(); 
``` 
* Consulta 30
``` sql

DROP PROCEDURE IF EXISTS Consulta30 ;

DELIMITER // 

CREATE PROCEDURE Consulta30()
BEGIN
    SELECT a.id AS id_asignatura, a.nombre AS nombre_asignatura
    FROM asignatura a
    LEFT JOIN profesor pr ON a.id_profesor = pr.id_profesor
    WHERE pr.id_profesor IS NULL;
END //

DELIMITER ;

CALL Consulta30(); 
``` 
* Consulta 31
``` sql

DROP PROCEDURE IF EXISTS Consulta31 ;

DELIMITER // 

CREATE PROCEDURE Consulta31()
BEGIN
    SELECT d.id AS id_departamento, d.nombre AS nombre_departamento
    FROM departamento d
    LEFT JOIN profesor pr ON d.id = pr.id_departamento
    LEFT JOIN asignatura a ON pr.id_profesor = a.id_profesor
    LEFT JOIN alumno_se_matricula_asignatura am ON a.id = am.id_asignatura
    WHERE am.id_curso_escolar IS NULL;
END //

DELIMITER ;

CALL Consulta31(); 
``` 


