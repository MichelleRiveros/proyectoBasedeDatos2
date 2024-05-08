# proyectoBasedeDatos2

1. Archivo sql con la estructura de la base de datos:

![image](1.png)}

2. DDL / Creacion de la base de datos:

```mysql
mysql> CREATE DATABASE proyecto2;
Query OK, 1 row affected (0.02 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| normalizacion      |
| performance_schema |
| proyecto           |
| proyecto2          |
| sys                |
| work               |
+--------------------+
8 rows in set (0.00 sec)
```

3. Creacion de las tablas:
```mysql
CREATE TABLE pais(
    id_pais INT(11) NOT NULL AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    PRIMARY KEY(id_pais)
);

CREATE TABLE region(
    id_region INT(11) NOT NULL AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    id_pais INT(11) NOT NULL,
    PRIMARY KEY(id_region),
    CONSTRAINT FK_region_pais FOREIGN KEY (id_pais) REFERENCES 
    pais(id_pais)
);

CREATE TABLE ciudad(
    id_ciudad INT(11) NOT NULL AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    id_region INT(11) NOT NULL,
    PRIMARY KEY(id_ciudad),
    CONSTRAINT FK_ciudad_region FOREIGN KEY (id_region) REFERENCES 
    region(id_region)
);

CREATE TABLE direccion(
    id_direccion INT(11) NOT NULL AUTO_INCREMENT,
    linea_direccion VARCHAR(50) NOT NULL,
    barrio VARCHAR(100) NOT NULL,
    codigo_postal VARCHAR(10),
    id_ciudad INT(11) NOT NULL,
    PRIMARY KEY(id_direccion),
    CONSTRAINT FK_direccion_ciudad FOREIGN KEY(id_ciudad) REFERENCES ciudad(id_ciudad)
);

CREATE TABLE sexo(
    id_sexo INT(11) NOT NULL AUTO_INCREMENT,
    tipo VARCHAR(100) NOT NULL,
    PRIMARY KEY(id_sexo)
);

CREATE TABLE tipo_asignatura(
    id_tipo INT(11) NOT NULL AUTO_INCREMENT,
    tipo_asignatura VARCHAR(50),
    PRIMARY KEY(id_tipo)
);

CREATE TABLE departamento(
    id_departamento INT(11) NOT NULL AUTO_INCREMENT,
    nombre VARCHAR(50),
    PRIMARY KEY(id_departamento)
);

CREATE TABLE grado(
    id_grado INT(11) NOT NULL AUTO_INCREMENT,
    nombre VARCHAR(100),
    anyo YEAR(4),
    PRIMARY KEY(id_grado)
);

CREATE TABLE curso_escolar(
    id_curso INT(11) NOT NULL AUTO_INCREMENT,
    anyo_inicio YEAR(4),
    anyo_fin YEAR (4),
    PRIMARY KEY(id_curso)
);

CREATE TABLE telefono(
    id_telefono INT(11) NOT NULL AUTO_INCREMENT,
    numero VARCHAR(9) NOT NULL,
    prefijo VARCHAR(9) NOT NULL,
    tipo_de_telefono VARCHAR(9) NOT NULL,
    PRIMARY KEY(id_telefono)
);

CREATE TABLE profesor(
    id_profesor INT(11) NOT NULL AUTO_INCREMENT,
    nif VARCHAR(9) NOT NULL,
    nombre_profesor VARCHAR(25) NOT NULL,
    apellido1_profesor VARCHAR(50) NOT NULL,
    apellido2_profesor VARCHAR(50) NOT NULL,
    fecha_nacimiento DATE NOT NULL,
    id_sexo INT(11) NOT NULL,
    id_departamento INT(11) NOT NULL,
    PRIMARY KEY(id_profesor),
    CONSTRAINT FK_sexo_profesor FOREIGN KEY (id_sexo) REFERENCES 
    sexo(id_sexo),
    CONSTRAINT FK_departamento FOREIGN KEY (id_departamento) REFERENCES departamento(id_departamento)
);

CREATE TABLE profesor_direccion (
    id_profesor INT(11) NOT NULL,
    id_direccion INT(11) NOT NULL,
    PRIMARY KEY (id_profesor, id_direccion),
    FOREIGN KEY (id_profesor) REFERENCES profesor(id_profesor),
    FOREIGN KEY (id_direccion) REFERENCES direccion(id_direccion)
);

CREATE TABLE profesor_telefono (
    id_profesor INT(11) NOT NULL,
    id_telefono INT(11) NOT NULL,
    PRIMARY KEY (id_profesor, id_telefono),
    FOREIGN KEY (id_profesor) REFERENCES profesor(id_profesor),
    FOREIGN KEY (id_telefono) REFERENCES telefono(id_telefono)
);

CREATE TABLE asignatura(
    id_asignatura INT(10) NOT NULL AUTO_INCREMENT,
    nombre VARCHAR(100) NOT NULL,
    creditos FLOAT NOT NULL,
    cuatrimestre TINYINT(3) NOT NULL,
    id_tipo_asignatura INT(11) NOT NULL,
    id_curso INT(11) NOT NULL,
    id_profesor INT(11) NOT NULL,
    id_grado INT(11) NOT NULL,
    PRIMARY KEY(id_asignatura),
    CONSTRAINT FK_tipo_asignatura FOREIGN KEY (id_tipo_asignatura) 
    REFERENCES tipo_asignatura(id_tipo),
    CONSTRAINT FK_curso FOREIGN KEY (id_curso) REFERENCES 
    curso_escolar(id_curso),
    CONSTRAINT FK_profesor FOREIGN KEY (id_profesor) REFERENCES 
    profesor(id_profesor),
    CONSTRAINT FK_grado FOREIGN KEY (id_grado) REFERENCES 
    grado(id_grado)
);

CREATE TABLE alumno(
    id_alumno INT(11) NOT NULL AUTO_INCREMENT,
    nif VARCHAR(9) NOT NULL,
    nombre_alumno VARCHAR(25) NOT NULL,
    apellido1_alumno VARCHAR(50) NOT NULL,
    apellido2_alumno VARCHAR(50) NOT NULL,
    fecha_nacimiento DATE NOT NULL,
    id_sexo INT(11) NOT NULL,
    PRIMARY KEY(id_alumno),
    CONSTRAINT FK_sexo_alumno FOREIGN KEY (id_sexo) REFERENCES 
    sexo(id_sexo)
);

CREATE TABLE alumno_direccion (
    id_alumno INT(11) NOT NULL,
    id_direccion INT(11) NOT NULL,
    PRIMARY KEY (id_alumno, id_direccion),
    FOREIGN KEY (id_alumno) REFERENCES alumno(id_alumno),
    FOREIGN KEY (id_direccion) REFERENCES direccion(id_direccion)
);

CREATE TABLE alumno_telefono (
    id_alumno INT(11) NOT NULL,
    id_telefono INT(11) NOT NULL,
    PRIMARY KEY (id_alumno, id_telefono),
    FOREIGN KEY (id_alumno) REFERENCES alumno(id_alumno),
    FOREIGN KEY (id_telefono) REFERENCES telefono(id_telefono)
);

CREATE TABLE alumno_se_matricula_asignatura(
    id_alumno INT(11) NOT NULL,
    id_asignatura INT(11) NOT NULL,
    id_curso INT(11) NOT NULL,
    PRIMARY KEY(id_alumno, id_asignatura, id_curso),
    CONSTRAINT FK_alumno_matricula FOREIGN KEY (id_alumno) REFERENCES 
    alumno(id_alumno),
    CONSTRAINT FK_asignatura_matricula FOREIGN KEY (id_asignatura) 
    REFERENCES asignatura(id_asignatura),
    CONSTRAINT FK_curso_matricula FOREIGN KEY (id_curso) REFERENCES 
    curso_escolar(id_curso)
);
    
+--------------------------------+
| Tables_in_proyecto2            |
+--------------------------------+
| alumno                         |
| alumno_direccion               |
| alumno_se_matricula_asignatura |
| alumno_telefono                |
| asignatura                     |
| ciudad                         |
| curso_escolar                  |
| departamento                   |
| direccion                      |
| grado                          |
| pais                           |
| profesor                       |
| profesor_direccion             |
| profesor_telefono              |
| region                         |
| sexo                           |
| telefono                       |
| tipo_asignatura                |
+--------------------------------+
18 rows in set (0.01 sec)
```

