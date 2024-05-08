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

-- sexo
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

-- direccion
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


-- telefono
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

-- profesor

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

-- Asignatura

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


