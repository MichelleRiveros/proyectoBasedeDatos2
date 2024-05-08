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