**DML** / **INSERT**

```mysql

-- Países
INSERT INTO pais (nombre) VALUES ('España');

-- Regiones
INSERT INTO region (nombre, id_pais) VALUES ('Andalucía', 1);

-- Ciudades
INSERT INTO ciudad (nombre, id_region) VALUES ('Almería', 1);

-- tipo_asignatura
INSERT INTO tipo_asignatura VALUES 
(1, 'Básica'),
(2, 'Obligatoria'),
(3, 'Optativa');

-- grado
INSERT INTO grado VALUES 
(1, 'Grado en Ingeniería Agrícola', 2015),
(2, 'Grado en Ingeniería Eléctrica', 2014),
(3, 'Grado en Ingeniería Electrónica Industrial', 2010),
(4, 'Grado en Ingeniería Informática', 2015),
(5, 'Grado en Ingeniería Mecánica', 2010),
(6, 'Grado en Ingeniería Química Industrial', 2010),
(7, 'Grado en Biotecnología', 2015),
(8, 'Grado en Ciencias Ambientales', 2009),
(9, 'Grado en Matemáticas', 2010),
(10, 'Grado en Química', 2009);

--  curso_escolar
INSERT INTO curso_escolar VALUES 
(1, 2014, 2015),
(2, 2015, 2016),
(3, 2016, 2017),
(4, 2017, 2018);

- sexo
INSERT INTO sexo (tipo) VALUES
('Masculino'),
('Femenino'),
('No binario'),
('Género fluido'),
('Otro');

--  departamento
INSERT INTO departamento VALUES (1, 'Informática');
INSERT INTO departamento VALUES (2, 'Matemáticas');
INSERT INTO departamento VALUES (3, 'Economía y Empresa');
INSERT INTO departamento VALUES (4, 'Educación');
INSERT INTO departamento VALUES (5, 'Agronomía');
INSERT INTO departamento VALUES (6, 'Química y Física');
INSERT INTO departamento VALUES (7, 'Filología');
INSERT INTO departamento VALUES (8, 'Derecho');
INSERT INTO departamento VALUES (9, 'Biología y Geología');

- direccion
INSERT INTO direccion (linea_direccion, barrio, codigo_postal, id_ciudad) VALUES
('C/ Alameda, 12', 'Centro', '28001', 1),
('Av. de la Constitución, 45', 'La Latina', '28005', 1),
('C/ Gran Vía, 123', 'Malasaña', '28004', 1),
('C/ Fuencarral, 67', 'Chueca', '28010', 1),
('Paseo del Prado, 5', 'Retiro', '28014', 1),
('C/ Serrano, 100', 'Salamanca', '28006', 1),
('Av. de América, 33', 'Chamartín', '28002', 1),
('C/ Velázquez, 20', 'Barrio de Salamanca', '28001', 1),
('C/ Goya, 15', 'Goya', '28009', 1),
('C/ Hortaleza, 35', 'Chueca', '28004', 1),
('C/ Atocha, 55', 'Las Letras', '28012', 1),
('C/ Mayor, 25', 'Sol', '28013', 1),
('C/ Argumosa, 10', 'Lavapiés', '28012', 1),
('Paseo de la Castellana, 200', 'Chamartín', '28046', 1),
('C/ Princesa, 70', 'Moncloa', '28008', 1),
('Av. de América, 160', 'Prosperidad', '28002', 1),
('C/ Santa Engracia, 100', 'Chamberí', '28010', 1),
('C/ Alberto Aguilera, 32', 'Universidad', '28015', 1),
('C/ Serrano, 50', 'Recoletos', '28001', 1),
('C/ Bravo Murillo, 340', 'Cuatro Caminos', '28020', 1),
('C/ Fuencarral, 90', 'Triball', '28004', 1),
('C/ Orense, 15', 'Azca', '28020', 1),
('C/ Gran Vía, 350', 'Callao', '28013', 1),
('C/ Preciados, 25', 'Sol', '28004', 1),
('C/ Príncipe de Vergara, 100', 'Lista', '28006', 1),
('C/ Génova, 25', 'Chamberí', '28004', 1),
('Paseo de la Habana, 75', 'Chamartín', '28036', 1),
('C/ Arturo Soria, 70', 'Pinar de Chamartín', '28033', 1),
('C/ Bravo Murillo, 230', 'Tetuán', '28020', 1);


- telefono
INSERT INTO telefono (numero, prefijo, tipo_de_telefono) VALUES
('123456789', '+34', 'Móvil'),
('987654321', '+34', 'Fijo'),
('246813579', '+34', 'Trabajo'),
('555555555', '+34', 'Fax'),
('999888777', '+34', 'Móvil'),
('111222333', '+34', 'Fijo'),
('777777777', '+34', 'Trabajo'),
('444444444', '+34', 'Fax'),
('666666666', '+34', 'Móvil'),
('222333444', '+34', 'Fijo'),
('888888888', '+34', 'Trabajo'),
('333333333', '+34', 'Fax'),
('123123123', '+34', 'Móvil'),
('456456456', '+34', 'Fijo'),
('789789789', '+34', 'Trabajo'),
('111111111', '+34', 'Fax'),
('222222222', '+34', 'Móvil'),
('333333333', '+34', 'Fijo'),
('444444444', '+34', 'Trabajo'),
('555555555', '+34', 'Fax'),
('666666666', '+34', 'Móvil'),
('777777777', '+34', 'Fijo'),
('888888888', '+34', 'Trabajo'),
('999999999', '+34', 'Fax'),
('000000000', '+34', 'Móvil'),
('111111111', '+34', 'Fijo'),
('222222222', '+34', 'Trabajo'),
('333333333', '+34', 'Fax');

- profesor

INSERT INTO profesor VALUES (3, '11105554G', 'Zoe', 'Ramirez', 'Gea', '1979-08-19', 3, 1);
INSERT INTO profesor VALUES (5, '38223286T', 'David', 'Schmidt', 'Fisher', '1978-01-19', 2, 2);
INSERT INTO profesor VALUES (8, '79503962T', 'Cristina', 'Lemke', 'Rutherford', '1977-08-21', 3, 3);
INSERT INTO profesor VALUES (10, '61142000L', 'Esther', 'Spencer', 'Lakin', '1977-05-19', 3, 4);
INSERT INTO profesor VALUES (12, '85366986W', 'Carmen', 'Streich', 'Hirthe', '1971-04-29', 3, 4);
INSERT INTO profesor VALUES (13, '73571384L', 'Alfredo', 'Stiedemann', 'Morissette', '1980-02-01', 2, 6);
INSERT INTO profesor VALUES (14, '82937751G', 'Manolo', 'Hamill', 'Kozey', '1977-01-02', 2, 1);
INSERT INTO profesor VALUES (15, '80502866Z', 'Alejandro', 'Kohler', 'Schoen', '1980-03-14', 2, 2);
INSERT INTO profesor VALUES (16, '10485008K', 'Antonio', 'Fahey', 'Considine', '1982-03-18', 2, 3);
INSERT INTO profesor VALUES (17, '85869555K', 'Guillermo', 'Ruecker', 'Upton', '1973-05-05', 2, 4);
INSERT INTO profesor VALUES (18, '04326833G', 'Micaela', 'Monahan', 'Murray', '1976-02-25', 2, 5);
INSERT INTO profesor VALUES (20, '79221403L', 'Francesca', 'Schowalter', 'Muller', '1980-10-31', 2, 6);
INSERT INTO profesor VALUES (21, '13175769N', 'Pepe', 'Sánchez', 'Ruiz', '1980-10-16', 2, 1);
INSERT INTO profesor VALUES (22, '98816696W', 'Juan', 'Guerrero', 'Martínez', '1980-11-21', 2, 1);
INSERT INTO profesor VALUES (23, '77194445M', 'María', 'Domínguez', 'Hernández', '1980-12-13', 3, 2);

- Asignatura

INSERT INTO asignatura VALUES (1, 'Álgegra lineal y matemática discreta', 6, 1, 1, 2, 3, 4);
INSERT INTO asignatura VALUES (2, 'Cálculo', 6, 1, 1, 3, 5, 4);
INSERT INTO asignatura VALUES (3, 'Física para informática', 6, 1, 1, 2, 8, 4);
INSERT INTO asignatura VALUES (4, 'Introducción a la programación', 6, 1, 1, 1, 10, 4);
INSERT INTO asignatura VALUES (5, 'Organización y gestión de empresas', 6, 1, 1, 4, 12, 4);
INSERT INTO asignatura VALUES (6, 'Estadística', 6, 2, 1, 1, 13, 4);
INSERT INTO asignatura VALUES (7, 'Estructura y tecnología de computadores', 6, 2, 1, 2, 14, 4);
INSERT INTO asignatura VALUES (8, 'Fundamentos de electrónica', 6, 2, 1, 3, 15, 4);
INSERT INTO asignatura VALUES (9, 'Lógica y algorítmica', 6, 1, 1, 3, 16, 4);
INSERT INTO asignatura VALUES (10, 'Metodología de la programación', 6, 1, 3, 2,  18, 4);
INSERT INTO asignatura VALUES (11, 'Arquitectura de Computadores', 6, 1,1, 3, 14,  4);
INSERT INTO asignatura VALUES (12, 'Estructura de Datos y Algoritmos I', 6, 1,2, 3, 10, 4);
INSERT INTO asignatura VALUES (13, 'Ingeniería del Software', 6, 1, 2, 3, 20, 4);
INSERT INTO asignatura VALUES (14, 'Sistemas Inteligentes', 6, 1, 2, 3, 22, 4);
INSERT INTO asignatura VALUES (15, 'Sistemas Operativos', 6, 1, 2, 3, 21, 4);
INSERT INTO asignatura VALUES (16, 'Bases de Datos', 6, 2, 1, 14, 17, 4);
INSERT INTO asignatura VALUES (17, 'Estructura de Datos y Algoritmos II', 6, 2, 2, 14, 4);
INSERT INTO asignatura VALUES (18, 'Fundamentos de Redes de Computadores', 6, 2, 2, 3, 15, 4);
INSERT INTO asignatura VALUES (19, 'Planificación y Gestión de Proyectos Informáticos', 6, 2, 2, 3, 16, 4);
INSERT INTO asignatura VALUES (20, 'Programación de Servicios Software', 6, 2, 1,  2, 14, 4);
INSERT INTO asignatura VALUES (21, 'Desarrollo de interfaces de usuario', 6, 3, 2, 4, 14, 4);
INSERT INTO asignatura VALUES (22, 'Ingeniería de Requisitos', 6, 3, 3, 2, 21, 4);
INSERT INTO asignatura VALUES (23, 'Integración de las Tecnologías de la Información en las Organizaciones', 6, 3, 3, 1, 22, 4);
INSERT INTO asignatura VALUES (24, 'Modelado y Diseño del Software 1', 6, 3, 3, 2, 20, 4);
INSERT INTO asignatura VALUES (25, 'Multiprocesadores', 6, 3, 3, 3, 23, 4);
INSERT INTO asignatura VALUES (26, 'Seguridad y cumplimiento normativo', 6, 3, 3, 4, 23, 4);
INSERT INTO asignatura VALUES (27, 'Sistema de Información para las Organizaciones', 6, 3, 3, 1, 23, 4); 
INSERT INTO asignatura VALUES (28, 'Tecnologías web', 6, 3, 3, 1, 12, 4);
INSERT INTO asignatura VALUES (29, 'Teoría de códigos y criptografía', 6, 3, 3, 3, 5, 4);
INSERT INTO asignatura VALUES (30, 'Administración de bases de datos', 6, 4, 3, 4, 8, 4);
INSERT INTO asignatura VALUES (31, 'Herramientas y Métodos de Ingeniería del Software', 6, 4, 1, 2, 10, 4);
INSERT INTO asignatura VALUES (32, 'Informática industrial y robótica', 6, 4, 1, 3, 12, 4);
INSERT INTO asignatura VALUES (33, 'Ingeniería de Sistemas de Información', 6, 4, 2, 4, 13, 4);
INSERT INTO asignatura VALUES (34, 'Modelado y Diseño del Software 2', 6, 4, 3, 1, 14, 4);
INSERT INTO asignatura VALUES (35, 'Negocio Electrónico', 6, 4, 2, 1, 22, 4);
INSERT INTO asignatura VALUES (36, 'Periféricos e interfaces', 6, 4, 3, 4, 21, 4);
INSERT INTO asignatura VALUES (37, 'Sistemas de tiempo real', 6, 4, 2, 2, 23, 4);
INSERT INTO asignatura VALUES (38, 'Tecnologías de acceso a red', 6, 4, 1, 1, 17, 4);
INSERT INTO asignatura VALUES (39, 'Tratamiento digital de imágenes', 6, 4, 1, 3, 15, 4);
INSERT INTO asignatura VALUES (40, 'Administración de redes y sistemas operativos', 6, 5, 3, 4, 13, 4);
INSERT INTO asignatura VALUES (41, 'Almacenes de Datos', 6, 1, 2, 3, 14, 7);

- profesor_telefono

INSERT INTO profesor_telefono (id_profesor, id_telefono) VALUES
(3, 1), -- Zoe Ramirez tiene el teléfono móvil con número 123456789
(3, 4), -- Zoe Ramirez tiene el teléfono de trabajo con número 555555555
(5, 2), -- David Schmidt tiene el teléfono fijo con número 987654321
(8, 3), -- Cristina Lemke tiene el teléfono de trabajo con número 246813579
(10, 4), -- Esther Spencer tiene el teléfono de trabajo con número 555555555
(12, 4), -- Carmen Streich tiene el teléfono de trabajo con número 555555555
(13, 6), -- Alfredo Stiedemann tiene el teléfono móvil con número 123123123
(14, 1), -- Manolo Hamill tiene el teléfono móvil con número 123456789
(15, 2), -- Alejandro Kohler tiene el teléfono fijo con número 987654321
(16, 3), -- Antonio Fahey tiene el teléfono de trabajo con número 246813579
(17, 4), -- Guillermo Ruecker tiene el teléfono de trabajo con número 555555555
(18, 5), -- Micaela Monahan tiene el teléfono de fax con número 333333333
(20, 6), -- Francesca Schowalter tiene el teléfono móvil con número 123123123
(21, 1), -- Pepe Sánchez tiene el teléfono móvil con número 123456789
(22, 1), -- Juan Guerrero tiene el teléfono móvil con número 123456789
(23, 2); -- María Domínguez tiene el teléfono fijo con número 987654321

- profesor_direccion

INSERT INTO profesor_direccion (id_profesor, id_direccion) VALUES
(3, 1), -- Zoe Ramirez reside en la dirección C/ Alameda, 12, Centro, 28001
(5, 2), -- David Schmidt reside en la dirección Av. de la Constitución, 45, La Latina, 28005
(8, 3), -- Cristina Lemke reside en la dirección C/ Gran Vía, 123, Malasaña, 28004
(10, 4), -- Esther Spencer reside en la dirección C/ Fuencarral, 67, Chueca, 28010
(12, 4), -- Carmen Streich reside en la dirección C/ Fuencarral, 67, Chueca, 28010
(13, 6), -- Alfredo Stiedemann reside en la dirección Av. de América, 33, Chamartín, 28002
(14, 1), -- Manolo Hamill reside en la dirección C/ Alameda, 12, Centro, 28001
(15, 2), -- Alejandro Kohler reside en la dirección Av. de la Constitución, 45, La Latina, 28005
(16, 3), -- Antonio Fahey reside en la dirección C/ Gran Vía, 123, Malasaña, 28004
(17, 4), -- Guillermo Ruecker reside en la dirección C/ Fuencarral, 67, Chueca, 28010
(18, 5), -- Micaela Monahan reside en la dirección Paseo del Prado, 5, Retiro, 28014
(20, 6), -- Francesca Schowalter reside en la dirección Av. de América, 33, Chamartín, 28002
(21, 1), -- Pepe Sánchez reside en la dirección C/ Alameda, 12, Centro, 28001
(22, 1), -- Juan Guerrero reside en la dirección C/ Alameda, 12, Centro, 28001
(23, 2); -- María Domínguez reside en la dirección Av. de la Constitución, 45, La Latina, 28005

- Alumno

INSERT INTO alumno VALUES (1, '89542419S', 'Juan', 'Saez', 'Vega', '1992/08/08', 1);
INSERT INTO alumno VALUES (2, '26902806M', 'Salvador', 'Sánchez', 'Pérez', '1991/03/28', 1);
INSERT INTO alumno VALUES (4, '17105885A', 'Pedro', 'Heller', 'Pagac', '2000/10/05', 1);
INSERT INTO alumno VALUES (6, '04233869Y', 'José', 'Koss', 'Bayer', '1998/01/28', 1);
INSERT INTO alumno VALUES (7, '97258166K', 'Ismael', 'Strosin', 'Turcotte', '1999/05/24', 1);
INSERT INTO alumno VALUES (9, '82842571K', 'Ramón', 'Herzog', 'Tremblay', '1996/11/21', 2);
INSERT INTO alumno VALUES (11, '46900725E', 'Daniel', 'Herman', 'Pacocha', '1997/04/26', 1);
INSERT INTO alumno VALUES (19, '11578526G', 'Inma', 'Lakin', 'Yundt', '1998/09/01', 2);
INSERT INTO alumno VALUES (21, '79089577Y', 'Juan', 'Gutiérrez', 'López', '1998/01/01', 1);
INSERT INTO alumno VALUES (22, '41491230N', 'Antonio', 'Domínguez', 'Guerrero', '1999/02/11', 1);
INSERT INTO alumno VALUES (23, '64753215G', 'Irene', 'Hernández', 'Martínez', '1996/03/12', 2);
INSERT INTO alumno VALUES (24, '85135690V', 'Sonia', 'Gea', 'Ruiz', '1995/04/13', 2);

alumno_direccion

INSERT INTO alumno_direccion (id_alumno, id_direccion) VALUES
(1, 2), -- Juan Saez reside en la dirección Av. de la Constitución, 45, La Latina, 28005
(2, 2), -- Salvador Sánchez reside en la dirección Av. de la Constitución, 45, La Latina, 28005
(4, 2), -- Pedro Heller reside en la dirección Av. de la Constitución, 45, La Latina, 28005
(6, 2), -- José Koss reside en la dirección Av. de la Constitución, 45, La Latina, 28005
(7, 2), -- Ismael Strosin reside en la dirección Av. de la Constitución, 45, La Latina, 28005
(9, 2), -- Ramón Herzog reside en la dirección Av. de la Constitución, 45, La Latina, 28005
(11, 2), -- Daniel Herman reside en la dirección Av. de la Constitución, 45, La Latina, 28005
(19, 3), -- Inma Lakin reside en la dirección C/ Gran Vía, 123, Malasaña, 28004
(21, 2), -- Juan Gutiérrez reside en la dirección Av. de la Constitución, 45, La Latina, 28005
(22, 2), -- Antonio Domínguez reside en la dirección Av. de la Constitución, 45, La Latina, 28005
(23, 3), -- Irene Hernández reside en la dirección C/ Gran Vía, 123, Malasaña, 28004
(24, 3); -- Sonia Gea reside en la dirección C/ Gran Vía, 123, Malasaña, 28004

- alumno_telefono

INSERT INTO alumno_telefono (id_alumno, id_telefono) VALUES
(1, 1), -- Juan Saez tiene el teléfono móvil con número 123456789
(2, 2), -- Salvador Sánchez tiene el teléfono fijo con número 987654321
(4, 3), -- Pedro Heller tiene el teléfono de trabajo con número 246813579
(6, 4), -- José Koss tiene el teléfono de fax con número 555555555
(7, 5), -- Ismael Strosin tiene el teléfono móvil con número 999888777
(9, 6), -- Ramón Herzog tiene el teléfono móvil con número 666666666
(11, 7), -- Daniel Herman tiene el teléfono de trabajo con número 777777777
(19, 8), -- Inma Lakin tiene el teléfono de fax con número 333333333
(21, 9), -- Juan Gutiérrez tiene el teléfono móvil con número 123123123
(22, 10), -- Antonio Domínguez tiene el teléfono fijo con número 456456456
(23, 11), -- Irene Hernández tiene el teléfono de trabajo con número 789789789
(24, 12); -- Sonia Gea tiene el teléfono de fax con número 111111111

- alumno_se_matricula_asignatura

INSERT INTO alumno_se_matricula_asignatura VALUES (1, 1, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 2, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 3, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 4, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 5, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 6, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 7, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 8, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 9, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 10, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 1, 2);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 2, 2);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 3, 2);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 1, 3);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 2, 3);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 3, 3);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 1, 4);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 2, 4);
INSERT INTO alumno_se_matricula_asignatura VALUES (1, 3, 4);
INSERT INTO alumno_se_matricula_asignatura VALUES (2, 1, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (2, 2, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (2, 3, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 1, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 2, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 3, 1);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 1, 2);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 2, 2);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 3, 2);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 4, 2);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 5, 2);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 6, 2);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 7, 2);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 8, 2);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 9, 2);
INSERT INTO alumno_se_matricula_asignatura VALUES (4, 10, 2);
   ```

**Consultas sobre una tabla**

1. Devuelve un listado con el primer apellido, segundo apellido y el nombre de todos los alumnos. El listado deberá estar ordenado alfabéticamente de menor a mayor por el primer apellido, segundo apellido y nombre.

```mysql
SELECT nombre_alumno, apellido1_alumno, apellido2_alumno
FROM alumno 
ORDER BY apellido1_alumno, apellido2_alumno, nombre_alumno ASC;

+---------------+------------------+------------------+
| nombre_alumno | apellido1_alumno | apellido2_alumno |
+---------------+------------------+------------------+
| Antonio       | Domínguez        | Guerrero         |
| Sonia         | Gea              | Ruiz             |
| Juan          | Gutiérrez        | López            |
| Pedro         | Heller           | Pagac            |
| Daniel        | Herman           | Pacocha          |
| Irene         | Hernández        | Martínez         |
| Ramón         | Herzog           | Tremblay         |
| José          | Koss             | Bayer            |
| Inma          | Lakin            | Yundt            |
| Juan          | Saez             | Vega             |
| Salvador      | Sánchez          | Pérez            |
| Ismael        | Strosin          | Turcotte         |
+---------------+------------------+------------------+
12 rows in set (0.00 sec)
   ```

2. Averigua el nombre y los dos apellidos de los alumnos que no han dado de
alta su número de teléfono en la base de datos.

```mysql
SELECT nombre_alumno, apellido1_alumno, apellido2_alumno
FROM alumno
LEFT JOIN alumno_telefono ON alumno.id_alumno = alumno_telefono.id_alumno
WHERE alumno_telefono.id_alumno IS NULL;

Empty set (0.00 sec)
   ```

3. Devuelve el listado de los alumnos que nacieron en 1999.
   
```mysql
SELECT nombre_alumno
FROM alumno 
WHERE YEAR(fecha_nacimiento)  = 1999;

+---------------+
| nombre_alumno |
+---------------+
| Ismael        |
| Antonio       |
+---------------+
2 rows in set (0.00 sec)
   ```

4. Devuelve el listado de profesores que no han dado de alta su número de
teléfono en la base de datos y además su nif termina en K.

```mysql
SELECT p.nombre_profesor
FROM profesor as p
RIGHT JOIN profesor_telefono as pt ON pt.id_profesor = p.id_profesor
WHERE  pt.id_profesor IS NULL AND p.nif LIKE  '%K';

Empty set (0.00 sec)
   ```

5. Devuelve el listado de las asignaturas que se imparten en el primer
cuatrimestre, en el tercer curso del grado que tiene el identificador 7.

```mysql
SELECT a.nombre as Asignatura
FROM asignatura as a
WHERE cuatrimestre = 1
AND id_curso = 3
AND id_grado = 7;

+--------------------+
| Asignatura         |
+--------------------+
| Almacenes de Datos |
+--------------------+
1 row in set (0.00 sec)
   ```

**Consultas multitabla (Composición interna)**

1. Devuelve un listado con los datos de todas las alumnas que se han
matriculado alguna vez en el Grado en Ingeniería Informática (Plan 2015).

```mysql
SELECT a.nombre_alumno, a.nif, a.apellido1_alumno, a.apellido2_alumno, a.fecha_nacimiento, a.id_sexo as genero, g.id_grado
FROM alumno as a 
LEFT JOIN sexo as s ON s.id_sexo = a.id_sexo
LEFT JOIN   alumno_se_matricula_asignatura as ams ON ams.id_alumno = a.id_alumno
JOIN asignatura as asig ON asig.id_asignatura = ams.id_asignatura
JOIN grado as g ON g.id_grado = asig.id_grado
WHERE s.tipo = 'Femenino' 
AND g.nombre = 'Grado en Ingeniería Informática'
AND g.anyo = 2015;

+---------------+-----------+------------------+------------------+------------------+--------+----------+
| nombre_alumno | nif       | apellido1_alumno | apellido2_alumno | fecha_nacimiento | genero | id_grado |
+---------------+-----------+------------------+------------------+------------------+--------+----------+
| Inma          | 11578526G | Lakin            | Yundt            | 1998-09-01       |      2 |        4 |
+---------------+-----------+------------------+------------------+------------------+--------+----------+
1 row in set (0.00 sec)

   ```

2. Devuelve un listado con todas las asignaturas ofertadas en el Grado en
Ingeniería Informática (Plan 2015).

```mysql

SELECT a.nombre as nombre_de_la_asignatura
FROM asignatura as a
 JOIN grado as g ON g.id_grado = a.id_grado
WHERE g.nombre = 'Grado en Ingeniería Informática'
AND g.anyo = 2015;

+---------------------------------------------------------------------------+
| nombre_de_la_asignatura                                                   |
+---------------------------------------------------------------------------+
| Álgegra lineal y matemática discreta                                      |
| Cálculo                                                                   |
| Física para informática                                                   |
| Introducción a la programación                                            |
| Organización y gestión de empresas                                        |
| Estadística                                                               |
| Estructura y tecnología de computadores                                   |
| Fundamentos de electrónica                                                |
| Lógica y algorítmica                                                      |
| Metodología de la programación                                            |
| Arquitectura de Computadores                                              |
| Estructura de Datos y Algoritmos I                                        |
| Ingeniería del Software                                                   |
| Sistemas Inteligentes                                                     |
| Sistemas Operativos                                                       |
| Fundamentos de Redes de Computadores                                      |
| Planificación y Gestión de Proyectos Informáticos                         |
| Programación de Servicios Software                                        |
| Desarrollo de interfaces de usuario                                       |
| Ingeniería de Requisitos                                                  |
| Integración de las Tecnologías de la Información en las Organizaciones    |
| Modelado y Diseño del Software 1                                          |
| Multiprocesadores                                                         |
| Seguridad y cumplimiento normativo                                        |
| Sistema de Información para las Organizaciones                            |
| Tecnologías web                                                           |
| Teoría de códigos y criptografía                                          |
| Administración de bases de datos                                          |
| Herramientas y Métodos de Ingeniería del Software                         |
| Informática industrial y robótica                                         |
| Ingeniería de Sistemas de Información                                     |
| Modelado y Diseño del Software 2                                          |
| Negocio Electrónico                                                       |
| Periféricos e interfaces                                                  |
| Sistemas de tiempo real                                                   |
| Tecnologías de acceso a red                                               |
| Tratamiento digital de imágenes                                           |
| Administración de redes y sistemas operativos                             |
| Líneas de Productos Software                                              |
+---------------------------------------------------------------------------+
39 rows in set (0.00 sec)
   ```

3. Devuelve un listado de los profesores junto con el nombre del
departamento al que están vinculados. El listado debe devolver cuatro
columnas, primer apellido, segundo apellido, nombre y nombre del
departamento. El resultado estará ordenado alfabéticamente de menor a
mayor por los apellidos y el nombre.

```mysql
SELECT  p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor, d.nombre as departamento
FROM profesor as p
JOIN departamento as d ON d.id_departamento = p.id_departamento
ORDER BY p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor ASC;

+--------------------+--------------------+-----------------+---------------------+
| apellido1_profesor | apellido2_profesor | nombre_profesor | departamento        |
+--------------------+--------------------+-----------------+---------------------+
| Domínguez          | Hernández          | María           | Matemáticas         |
| Fahey              | Considine          | Antonio         | Economía y Empresa  |
| Guerrero           | Martínez           | Juan            | Informática         |
| Hamill             | Kozey              | Manolo          | Informática         |
| Kohler             | Schoen             | Alejandro       | Matemáticas         |
| Lemke              | Rutherford         | Cristina        | Economía y Empresa  |
| Monahan            | Murray             | Micaela         | Agronomía           |
| Ramirez            | Gea                | Zoe             | Informática         |
| Ruecker            | Upton              | Guillermo       | Educación           |
| Sánchez            | Ruiz               | Pepe            | Informática         |
| Schmidt            | Fisher             | David           | Matemáticas         |
| Schowalter         | Muller             | Francesca       | Química y Física    |
| Spencer            | Lakin              | Esther          | Educación           |
| Stiedemann         | Morissette         | Alfredo         | Química y Física    |
| Streich            | Hirthe             | Carmen          | Educación           |
+--------------------+--------------------+-----------------+---------------------+
15 rows in set (0.00 sec)
   ```

4. Devuelve un listado con el nombre de las asignaturas, año de inicio y año de fin del curso escolar del alumno con nif 26902806M.

```mysql
SELECT alum.nombre_alumno, a.nombre as asignatura, c.anyo_inicio, c.anyo_fin
FROM alumno as alum
JOIN alumno_se_matricula_asignatura as ama ON ama.id_alumno = alum.id_alumno
JOIN asignatura as a ON a.id_asignatura = ama.id_asignatura
JOIN curso_escolar as c ON c.id_curso = a.id_curso
WHERE alum.nif = '26902806M';

+---------------+----------------------------------------+-------------+----------+
| nombre_alumno | asignatura                             | anyo_inicio | anyo_fin |
+---------------+----------------------------------------+-------------+----------+
| Salvador      | Álgegra lineal y matemática discreta   |        2015 |     2016 |
| Salvador      | Cálculo                                |        2016 |     2017 |
| Salvador      | Física para informática                |        2015 |     2016 |
+---------------+----------------------------------------+-------------+----------+
3 rows in set (0.00 sec)
   ```

5. Devuelve un listado con el nombre de todos los departamentos que tienen
profesores que imparten alguna asignatura en el Grado en Ingeniería
Informática (Plan 2015).

```mysql
SELECT d.nombre as departamento
FROM profesor as p
JOIN departamento as d ON d.id_departamento = p.id_departamento
JOIN asignatura as a ON a.id_profesor = p.id_profesor
JOIN grado as g ON g.id_grado = a.id_grado
WHERE g.nombre = 'Grado en Ingeniería Informática' AND g.anyo = 2015;

+---------------------+
| departamento        |
+---------------------+
| Informática         |
| Matemáticas         |
| Economía y Empresa  |
| Educación           |
| Educación           |
| Química y Física    |
| Informática         |
| Matemáticas         |
| Economía y Empresa  |
| Agronomía           |
| Informática         |
| Educación           |
| Química y Física    |
| Informática         |
| Informática         |
| Matemáticas         |
| Economía y Empresa  |
| Informática         |
| Informática         |
| Informática         |
| Informática         |
| Química y Física    |
| Matemáticas         |
| Matemáticas         |
| Matemáticas         |
| Educación           |
| Matemáticas         |
| Economía y Empresa  |
| Educación           |
| Educación           |
| Química y Física    |
| Informática         |
| Informática         |
| Informática         |
| Matemáticas         |
| Educación           |
| Matemáticas         |
| Química y Física    |
| Educación           |
+---------------------+
39 rows in set (0.00 sec)
   ```

6. Devuelve un listado con todos los alumnos que se han matriculado en
alguna asignatura durante el curso escolar 2018/2019.

```mysql
SELECT DISTINCT a.nombre_alumno, a.nif, a.apellido1_alumno, a.apellido2_alumno
FROM alumno AS a
INNER JOIN alumno_se_matricula_asignatura AS ams ON a.id_alumno = ams.id_alumno
INNER JOIN asignatura AS asig ON ams.id_asignatura = asig.id_asignatura
INNER JOIN curso_escolar AS ce ON ams.id_curso = ce.id_curso
WHERE ce.anyo_inicio = 2018 AND ce.anyo_fin = 2019;

Empty set (0.01 sec)
   ```

**Consultas multitabla (Composición externa)**
Resuelva todas las consultas utilizando las cláusulas LEFT JOIN y RIGHT JOIN.

1. Devuelve un listado con los nombres de todos los profesores y los
departamentos que tienen vinculados. El listado también debe mostrar
aquellos profesores que no tienen ningún departamento asociado. El listado
debe devolver cuatro columnas, nombre del departamento, primer apellido,
segundo apellido y nombre del profesor. El resultado estará ordenado
alfabéticamente de menor a mayor por el nombre del departamento,
apellidos y el nombre.

```mysql
SELECT d.nombre AS nombre_departamento, p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor
FROM profesor AS p
LEFT JOIN departamento AS d ON p.id_departamento = d.id_departamento
ORDER BY IFNULL(d.nombre, ''), p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor;

+---------------------+--------------------+--------------------+-----------------+
| nombre_departamento | apellido1_profesor | apellido2_profesor | nombre_profesor |
+---------------------+--------------------+--------------------+-----------------+
| Agronomía           | Monahan            | Murray             | Micaela         |
| Economía y Empresa  | Fahey              | Considine          | Antonio         |
| Economía y Empresa  | Lemke              | Rutherford         | Cristina        |
| Educación           | Ruecker            | Upton              | Guillermo       |
| Educación           | Spencer            | Lakin              | Esther          |
| Educación           | Streich            | Hirthe             | Carmen          |
| Informática         | Guerrero           | Martínez           | Juan            |
| Informática         | Hamill             | Kozey              | Manolo          |
| Informática         | Ramirez            | Gea                | Zoe             |
| Informática         | Sánchez            | Ruiz               | Pepe            |
| Matemáticas         | Domínguez          | Hernández          | María           |
| Matemáticas         | Kohler             | Schoen             | Alejandro       |
| Matemáticas         | Schmidt            | Fisher             | David           |
| Química y Física    | Schowalter         | Muller             | Francesca       |
| Química y Física    | Stiedemann         | Morissette         | Alfredo         |
+---------------------+--------------------+--------------------+-----------------+
15 rows in set (0.00 sec)
   ```

2. Devuelve un listado con los profesores que no están asociados a un
departamento.

```mysql
SELECT p.nombre_profesor, p.apellido1_profesor, p.apellido2_profesor
FROM profesor AS p
LEFT JOIN departamento AS d ON p.id_departamento = d.id_departamento
WHERE d.id_departamento IS NULL;

Empty set (0.00 sec)
   ```

3. Devuelve un listado con los departamentos que no tienen profesores
asociados.

```mysql
SELECT d.nombre AS nombre_departamento
FROM departamento AS d
LEFT JOIN profesor AS p ON d.id_departamento = p.id_departamento
WHERE p.id_departamento IS NULL;

+-----------------------+
| nombre_departamento   |
+-----------------------+
| Filología             |
| Derecho               |
| Biología y Geología   |
+-----------------------+
3 rows in set (0.00 sec)
   ```

4. Devuelve un listado con los profesores que no imparten ninguna asignatura.

```mysql
SELECT p.nombre_profesor, p.apellido1_profesor, p.apellido2_profesor
FROM profesor AS p
LEFT JOIN asignatura AS a ON p.id_profesor = a.id_profesor
WHERE a.id_profesor IS NULL;

Empty set (0.00 sec)
   ```

5. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

```mysql
SELECT a.nombre AS nombre_asignatura
FROM asignatura AS a
LEFT JOIN profesor AS p ON a.id_profesor = p.id_profesor
WHERE p.id_profesor IS NULL;


Empty set (0.00 sec)
   ```

6. Devuelve un listado con todos los departamentos que tienen alguna
asignatura que no se haya impartido en ningún curso escolar. El resultado
debe mostrar el nombre del departamento y el nombre de la asignatura que
no se haya impartido nunca.

```mysql
SELECT d.nombre AS nombre_departamento, a.nombre AS nombre_asignatura
FROM departamento d
JOIN profesor p ON d.id_departamento = p.id_departamento
JOIN asignatura a ON p.id_profesor = a.id_profesor
LEFT JOIN alumno_se_matricula_asignatura am ON a.id_asignatura = am.id_asignatura
WHERE am.id_asignatura IS NULL;


+---------------------+---------------------------------------------------------------------------+
| nombre_departamento | nombre_asignatura                                                         |
+---------------------+---------------------------------------------------------------------------+
| Informática         | Arquitectura de Computadores                                              |
| Informática         | Programación de Servicios Software                                        |
| Informática         | Desarrollo de interfaces de usuario                                       |
| Informática         | Modelado y Diseño del Software 2                                          |
| Informática         | Almacenes de Datos                                                        |
| Informática         | Sistemas Operativos                                                       |
| Informática         | Ingeniería de Requisitos                                                  |
| Informática         | Periféricos e interfaces                                                  |
| Informática         | Sistemas Inteligentes                                                     |
| Informática         | Integración de las Tecnologías de la Información en las Organizaciones    |
| Informática         | Negocio Electrónico                                                       |
| Matemáticas         | Teoría de códigos y criptografía                                          |
| Matemáticas         | Fundamentos de Redes de Computadores                                      |
| Matemáticas         | Tratamiento digital de imágenes                                           |
| Matemáticas         | Multiprocesadores                                                         |
| Matemáticas         | Seguridad y cumplimiento normativo                                        |
| Matemáticas         | Sistema de Información para las Organizaciones                            |
| Matemáticas         | Sistemas de tiempo real                                                   |
| Economía y Empresa  | Administración de bases de datos                                          |
| Economía y Empresa  | Planificación y Gestión de Proyectos Informáticos                         |
| Educación           | Estructura de Datos y Algoritmos I                                        |
| Educación           | Herramientas y Métodos de Ingeniería del Software                         |
| Educación           | Tecnologías web                                                           |
| Educación           | Informática industrial y robótica                                         |
| Educación           | Tecnologías de acceso a red                                               |
| Química y Física    | Ingeniería de Sistemas de Información                                     |
| Química y Física    | Administración de redes y sistemas operativos                             |
| Química y Física    | Ingeniería del Software                                                   |
| Química y Física    | Modelado y Diseño del Software 1                                          |
+---------------------+---------------------------------------------------------------------------+
29 rows in set (0.00 sec)
   ```

**Consultas resumen**

1. Devuelve el número total de alumnas que hay.

```mysql
SELECT COUNT(*) AS total_alumnas
FROM alumno
WHERE id_sexo = (SELECT id_sexo FROM sexo WHERE tipo = 'Femenino');

+---------------+
| total_alumnas |
+---------------+
|             3 |
+---------------+
1 row in set (0.14 sec)
   ```

2. Calcula cuántos alumnos nacieron en 1999.

```mysql
SELECT COUNT(*) AS total_alumnos_nacidos_1999
FROM alumno
WHERE YEAR(fecha_nacimiento) = 1999;

+----------------------------+
| total_alumnos_nacidos_1999 |
+----------------------------+
|                          2 |
+----------------------------+
1 row in set (0.00 sec)
   ```

3. Calcula cuántos profesores hay en cada departamento. El resultado sólo
debe mostrar dos columnas, una con el nombre del departamento y otra
con el número de profesores que hay en ese departamento. El resultado
sólo debe incluir los departamentos que tienen profesores asociados y
deberá estar ordenado de mayor a menor por el número de profesores.

```mysql
SELECT d.nombre AS nombre_departamento, COUNT(p.id_profesor) AS cantidad_profesores
FROM departamento d
LEFT JOIN profesor p ON d.id_departamento = p.id_departamento
GROUP BY d.nombre
HAVING COUNT(p.id_profesor) > 0
ORDER BY cantidad_profesores DESC;


+---------------------+---------------------+
| nombre_departamento | cantidad_profesores |
+---------------------+---------------------+
| Informática         |                   4 |
| Matemáticas         |                   3 |
| Educación           |                   3 |
| Economía y Empresa  |                   2 |
| Química y Física    |                   2 |
| Agronomía           |                   1 |
+---------------------+---------------------+
6 rows in set (0.00 sec)
   ```

4. Devuelve un listado con todos los departamentos y el número de profesores
que hay en cada uno de ellos. Tenga en cuenta que pueden existir
departamentos que no tienen profesores asociados. Estos departamentos
también tienen que aparecer en el listado.

```mysql
SELECT d.nombre AS nombre_departamento, COUNT(p.id_profesor) AS cantidad_profesores
FROM departamento d
LEFT JOIN profesor p ON d.id_departamento = p.id_departamento
GROUP BY d.nombre;

+-----------------------+---------------------+
| nombre_departamento   | cantidad_profesores |
+-----------------------+---------------------+
| Informática           |                   4 |
| Matemáticas           |                   3 |
| Economía y Empresa    |                   2 |
| Educación             |                   3 |
| Agronomía             |                   1 |
| Química y Física      |                   2 |
| Filología             |                   0 |
| Derecho               |                   0 |
| Biología y Geología   |                   0 |
+-----------------------+---------------------+
9 rows in set (0.00 sec)
   ```

5. Devuelve un listado con el nombre de todos los grados existentes en la base
de datos y el número de asignaturas que tiene cada uno. Tenga en cuenta
que pueden existir grados que no tienen asignaturas asociadas. Estos grados también tienen que aparecer en el listado. El resultado deberá estar ordenado de mayor a menor por el número de asignaturas.

```mysql
SELECT g.nombre AS nombre_grado, COUNT(a.id_asignatura) AS cantidad_asignaturas
FROM grado g
LEFT JOIN asignatura a ON g.id_grado = a.id_grado
GROUP BY g.nombre
ORDER BY cantidad_asignaturas DESC;

+----------------------------------------------+----------------------+
| nombre_grado                                 | cantidad_asignaturas |
+----------------------------------------------+----------------------+
| Grado en Ingeniería Informática              |                   39 |
| Grado en Biotecnología                       |                    1 |
| Grado en Ingeniería Agrícola                 |                    0 |
| Grado en Ingeniería Eléctrica                |                    0 |
| Grado en Ingeniería Electrónica Industrial   |                    0 |
| Grado en Ingeniería Mecánica                 |                    0 |
| Grado en Ingeniería Química Industrial       |                    0 |
| Grado en Ciencias Ambientales                |                    0 |
| Grado en Matemáticas                         |                    0 |
| Grado en Química                             |                    0 |
+----------------------------------------------+----------------------+
10 rows in set (0.00 sec)
   ```

6. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno, de los grados que tengan más de 40 asignaturas asociadas.

```mysql
SELECT g.nombre AS nombre_grado, COUNT(a.id_asignatura) AS cantidad_asignaturas
FROM grado g
JOIN asignatura a ON g.id_grado = a.id_grado
GROUP BY g.nombre
HAVING COUNT(a.id_asignatura) > 40
ORDER BY cantidad_asignaturas DESC;

Empty set (0.00 sec)
   ```

7. Devuelve un listado que muestre el nombre de los grados y la suma del
número total de créditos que hay para cada tipo de asignatura. El resultado
debe tener tres columnas: nombre del grado, tipo de asignatura y la suma
de los créditos de todas las asignaturas que hay de ese tipo. Ordene el
resultado de mayor a menor por el número total de crédidos.

```mysql
SELECT g.nombre AS nombre_grado, ta.tipo_asignatura, SUM(a.creditos) AS suma_creditos
FROM grado g
JOIN asignatura a ON g.id_grado = a.id_grado
JOIN tipo_asignatura ta ON a.id_tipo_asignatura = ta.id_tipo
GROUP BY g.nombre, ta.tipo_asignatura
ORDER BY suma_creditos DESC;

+-----------------------------------+-----------------+---------------+
| nombre_grado                      | tipo_asignatura | suma_creditos |
+-----------------------------------+-----------------+---------------+
| Grado en Ingeniería Informática   | Básica          |            96 |
| Grado en Ingeniería Informática   | Optativa        |            78 |
| Grado en Ingeniería Informática   | Obligatoria     |            60 |
| Grado en Biotecnología            | Obligatoria     |             6 |
+-----------------------------------+-----------------+---------------+
4 rows in set (0.01 sec)
   ```

8. Devuelve un listado que muestre cuántos alumnos se han matriculado de
alguna asignatura en cada uno de los cursos escolares. El resultado deberá
mostrar dos columnas, una columna con el año de inicio del curso escolar y
otra con el número de alumnos matriculados.

```mysql
SELECT ce.anyo_inicio AS año_inicio_curso, COUNT(DISTINCT ama.id_alumno) AS numero_alumnos_matriculados
FROM curso_escolar ce
LEFT JOIN alumno_se_matricula_asignatura ama ON ce.id_curso = ama.id_curso
GROUP BY ce.anyo_inicio;


+-------------------+-----------------------------+
| año_inicio_curso  | numero_alumnos_matriculados |
+-------------------+-----------------------------+
|              2014 |                           3 |
|              2015 |                           3 |
|              2016 |                           1 |
|              2017 |                           1 |
+-------------------+-----------------------------+
4 rows in set (0.00 sec)

   ```

9. Devuelve un listado con el número de asignaturas que imparte cada
profesor. El listado debe tener en cuenta aquellos profesores que no
imparten ninguna asignatura. El resultado mostrará cinco columnas: id,
nombre, primer apellido, segundo apellido y número de asignaturas. El
resultado estará ordenado de mayor a menor por el número de asignaturas.

```mysql
SELECT 
    p.id_profesor AS id,
    p.nombre_profesor AS nombre,
    p.apellido1_profesor AS primer_apellido,
    p.apellido2_profesor AS segundo_apellido,
    COUNT(a.id_asignatura) AS numero_asignaturas
FROM profesor p
LEFT JOIN asignatura a ON p.id_profesor = a.id_profesor
GROUP BY p.id_profesor, p.nombre_profesor, p.apellido1_profesor, p.apellido2_profesor
ORDER BY numero_asignaturas DESC;

+----+-----------+-----------------+------------------+--------------------+
| id | nombre    | primer_apellido | segundo_apellido | numero_asignaturas |
+----+-----------+-----------------+------------------+--------------------+
| 14 | Manolo    | Hamill          | Kozey            |                  6 |
| 10 | Esther    | Spencer         | Lakin            |                  4 |
| 23 | María     | Domínguez       | Hernández        |                  4 |
| 12 | Carmen    | Streich         | Hirthe           |                  3 |
| 13 | Alfredo   | Stiedemann      | Morissette       |                  3 |
| 15 | Alejandro | Kohler          | Schoen           |                  3 |
| 21 | Pepe      | Sánchez         | Ruiz             |                  3 |
| 22 | Juan      | Guerrero        | Martínez         |                  3 |
|  5 | David     | Schmidt         | Fisher           |                  2 |
|  8 | Cristina  | Lemke           | Rutherford       |                  2 |
| 16 | Antonio   | Fahey           | Considine        |                  2 |
| 20 | Francesca | Schowalter      | Muller           |                  2 |
|  3 | Zoe       | Ramirez         | Gea              |                  1 |
| 17 | Guillermo | Ruecker         | Upton            |                  1 |
| 18 | Micaela   | Monahan         | Murray           |                  1 |
+----+-----------+-----------------+------------------+--------------------+
15 rows in set (0.00 sec)
   ```

**Subconsultas**

1. Devuelve todos los datos del alumno más joven.

```mysql

SELECT nif, nombre_alumno, apellido1_alumno, apellido2_alumno, fecha_nacimiento, id_sexo
FROM alumno
WHERE fecha_nacimiento = (SELECT MIN(fecha_nacimiento) FROM alumno);

+-----------+-----------+---------------+------------------+------------------+------------------+---------+
| id_alumno | nif       | nombre_alumno | apellido1_alumno | apellido2_alumno | fecha_nacimiento | id_sexo |
+-----------+-----------+---------------+------------------+------------------+------------------+---------+
|         2 | 26902806M | Salvador      | Sánchez          | Pérez            | 1991-03-28       |       1 |
+-----------+-----------+---------------+------------------+------------------+------------------+---------+
1 row in set (0.01 sec)
   ```

2. Devuelve un listado con los profesores que no están asociados a un
departamento.

```mysql
SELECT *
FROM profesor
WHERE id_departamento IS NULL;

Empty set (0.00 sec)
   ```

3. Devuelve un listado con los departamentos que no tienen profesores
asociados.

```mysql
SELECT *
FROM departamento
WHERE id_departamento NOT IN (SELECT id_departamento FROM profesor);

+-----------------+-----------------------+
| id_departamento | nombre                |
+-----------------+-----------------------+
|               7 | Filología             |
|               8 | Derecho               |
|               9 | Biología y Geología   |
+-----------------+-----------------------+
3 rows in set (0.00 sec)
   ```

4. Devuelve un listado con los profesores que tienen un departamento
asociado y que no imparten ninguna asignatura.

```mysql
SELECT nombre_profesor
FROM profesor
WHERE id_departamento IS NOT NULL
AND id_profesor NOT IN (SELECT id_profesor FROM asignatura);

Empty set (0.00 sec)
   ```

5. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

```mysql
SELECT nombre
FROM asignatura
WHERE id_profesor IS NULL;

Empty set (0.00 sec)
   ```

6. Devuelve un listado con todos los departamentos que no han impartido
asignaturas en ningún curso escolar.

```mysql
SELECT d.*
FROM departamento d
LEFT JOIN profesor p ON d.id_departamento = p.id_departamento
LEFT JOIN asignatura a ON p.id_profesor = a.id_profesor
GROUP BY d.id_departamento
HAVING COUNT(a.id_asignatura) = 0;

+-----------------+-----------------------+
| id_departamento | nombre                |
+-----------------+-----------------------+
|               7 | Filología             |
|               8 | Derecho               |
|               9 | Biología y Geología   |
+-----------------+-----------------------+
3 rows in set (0.00 sec)
   ```

- Procedimientos
  
1. Crear un nuevo país:
   
```mysql
CREATE PROCEDURE CrearPais(
    IN nombre_pais VARCHAR(100)
)
BEGIN
    INSERT INTO pais (nombre) VALUES (nombre_pais);
END;
   ```

2. Actualizar el nombre de un país:
   
```mysql
CREATE PROCEDURE ActualizarNombrePais(
    IN id_pais INT,
    IN nuevo_nombre VARCHAR(100)
)
BEGIN
    UPDATE pais SET nombre = nuevo_nombre WHERE id_pais = id_pais;
END;
   ```

3. Eliminar un país por ID:
   
```mysql
CREATE PROCEDURE EliminarPaisPorID(
    IN id_pais INT
)
BEGIN
    DELETE FROM pais WHERE id_pais = id_pais;
END;
   ```

4. Buscar país por nombre:
   
```mysql
CREATE PROCEDURE BuscarPaisPorNombre(
    IN nombre_pais VARCHAR(100)
)
BEGIN
    SELECT * FROM pais WHERE nombre = nombre_pais;
END;
   ```

5. Crear una nueva asignatura:
   
```mysql
CREATE PROCEDURE CrearAsignatura(
    IN nombre_asignatura VARCHAR(100),
    IN creditos FLOAT,
    IN cuatrimestre TINYINT(3),
    IN id_tipo_asignatura INT,
    IN id_curso INT,
    IN id_profesor INT,
    IN id_grado INT
)
BEGIN
    INSERT INTO asignatura (nombre, creditos, cuatrimestre, id_tipo_asignatura, id_curso, id_profesor, id_grado)
    VALUES (nombre_asignatura, creditos, cuatrimestre, id_tipo_asignatura, id_curso, id_profesor, id_grado);
END;
   ```

6. Actualizar el nombre de una asignatura por ID:
   
```mysql
CREATE PROCEDURE ActualizarNombreAsignatura(
    IN id_asignatura INT,
    IN nuevo_nombre VARCHAR(100)
)
BEGIN
    UPDATE asignatura SET nombre = nuevo_nombre WHERE id_asignatura = id_asignatura;
END;
   ```

7. Eliminar una asignatura por ID:
   
```mysql
CREATE PROCEDURE EliminarAsignaturaPorID(
    IN id_asignatura INT
)
BEGIN
    DELETE FROM asignatura WHERE id_asignatura = id_asignatura;
END;
   ```

8. Buscar asignaturas por ID de curso:
   
```mysql
CREATE PROCEDURE BuscarAsignaturasPorIDCurso(
    IN id_curso INT
)
BEGIN
    SELECT * FROM asignatura WHERE id_curso = id_curso;
END;
   ```

9. Actualizar el nombre de un profesor por ID:
    
```mysql
CREATE PROCEDURE ActualizarNombreProfesor(
    IN id_profesor INT,
    IN nuevo_nombre VARCHAR(100)
)
BEGIN
    UPDATE profesor SET nombre_profesor = nuevo_nombre WHERE id_profesor = id_profesor;
END;
   ```

10. Eliminar un profesor por ID:
    
```mysql
CREATE PROCEDURE EliminarProfesorPorID(
    IN id_profesor INT
)
BEGIN
    DELETE FROM profesor WHERE id_profesor = id_profesor;
END;
   ```    

- Vistas

1. Vista de información de todos los profesores:
   
```mysql
CREATE VIEW VistaProfesores AS
SELECT id_profesor, nif, nombre_profesor, apellido1_profesor, apellido2_profesor, fecha_nacimiento, id_sexo, id_departamento
FROM profesor;
   ```    

2. Vista de información de todas las asignaturas:

```mysql
CREATE VIEW VistaAsignaturas AS
SELECT id_asignatura, nombre, creditos, cuatrimestre, id_tipo_asignatura, id_curso, id_profesor, id_grado
FROM asignatura;
   ```

3. Vista de información de todos los países y sus regiones:
   
```mysql
CREATE VIEW VistaPaisesRegiones AS
SELECT p.nombre AS nombre_pais, r.nombre AS nombre_region
FROM pais p
LEFT JOIN region r ON p.id_pais = r.id_pais;
   ```

4. Vista de información de todos los alumnos matriculados en asignaturas:
   
```mysql
CREATE VIEW VistaAlumnosMatriculados AS
SELECT a.id_alumno, a.nif, a.nombre_alumno, a.apellido1_alumno, a.apellido2_alumno, a.fecha_nacimiento, a.id_sexo, COUNT(am.id_alumno) AS num_matriculas
FROM alumno a
LEFT JOIN alumno_se_matricula_asignatura am ON a.id_alumno = am.id_alumno
GROUP BY a.id_alumno;
   ```

5. Vista de información de todos los profesores y sus asignaturas impartidas:

```mysql
CREATE VIEW VistaProfesoresAsignaturas AS
SELECT p.id_profesor, p.nif, p.nombre_profesor, p.apellido1_profesor, p.apellido2_profesor, p.fecha_nacimiento, p.id_sexo, p.id_departamento, a.*
FROM profesor p
LEFT JOIN asignatura a ON p.id_profesor = a.id_profesor;
   ```

6. Vista de información de todos los cursos escolares y las asignaturas asociadas:
   
```mysql
CREATE VIEW VistaCursosAsignaturas AS
SELECT ce.id_curso, ce.anyo_inicio, ce.anyo_fin, a.*
FROM curso_escolar ce
LEFT JOIN asignatura a ON ce.id_curso = a.id_curso;
   ```

7.Vista de información de todos los teléfonos de los profesores:

```mysql
CREATE VIEW VistaTelefonosProfesores AS
SELECT p.id_profesor, t.id_telefono, t.numero, t.prefijo, t.tipo_de_telefono
FROM profesor p
LEFT JOIN profesor_telefono pt ON p.id_profesor = pt.id_profesor
LEFT JOIN telefono t ON pt.id_telefono = t.id_telefono;
   ```

8. Vista de información de todos los alumnos y sus direcciones:
   
```mysql
CREATE VIEW VistaAlumnosDirecciones AS
SELECT a.id_alumno, a.nif, a.nombre_alumno, a.apellido1_alumno, a.apellido2_alumno, a.fecha_nacimiento, a.id_sexo, d.*
FROM alumno a
LEFT JOIN alumno_direccion ad ON a.id_alumno = ad.id_alumno
LEFT JOIN direccion d ON ad.id_direccion = d.id_direccion;
   ```
 
9. Vista de información de todas las ciudades y sus direcciones asociadas:
    
```mysql
CREATE VIEW VistaCiudadesDirecciones AS
SELECT c.nombre AS nombre_ciudad, d.*
FROM ciudad c
LEFT JOIN direccion d ON c.id_ciudad = d.id_ciudad;
   ```

10. Vista de información de todas las asignaturas y sus profesores:
    
```mysql
CREATE VIEW VistaAsignaturasProfesores AS
SELECT a.id_asignatura, a.nombre, a.creditos, a.cuatrimestre, a.id_tipo_asignatura, a.id_curso, a.id_profesor, a.id_grado, p.*
FROM asignatura a
LEFT JOIN profesor p ON a.id_profesor = p.id_profesor;
   ```   
