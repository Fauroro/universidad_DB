# BASE DE DATOS UNIVERSIDAD 



### DER


![DER_universidad_DB](https://github.com/Fauroro/universidad_DB/blob/main/DER_universidad.png?raw=true)


### Creación de la DB y sus Tablas

```mysql
-- -----------------------------------------------------
-- Database universidad_DB
-- -----------------------------------------------------
CREATE DATABASE IF NOT EXISTS universidad_DB;
USE universidad_DB;

-- -----------------------------------------------------
-- Table departamento
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS departamento (
  id_departamento INT NOT NULL AUTO_INCREMENT,
  nombre_departamento VARCHAR(50) NOT NULL,
  PRIMARY KEY (id_departamento))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table curso_escolar
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS curso_escolar (
  id_curso_escolar INT NOT NULL AUTO_INCREMENT,
  anyo_inicio YEAR NOT NULL,
  anyo_fin YEAR NOT NULL,
  PRIMARY KEY (id_curso_escolar))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table tipo_asignatura
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS tipo_asignatura (
  id_tipo_asignatura INT NOT NULL AUTO_INCREMENT,
  nombre_tipo_asignatura VARCHAR(45) NOT NULL,
  PRIMARY KEY (id_tipo_asignatura))
ENGINE = InnoDB;



-- -----------------------------------------------------
-- Table grado
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS grado (
  id_grado INT NOT NULL AUTO_INCREMENT,
  nombre_grado VARCHAR(100) NOT NULL,
  PRIMARY KEY (id_grado))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table pais
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS pais (
  id_pais VARCHAR(5) NOT NULL,
  nombre_pais VARCHAR(45) NOT NULL,
  PRIMARY KEY (id_pais))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table region
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS region (
  id_region VARCHAR(5) NOT NULL,
  nombre_region VARCHAR(50) NOT NULL,
  id_pais VARCHAR(5) NOT NULL,
  PRIMARY KEY (id_region),
  CONSTRAINT FK_pais FOREIGN KEY (id_pais) REFERENCES pais (id_pais))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table ciudad
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS ciudad (
  id_ciudad VARCHAR(5) NOT NULL,
  nombre_ciudad VARCHAR(50) NOT NULL,
  id_region VARCHAR(5) NOT NULL,
  PRIMARY KEY (id_ciudad),  
  CONSTRAINT FK_Region FOREIGN KEY (id_region) REFERENCES region (id_region))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table tipo_direccion
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS tipo_direccion (
  id_tipo_dir SMALLINT NOT NULL AUTO_INCREMENT,
  nombre_tipo_dir VARCHAR(50) NOT NULL,
  PRIMARY KEY (id_tipo_dir))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table tipo_telefono
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS tipo_telefono (
  id_tipo_telefono INT NOT NULL AUTO_INCREMENT,
  nombre_tipo_telefono VARCHAR(50) NOT NULL,
  PRIMARY KEY (id_tipo_telefono))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table sexo
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS sexo (
  id_sexo VARCHAR(3) NOT NULL,
  nombre_sexo VARCHAR(45) NOT NULL,
  PRIMARY KEY (id_sexo))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table alumno
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS alumno (
  id_alumno INT NOT NULL AUTO_INCREMENT,
  nif_alumno VARCHAR(50) NOT NULL,
  nombre_alumno VARCHAR(45) NOT NULL,
  apellido1_alumno VARCHAR(45) NOT NULL,
  apellido2_alumno VARCHAR(45) NULL DEFAULT NULL,
  fecha_nacimiento_alumno DATE NOT NULL,
  id_sexo VARCHAR(45) NOT NULL,
  PRIMARY KEY (id_alumno),
  CONSTRAINT FK_sexo FOREIGN KEY (id_sexo) REFERENCES sexo (id_sexo))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table profesor
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS profesor (
  id_profesor INT NOT NULL AUTO_INCREMENT,
  nif_profesor VARCHAR(50) NOT NULL,
  nombre_profesor VARCHAR(45) NOT NULL,
  apellido1_profesor VARCHAR(45) NOT NULL,
  apellido2_profesor VARCHAR(45) NULL DEFAULT NULL,
  fecha_nacimiento_profesor DATE NOT NULL,
  id_sexo VARCHAR(45) NOT NULL,
  id_departamento INT NOT NULL,
  PRIMARY KEY (id_profesor),
  CONSTRAINT FK_departamento FOREIGN KEY (id_departamento) REFERENCES departamento (id_departamento),
  CONSTRAINT FK_sexo_profesor FOREIGN KEY (id_sexo) REFERENCES sexo (id_sexo))
ENGINE = InnoDB;



-- -----------------------------------------------------
-- Table direccion_alumno
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS direccion_alumno (
  id_direccion INT NOT NULL AUTO_INCREMENT,
  sector VARCHAR(50) NULL DEFAULT NULL,
  nombre_calle VARCHAR(50) NULL DEFAULT NULL,
  complemento VARCHAR(50) NULL DEFAULT NULL,
  id_ciudad VARCHAR(5) NOT NULL,
  id_alumno INT NOT NULL,
  id_tipo_dir SMALLINT NOT NULL,
  PRIMARY KEY (id_direccion),
  CONSTRAINT FK_ciudad_alumno FOREIGN KEY (id_ciudad) REFERENCES ciudad (id_ciudad),
  CONSTRAINT FK_dir_alumno FOREIGN KEY (id_alumno) REFERENCES alumno (id_alumno),
  CONSTRAINT FK_tipo_dir_alum FOREIGN KEY (id_tipo_dir) REFERENCES tipo_direccion (id_tipo_dir))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table direccion_profesor 
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS direccion_profesor (
  id_direccion INT NOT NULL AUTO_INCREMENT,
  sector VARCHAR(50) NULL DEFAULT NULL,
  nombre_calle VARCHAR(50) NULL DEFAULT NULL,
  complemento VARCHAR(50) NULL DEFAULT NULL,
  id_ciudad VARCHAR(5) NOT NULL,
  id_profesor INT NOT NULL,
  id_tipo_dir SMALLINT NOT NULL,
  PRIMARY KEY (id_direccion),
  CONSTRAINT FK_ciudad_profesor FOREIGN KEY (id_ciudad) REFERENCES ciudad (id_ciudad),
  CONSTRAINT FK_dir_profesor FOREIGN KEY (id_profesor) REFERENCES profesor (id_profesor),
  CONSTRAINT FK_tipo_dir_prof FOREIGN KEY (id_tipo_dir) REFERENCES tipo_direccion (id_tipo_dir))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table telefono_profesor
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS telefono_profesor (
  id_telefono_profesor INT NOT NULL AUTO_INCREMENT,
  telefono VARCHAR(50) NOT NULL,
  id_tipo_telefono INT NULL DEFAULT NULL,
  id_profesor INT NOT NULL,
  PRIMARY KEY (id_telefono_profesor),
  CONSTRAINT FK_tel_profesor FOREIGN KEY (id_profesor) REFERENCES profesor (id_profesor),
  CONSTRAINT FK_tipo_tel_prof FOREIGN KEY (id_tipo_telefono) REFERENCES tipo_telefono (id_tipo_telefono))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table telefono_alumno
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS telefono_alumno (
  id_telefono_alumno INT NOT NULL AUTO_INCREMENT,
  telefono VARCHAR(50) NOT NULL,
  id_tipo_telefono INT NULL DEFAULT NULL,
  id_alumno INT NOT NULL,
  PRIMARY KEY (id_telefono_alumno),
  CONSTRAINT FK_tel_alumno FOREIGN KEY (id_alumno) REFERENCES alumno (id_alumno),
  CONSTRAINT FK_tipo_tel_alum FOREIGN KEY (id_tipo_telefono) REFERENCES tipo_telefono (id_tipo_telefono))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table asignatura
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS asignatura (
  id_asignatura INT NOT NULL,
  nombre_asignatura VARCHAR(100) NOT NULL,
  creditos FLOAT NOT NULL,
  curso TINYINT NOT NULL,
  cuatrimestre TINYINT NOT NULL,
  id_profesor INT NULL DEFAULT NULL,
  id_grado INT NOT NULL,
  id_tipo_asignatura INT NOT NULL,
  PRIMARY KEY (id_asignatura),
  CONSTRAINT FK_grado FOREIGN KEY (id_grado) REFERENCES grado (id_grado),
  CONSTRAINT FK_profesor FOREIGN KEY (id_profesor) REFERENCES profesor (id_profesor),
  CONSTRAINT FK_tipo_asignatura FOREIGN KEY (id_tipo_asignatura) REFERENCES tipo_asignatura (id_tipo_asignatura))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table alumno_matricula_asignatura
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS alumno_matricula_asignatura (
  id_alumno INT NOT NULL,
  id_asignatura INT NOT NULL,
  id_curso_escolar INT NOT NULL,
  PRIMARY KEY (id_alumno,id_asignatura,id_curso_escolar),
  CONSTRAINT FK_alumno FOREIGN KEY (id_alumno) REFERENCES alumno (id_alumno),
  CONSTRAINT FK_asignatura FOREIGN KEY (id_asignatura) REFERENCES asignatura (id_asignatura),
  CONSTRAINT FK_curso_escolar FOREIGN KEY (id_curso_escolar) REFERENCES curso_escolar (id_curso_escolar))
ENGINE = InnoDB;

```



### Inserción de datos

```mysql
-- -----------------------------------------------------
-- INSERT DEPARTAMENTO
-- ----------------------------------------------------

INSERT INTO departamento VALUES (1, 'Informática');
INSERT INTO departamento VALUES (2, 'Matemáticas');
INSERT INTO departamento VALUES (3, 'Economía y Empresa');
INSERT INTO departamento VALUES (4, 'Educación');
INSERT INTO departamento VALUES (5, 'Agronomía');
INSERT INTO departamento VALUES (6, 'Química y Física');
INSERT INTO departamento VALUES (7, 'Filología');
INSERT INTO departamento VALUES (8, 'Derecho');
INSERT INTO departamento VALUES (9, 'Biología y Geología');


-- -----------------------------------------------------
-- INSERT CURSO ESCOLAR
-- ----------------------------------------------------

INSERT INTO curso_escolar VALUES (1, 2014, 2015);
INSERT INTO curso_escolar VALUES (2, 2015, 2016);
INSERT INTO curso_escolar VALUES (3, 2016, 2017);
INSERT INTO curso_escolar VALUES (4, 2017, 2018);


-- -----------------------------------------------------
-- INSERT TIPO ASIGNATURA
-- ----------------------------------------------------

INSERT INTO tipo_asignatura values (1, 'Basica');
INSERT INTO tipo_asignatura values (2, 'Obligatoria');
INSERT INTO tipo_asignatura values (3, 'Optativa');


-- -----------------------------------------------------
-- INSERT GRADO
-- ----------------------------------------------------

INSERT INTO grado VALUES (1, 'Grado en Ingeniería Agrícola (Plan 2015)');
INSERT INTO grado VALUES (2, 'Grado en Ingeniería Eléctrica (Plan 2014)');
INSERT INTO grado VALUES (3, 'Grado en Ingeniería Electrónica Industrial (Plan 2010)');
INSERT INTO grado VALUES (4, 'Grado en Ingeniería Informática (Plan 2015)');
INSERT INTO grado VALUES (5, 'Grado en Ingeniería Mecánica (Plan 2010)');
INSERT INTO grado VALUES (6, 'Grado en Ingeniería Química Industrial (Plan 2010)');
INSERT INTO grado VALUES (7, 'Grado en Biotecnología (Plan 2015)');
INSERT INTO grado VALUES (8, 'Grado en Ciencias Ambientales (Plan 2009)');
INSERT INTO grado VALUES (9, 'Grado en Matemáticas (Plan 2010)');
INSERT INTO grado VALUES (10, 'Grado en Química (Plan 2009)');


-- -----------------------------------------------------
-- INSERT PAIS
-- ----------------------------------------------------
INSERT INTO pais (id_pais, nombre_pais) VALUES ('ESP', 'España');


-- -----------------------------------------------------
-- INSERT REGION
-- ----------------------------------------------------
INSERT INTO region (id_region, nombre_region, id_pais) VALUES ('AND', 'Andalucía', 'ESP');
INSERT INTO region (id_region, nombre_region, id_pais) VALUES ('MAD', 'Comunidad de Madrid', 'ESP');
INSERT INTO region (id_region, nombre_region, id_pais) VALUES ('CAT', 'Cataluña', 'ESP');
INSERT INTO region (id_region, nombre_region, id_pais) VALUES ('VAL', 'Comunidad Valenciana', 'ESP');


-- -----------------------------------------------------
-- INSERT CIUDAD
-- ----------------------------------------------------

INSERT INTO ciudad VALUES ('ALM', 'Almería', 'AND');
INSERT INTO ciudad VALUES ('MAD', 'Madrid', 'MAD');
INSERT INTO ciudad VALUES ('BCN', 'Barcelona', 'CAT');
INSERT INTO ciudad VALUES ('VAL', 'Valencia', 'VAL');
INSERT INTO ciudad VALUES ('SEV', 'Sevilla', 'AND');



-- -----------------------------------------------------
-- INSERT TIPO DIRECCION
-- ----------------------------------------------------
INSERT INTO tipo_direccion VALUES (NULL, 'Residencia');
INSERT INTO tipo_direccion VALUES (NULL, 'Origen');


-- -----------------------------------------------------
-- INSERT TIPO TELEFONO
-- ----------------------------------------------------

 INSERT INTO tipo_telefono VALUES (1, 'Fijo');
 INSERT INTO tipo_telefono VALUES (2, 'Celular');


-- -----------------------------------------------------
-- INSERT SEXO
-- ----------------------------------------------------

INSERT INTO sexo VALUES (1, 'H');
INSERT INTO sexo VALUES (2, 'M');


-- -----------------------------------------------------
-- INSERT ALUMNO
-- ----------------------------------------------------

INSERT INTO alumno VALUES (1, '89542419S', 'Juan', 'Saez', 'Vega','1992-08-08', 1);
INSERT INTO alumno VALUES (2, '26902806M', 'Salvador', 'Sánchez', 'Pérez','1991-03-28', 1);
INSERT INTO alumno VALUES (4, '17105885A', 'Pedro', 'Heller', 'Pagac','2000-10-05', 1);
INSERT INTO alumno VALUES (6, '04233869Y', 'José', 'Koss', 'Bayer', '1998-01-28', 1);
INSERT INTO alumno VALUES (7, '97258166K', 'Ismael', 'Strosin', 'Turcotte', '1999-05-24', 1);
INSERT INTO alumno VALUES (9, '82842571K', 'Ramón', 'Herzog', 'Tremblay', '1996-11-21', 1);
INSERT INTO alumno VALUES (11, '46900725E', 'Daniel', 'Herman', 'Pacocha','1997-04-26', 1);
INSERT INTO alumno VALUES (19, '11578526G', 'Inma', 'Lakin', 'Yundt','1998-09-01', 2);
INSERT INTO alumno VALUES (21, '79089577Y', 'Juan', 'Gutiérrez', 'López','1998-01-01', 1);
INSERT INTO alumno VALUES (22, '41491230N', 'Antonio', 'Domínguez', 'Guerrero','1999-02-11', 1);
INSERT INTO alumno VALUES (23, '64753215G', 'Irene', 'Hernández', 'Martínez', '1996-03-12', 2) ;
INSERT INTO alumno VALUES (24, '85135690V', 'Sonia', 'Gea', 'Ruiz','1995-04-13', 2);


-- -----------------------------------------------------
-- INSERT PROFESOR
-- ----------------------------------------------------

INSERT INTO profesor VALUES (3, '11105554G', 'Zoe', 'Ramirez', 'Gea','1979-08-19', 2, 1);
INSERT INTO profesor VALUES (5, '38223286T', 'David', 'Schmidt', 'Fisher', '1978-01-19', 1, 5);
INSERT INTO profesor VALUES (8, '79503962T', 'Cristina', 'Lemke', 'Rutherford', '1977-08-21', 2,3);
INSERT INTO profesor VALUES (10, '61142000L', 'Esther', 'Spencer', 'Lakin','1977-05-19', 2, 9);
INSERT INTO profesor VALUES (12, '85366986W', 'Carmen', 'Streich', 'Hirthe','1971-04-29', 2, 1);
INSERT INTO profesor VALUES (13, '73571384L', 'Alfredo', 'Stiedemann', 'Morissette','1980-02-01', 1, 7);
INSERT INTO profesor VALUES (14, '82937751G', 'Manolo', 'Hamill', 'Kozey','1977-01-02', 1, 9);
INSERT INTO profesor VALUES (15, '80502866Z', 'Alejandro', 'Kohler', 'Schoen','1980-03-14', 1, 2);
INSERT INTO profesor VALUES (16, '10485008K', 'Antonio', 'Fahey', 'Considine', '1982-03-18', 1, 4);
INSERT INTO profesor VALUES (17, '85869555K', 'Guillermo', 'Ruecker', 'Upton', '1973-05-05', 1, 6);
INSERT INTO profesor VALUES (18, '04326833G', 'Micaela', 'Monahan', 'Murray', '1976-02-25', 2, 7);
INSERT INTO profesor VALUES (20, '79221403L', 'Francesca', 'Schowalter', 'Muller','1980-10-31', 2, 8);
INSERT INTO profesor VALUES (21, '13175769N', 'Pepe', 'Sánchez', 'Ruiz','1980-10-16', 1, 6);
INSERT INTO profesor VALUES (22, '98816696W', 'Juan', 'Guerrero', 'Martínez','1980-11-21', 1, 2);
INSERT INTO profesor VALUES (23, '77194445M', 'María', 'Domínguez', 'Hernández','1980-12-13', 1, 1);



-- -----------------------------------------------------
-- INSERT DIRECCION ALUMNOS
-- ----------------------------------------------------

INSERT INTO direccion_alumno VALUES (01, 'Oeste', 'La Rambla', 'Carrera 4', 'BCN', 7, 1);
INSERT INTO direccion_alumno VALUES (02, 'Sur', 'Calle Gran Vía', 'Carrera 2', 'MAD', 9, 2);
INSERT INTO direccion_alumno VALUES (03, 'Este', 'Calle Marqués de Sotelo', 'Carrera 3', 'VAL', 1, 1);
INSERT INTO direccion_alumno VALUES (04, 'Este', 'Calle Reyes Católicos', 'Carrera 3', 'ALM', 11, 2);
INSERT INTO direccion_alumno VALUES (05, 'Centro', 'Plaça Catalunya', 'Carrera 5', 'BCN', 19, 1);
INSERT INTO direccion_alumno VALUES (06, 'Sur', 'Passeig de Gràcia', 'Carrera 2', 'BCN', 2, 2);
INSERT INTO direccion_alumno VALUES (07, 'Centro', 'Plaza Nueva', 'Carrera 5', 'SEV', 21, 1);
INSERT INTO direccion_alumno VALUES (08, 'Centro', 'Plaça de l Ajuntament', 'Carrera 5', 'VAL', 22, 2);
INSERT INTO direccion_alumno VALUES (09, 'Centro', 'Calle Preciados', 'Carrera 5', 'MAD', 4, 1);
INSERT INTO direccion_alumno VALUES (10, 'Centro', 'Plaça de l Ajuntament', 'Carrera 5', 'VAL', 23, 2);
INSERT INTO direccion_alumno VALUES (11, 'Centro', 'Paseo de Almería', 'Carrera 5', 'ALM', 24, 1);
INSERT INTO direccion_alumno VALUES (12, 'Norte', 'Calle Sierpes', 'Carrera 1', 'SEV', 6, 2);



-- -----------------------------------------------------
-- INSERT DIRECCION PROFESORES
-- ----------------------------------------------------

INSERT INTO direccion_profesor VALUES (01, 'Norte', 'Calle Mayor', 'Carrera 1', 'MAD', 3, 1);
INSERT INTO direccion_profesor VALUES (02, 'Este', 'Calle Alcalá', 'Carrera 3', 'MAD', 5, 2);
INSERT INTO direccion_profesor VALUES (03, 'Oeste', 'Calle Princesa', 'Carrera 4', 'MAD', 8, 1);
INSERT INTO direccion_profesor VALUES (04, 'Norte', 'Rambla Catalunya', 'Carrera 1', 'BCN', 10, 2);
INSERT INTO direccion_profesor VALUES (05, 'Este', 'Avinguda Diagonal', 'Carrera 3', 'BCN', 12, 1);
INSERT INTO direccion_profesor VALUES (06, 'Norte', 'Calle Colón', 'Carrera 1', 'VAL', 21, 2);
INSERT INTO direccion_profesor VALUES (07, 'Sur', 'Calle Xátiva', 'Carrera 2', 'VAL', 18, 1);
INSERT INTO direccion_profesor VALUES (08, 'Oeste', 'Calle San Vicente', 'Carrera 4', 'VAL', 13, 2);
INSERT INTO direccion_profesor VALUES (09, 'Centro', 'Plaça de l Ajuntament', 'Carrera 5', 'VAL', 23, 1);
INSERT INTO direccion_profesor VALUES (10, 'Sur', 'Calle Tetuán', 'Carrera 2', 'SEV', 20, 2);
INSERT INTO direccion_profesor VALUES (11, 'Oeste', 'Calle Alfonso XII', 'Carrera 4', 'SEV', 14, 1);
INSERT INTO direccion_profesor VALUES (12, 'Norte', 'Calle Real', 'Carrera 1', 'ALM', 22, 2);
INSERT INTO direccion_profesor VALUES (13, 'Sur', 'Calle Granada', 'Carrera 2', 'ALM', 17, 1);
INSERT INTO direccion_profesor VALUES (14, 'Oeste', 'Calle Alfonso XIII', 'Carrera 4', 'ALM', 15, 2);
INSERT INTO direccion_profesor VALUES (15, 'Nordeste', 'Calle del Sol', 'Carrera 6', 'MAD', 16, 1);



-- -----------------------------------------------------
-- INSERT TELEFONO PROFESORES  
-- ----------------------------------------------------
INSERT INTO telefono_profesor VALUES (NULL, 1234567890, 1, 3),
    (NULL, 611223344, 2, 5),
    (NULL, 689998877, 1, 8),
    (NULL, 938877665, 1, 10),
    (NULL, 611223344, 2, 12),
    (NULL, 677889900, 1, 13); 


-- -----------------------------------------------------
-- INSERT TELEFONO ALUMNOS
-- ----------------------------------------------------

INSERT INTO telefono_alumno VALUES (NULL, 938877665, 1, 1),
    (NULL, 912345678, 2, 2),
    (NULL, 934567890, 1, 4),
    (NULL, 689123456, 1, 6),
    (NULL, 678901234, 2, 7),
    (NULL, 666778899, 1, 9);


-- -----------------------------------------------------
-- INSERT ASIGNATURA
-- ----------------------------------------------------

INSERT INTO asignatura VALUES (1, 'Álgegra lineal y matemática discreta', 6, 1, 1, 5, 4, 1);
INSERT INTO asignatura VALUES (2, 'Cálculo', 6, 1, 1, 23, 4, 1); 
INSERT INTO asignatura VALUES (3, 'Física para informática', 6, 1, 1, 8, 1, 1);
INSERT INTO asignatura VALUES (4, 'Introducción a la programación', 6, 1, 1, 21, 3, 1);
INSERT INTO asignatura VALUES (5, 'Organización y gestión de empresas', 6, 1, 1, 20, 4, 1);
INSERT INTO asignatura VALUES (6, 'Estadística', 6, 1, 2, 17, 2, 1);
INSERT INTO asignatura VALUES (7, 'Estructura y tecnología de computadores', 6, 1, 2, 5, 10, 1);
INSERT INTO asignatura VALUES (8, 'Fundamentos de electrónica', 6, 1, 2, 3, 10, 1);
INSERT INTO asignatura VALUES (9, 'Lógica y algorítmica', 6, 1, 2, 5, 8, 1);
INSERT INTO asignatura VALUES (10, 'Metodología de la programación', 6, 1, 2, 8, 9, 1);
INSERT INTO asignatura VALUES (11, 'Arquitectura de Computadores', 6, 2, 1, 3, 7, 1);
INSERT INTO asignatura VALUES (12, 'Estructura de Datos y Algoritmos I', 6, 2, 1, 3, 5, 2);
INSERT INTO asignatura VALUES (13, 'Ingeniería del Software', 6, 2, 1, 14, 5, 2);
INSERT INTO asignatura VALUES (14, 'Sistemas Inteligentes', 6, 2, 1, 3, 9, 2);
INSERT INTO asignatura VALUES (15, 'Sistemas Operativos', 6, 2, 1, 14, 10, 2);
INSERT INTO asignatura VALUES (16, 'Bases de Datos', 6, 2, 2, 14, 6, 1);
INSERT INTO asignatura VALUES (17, 'Estructura de Datos y Algoritmos II', 6, 2, 2, 14, 4, 2);
INSERT INTO asignatura VALUES (18, 'Fundamentos de Redes de Computadores', 6 ,2, 2, 3, 5, 2);
INSERT INTO asignatura VALUES (19, 'Planificación y Gestión de Proyectos Informáticos', 6, 2, 2, 3, 5, 2);
INSERT INTO asignatura VALUES (20, 'Programación de Servicios Software', 6, 2, 2, 14, 9, 2);
INSERT INTO asignatura VALUES (21, 'Desarrollo de interfaces de usuario', 6, 3, 1, 14, 8, 2);
INSERT INTO asignatura VALUES (22, 'Ingeniería de Requisitos', 6, 3, 1, 5, 2, 3);
INSERT INTO asignatura VALUES (23, 'Integración de las Tecnologías de la Información en las Organizaciones', 6, 3, 1, 8, 1, 3);
INSERT INTO asignatura VALUES (24, 'Modelado y Diseño del Software 1', 6, 3, 1, 17, 1, 3);
INSERT INTO asignatura VALUES (25, 'Multiprocesadores', 6, 3, 1, 18, 2, 3);
INSERT INTO asignatura VALUES (26, 'Seguridad y cumplimiento normativo', 6, 3, 1, 22, 2, 3);
INSERT INTO asignatura VALUES (27, 'Sistema de Información para las Organizaciones', 6, 3, 1, 20, 5, 3); 
INSERT INTO asignatura VALUES (28, 'Tecnologías web', 6, 3, 1, 10, 2, 3);
INSERT INTO asignatura VALUES (29, 'Teoría de códigos y criptografía', 6, 3, 1, 12, 1, 3);
INSERT INTO asignatura VALUES (30, 'Administración de bases de datos', 6, 3, 2, 8, 2, 3);
INSERT INTO asignatura VALUES (31, 'Herramientas y Métodos de Ingeniería del Software', 6, 3, 2, 13, 5, 3);
INSERT INTO asignatura VALUES (32, 'Informática industrial y robótica', 6, 3, 2, 21, 1, 3);
INSERT INTO asignatura VALUES (33, 'Ingeniería de Sistemas de Información', 6, 3, 2, 15, 2, 3);
INSERT INTO asignatura VALUES (34, 'Modelado y Diseño del Software 2', 6, 3, 2, 18, 10, 3);
INSERT INTO asignatura VALUES (35, 'Negocio Electrónico', 6, 3, 2, 10, 10, 3);
INSERT INTO asignatura VALUES (36, 'Periféricos e interfaces', 6, 3, 2, 14, 10, 3);
INSERT INTO asignatura VALUES (37, 'Sistemas de tiempo real', 6, 3, 2, 10, 4, 3);
INSERT INTO asignatura VALUES (38, 'Tecnologías de acceso a red', 6, 3, 2, 12, 7, 3);
INSERT INTO asignatura VALUES (39, 'Tratamiento digital de imágenes', 6, 3, 2, 15, 3, 3);
INSERT INTO asignatura VALUES (40, 'Administración de redes y sistemas operativos', 6, 4, 1, 16, 3, 3);
INSERT INTO asignatura VALUES (41, 'Almacenes de Datos', 6, 4, 1, 10, 9, 3);
INSERT INTO asignatura VALUES (42, 'Fiabilidad y Gestión de Riesgos', 6, 4, 1, 8, 7, 3);
INSERT INTO asignatura VALUES (43, 'Líneas de Productos Software', 6, 4, 1, 17, 8, 3);
INSERT INTO asignatura VALUES (44, 'Procesos de Ingeniería del Software 1', 6, 4, 1, 18, 4, 3);
INSERT INTO asignatura VALUES (45, 'Tecnologías multimedia', 6, 4, 1, 8, 3, 3);
INSERT INTO asignatura VALUES (46, 'Análisis y planificación de las TI', 6, 4, 2, 21, 10, 3);
INSERT INTO asignatura VALUES (47, 'Desarrollo Rápido de Aplicaciones', 6, 4, 2, 23, 5, 3);
INSERT INTO asignatura VALUES (48, 'Gestión de la Calidad y de la Innovación Tecnológica', 6, 4, 2, 22, 6, 3);
INSERT INTO asignatura VALUES (49, 'Inteligencia del Negocio', 6, 4, 2, 3, 4, 1);
INSERT INTO asignatura VALUES (50, 'Procesos de Ingeniería del Software 2', 6, 4, 2, 3, 9, 3);
INSERT INTO asignatura VALUES (51, 'Seguridad Informática', 6, 4, 2, 22, 5, 3);
INSERT INTO asignatura VALUES (52, 'Biologia celular', 6, 1, 1, 12, 7, 1);
INSERT INTO asignatura VALUES (53, 'Física', 6, 1, 1, 21, 6, 1);
INSERT INTO asignatura VALUES (54, 'Matemáticas I', 6, 1, 1, 20, 10, 1);
INSERT INTO asignatura VALUES (55, 'Química general', 6, 1, 1, 14, 5, 1);
INSERT INTO asignatura VALUES (56, 'Química orgánica', 6, 1, 1, 16, 8, 1);
INSERT INTO asignatura VALUES (57, 'Biología vegetal y animal', 6, 1, 2, 23, 7, 1);
INSERT INTO asignatura VALUES (58, 'Bioquímica', 6, 1, 2, 18, 7, 1);
INSERT INTO asignatura VALUES (59, 'Genética', 6, 1, 2, 20, 9, 1);
INSERT INTO asignatura VALUES (60, 'Matemáticas II', 6, 1, 2, 23, 1, 1);
INSERT INTO asignatura VALUES (61, 'Microbiología', 6, 1, 2, 22, 2, 1);
INSERT INTO asignatura VALUES (62, 'Botánica agrícola', 6, 2, 1, 8, 3, 2);
INSERT INTO asignatura VALUES (63, 'Fisiología vegetal', 6, 2, 1, 12, 1, 2);
INSERT INTO asignatura VALUES (64, 'Genética molecular', 6, 2, 1, 15, 6, 2);
INSERT INTO asignatura VALUES (65, 'Ingeniería bioquímica', 6, 2, 1, 20, 5, 2);
INSERT INTO asignatura VALUES (66, 'Termodinámica y cinética química aplicada', 6, 2, 1, 10, 1, 2);
INSERT INTO asignatura VALUES (67, 'Biorreactores', 6, 2, 2, 12, 2, 2);
INSERT INTO asignatura VALUES (68, 'Biotecnología microbiana', 6, 2, 2, 22, 1, 2);
INSERT INTO asignatura VALUES (69, 'Ingeniería genética', 6, 2, 2, 23, 1, 2);
INSERT INTO asignatura VALUES (70, 'Inmunología', 6, 2, 2, 10, 9, 2);
INSERT INTO asignatura VALUES (71, 'Virología', 6, 2, 2, 8, 2, 2);
INSERT INTO asignatura VALUES (72, 'Bases moleculares del desarrollo vegetal', 4.5, 3, 1, 5, 5, 2);
INSERT INTO asignatura VALUES (73, 'Fisiología animal', 4.5, 3, 1, 3, 7, 2);
INSERT INTO asignatura VALUES (74, 'Metabolismo y biosíntesis de biomoléculas', 6, 3, 1, 5, 6, 2);
INSERT INTO asignatura VALUES (75, 'Operaciones de separación', 6, 3, 1, 3, 7, 2);
INSERT INTO asignatura VALUES (76, 'Patología molecular de plantas', 4.5, 3, 1, 5, 3, 2);
INSERT INTO asignatura VALUES (77, 'Técnicas instrumentales básicas', 4.5, 3, 1, 20, 6, 2);
INSERT INTO asignatura VALUES (78, 'Bioinformática', 4.5, 3, 2, 14, 10, 2);
INSERT INTO asignatura VALUES (79, 'Biotecnología de los productos hortofrutículas', 4.5, 3, 2, 8, 10, 2);
INSERT INTO asignatura VALUES (80, 'Biotecnología vegetal', 6, 3, 2, 22, 1, 2);
INSERT INTO asignatura VALUES (81, 'Genómica y proteómica', 4.5, 3, 2, 23, 5, 2);
INSERT INTO asignatura VALUES (82, 'Procesos biotecnológicos', 6, 3, 2, 12, 7, 2);
INSERT INTO asignatura VALUES (83, 'Técnicas instrumentales avanzadas', 4.5, 3, 2, 10, 9, 2);


-- -----------------------------------------------------
-- INSERT ALUMNO ASIGNATURA
-- ----------------------------------------------------

INSERT INTO alumno_matricula_asignatura VALUES (1, 1, 1);
INSERT INTO alumno_matricula_asignatura VALUES (1, 2, 1);
INSERT INTO alumno_matricula_asignatura VALUES (1, 3, 1);
INSERT INTO alumno_matricula_asignatura VALUES (1, 4, 1);
INSERT INTO alumno_matricula_asignatura VALUES (1, 5, 1);
INSERT INTO alumno_matricula_asignatura VALUES (1, 6, 1);
INSERT INTO alumno_matricula_asignatura VALUES (1, 7, 1);
INSERT INTO alumno_matricula_asignatura VALUES (1, 8, 1);
INSERT INTO alumno_matricula_asignatura VALUES (1, 9, 1);
INSERT INTO alumno_matricula_asignatura VALUES (1, 10, 1);
INSERT INTO alumno_matricula_asignatura VALUES (1, 1, 2);
INSERT INTO alumno_matricula_asignatura VALUES (1, 2, 2);
INSERT INTO alumno_matricula_asignatura VALUES (1, 3, 2);
INSERT INTO alumno_matricula_asignatura VALUES (1, 1, 3);
INSERT INTO alumno_matricula_asignatura VALUES (1, 2, 3);
INSERT INTO alumno_matricula_asignatura VALUES (1, 3, 3);
INSERT INTO alumno_matricula_asignatura VALUES (1, 1, 4);
INSERT INTO alumno_matricula_asignatura VALUES (1, 2, 4);
INSERT INTO alumno_matricula_asignatura VALUES (1, 3, 4);
INSERT INTO alumno_matricula_asignatura VALUES (2, 1, 1);
INSERT INTO alumno_matricula_asignatura VALUES (2, 2, 1);
INSERT INTO alumno_matricula_asignatura VALUES (2, 3, 1);
INSERT INTO alumno_matricula_asignatura VALUES (4, 1, 1);
INSERT INTO alumno_matricula_asignatura VALUES (4, 2, 1);
INSERT INTO alumno_matricula_asignatura VALUES (4, 3, 1);
INSERT INTO alumno_matricula_asignatura VALUES (4, 1, 2);
INSERT INTO alumno_matricula_asignatura VALUES (4, 2, 2);
INSERT INTO alumno_matricula_asignatura VALUES (4, 3, 2);
INSERT INTO alumno_matricula_asignatura VALUES (4, 4, 2);
INSERT INTO alumno_matricula_asignatura VALUES (4, 5, 2);
INSERT INTO alumno_matricula_asignatura VALUES (4, 6, 2);
INSERT INTO alumno_matricula_asignatura VALUES (4, 7, 2);
INSERT INTO alumno_matricula_asignatura VALUES (4, 8, 2);
INSERT INTO alumno_matricula_asignatura VALUES (4, 9, 2);
INSERT INTO alumno_matricula_asignatura VALUES (4, 10, 2);

```





### Consultas sobre una tabla

1. Devuelve un listado con el primer apellido, segundo apellido y el nombre de todos los alumnos. El listado deberá estar ordenado alfabéticamente de menor a mayor por el primer apellido, segundo apellido y nombre.

   ```mysql
   SELECT apellido1_alumno, apellido2_alumno, nombre_alumno
   FROM alumno
   ORDER BY apellido1_alumno ASC, apellido2_alumno ASC, nombre_alumno ASC;
   +------------------+------------------+---------------+
   | apellido1_alumno | apellido2_alumno | nombre_alumno |
   +------------------+------------------+---------------+
   | Domínguez        | Guerrero         | Antonio       |
   | Gea              | Ruiz             | Sonia         |
   | Gutiérrez        | López            | Juan          |
   | Heller           | Pagac            | Pedro         |
   | Herman           | Pacocha          | Daniel        |
   | Hernández        | Martínez         | Irene         |
   | Herzog           | Tremblay         | Ramón         |
   | Koss             | Bayer            | José          |
   | Lakin            | Yundt            | Inma          |
   | Saez             | Vega             | Juan          |
   | Sánchez          | Pérez            | Salvador      |
   | Strosin          | Turcotte         | Ismael        |
   +------------------+------------------+---------------+
   12 rows in set (0.00 sec)
   ```

     

2. Averigua el nombre y los dos apellidos de los alumnos que no han dado de alta su número de teléfono en la base de datos.

   ```mysql
   SELECT a.nombre_alumno, a.apellido1_alumno, a.apellido2_alumno
   FROM alumno a
   LEFT JOIN telefono_alumno ta ON a.id_alumno = ta.id_alumno
   WHERE ta.id_alumno IS NULL;
   +---------------+------------------+------------------+
   | nombre_alumno | apellido1_alumno | apellido2_alumno |
   +---------------+------------------+------------------+
   | Daniel        | Herman           | Pacocha          |
   | Inma          | Lakin            | Yundt            |
   | Juan          | Gutiérrez        | López            |
   | Antonio       | Domínguez        | Guerrero         |
   | Irene         | Hernández        | Martínez         |
   | Sonia         | Gea              | Ruiz             |
   +---------------+------------------+------------------+
   6 rows in set (0.00 sec)
   ```

     

3. Devuelve el listado de los alumnos que nacieron en 1999.

   ```mysql
   SELECT id_alumno, nif_alumno, nombre_alumno, apellido1_alumno, apellido2_alumno, fecha_nacimiento_alumno, id_sexo
   FROM alumno
   WHERE YEAR(fecha_nacimiento_alumno) = 1999;
   +-----------+------------+---------------+------------------+------------------+-------------------------+---------+
   | id_alumno | nif_alumno | nombre_alumno | apellido1_alumno | apellido2_alumno | fecha_nacimiento_alumno | id_sexo |
   +-----------+------------+---------------+------------------+------------------+-------------------------+---------+
   |         7 | 97258166K  | Ismael        | Strosin          | Turcotte         | 1999-05-24              | 1       |
   |        22 | 41491230N  | Antonio       | Domínguez        | Guerrero         | 1999-02-11              | 1       |
   +-----------+------------+---------------+------------------+------------------+-------------------------+---------+
   2 rows in set (0.00 sec)
   
   ```

   

4. Devuelve el listado de profesores que no han dado de alta su número de teléfono en la base de datos y además su nif termina en K.

   ```mysql
   SELECT p.id_profesor, p.nif_profesor, p.nombre_profesor, p.apellido1_profesor, p.apellido2_profesor, p.fecha_nacimiento_profesor, p.id_sexo, p.id_departamento
   FROM profesor p
   LEFT JOIN telefono_profesor tp ON p.id_profesor = tp.id_profesor
   WHERE tp.id_profesor IS NULL
   AND p.nif_profesor LIKE '%K';
   +-------------+--------------+-----------------+--------------------+--------------------+---------------------------+---------+-----------------+
   | id_profesor | nif_profesor | nombre_profesor | apellido1_profesor | apellido2_profesor | fecha_nacimiento_profesor | id_sexo | id_departamento |
   +-------------+--------------+-----------------+--------------------+--------------------+---------------------------+---------+-----------------+
   |          16 | 10485008K    | Antonio         | Fahey              | Considine          | 1982-03-18                | 1       |               4 |
   |          17 | 85869555K    | Guillermo       | Ruecker            | Upton              | 1973-05-05                | 1       |               6 |
   +-------------+--------------+-----------------+--------------------+--------------------+---------------------------+---------+-----------------+
   2 rows in set (0.00 sec)
   ```

     

5. Devuelve el listado de las asignaturas que se imparten en el primer cuatrimestre, en el tercer curso del grado que tiene el identificador 7.

   ```mysql
   SELECT a.id_asignatura, a.nombre_asignatura, a.creditos, a.curso, a.cuatrimestre, a.id_profesor, a.id_grado, a.id_tipo_asignatura
   FROM asignatura a
   INNER JOIN grado g ON a.id_grado = g.id_grado
   WHERE a.cuatrimestre = 1
   AND a.curso = 3
   AND g.id_grado = 7;
   +---------------+---------------------------+----------+-------+--------------+-------------+----------+--------------------+
   | id_asignatura | nombre_asignatura         | creditos | curso | cuatrimestre | id_profesor | id_grado | id_tipo_asignatura |
   +---------------+---------------------------+----------+-------+--------------+-------------+----------+--------------------+
   |            73 | Fisiología animal         |      4.5 |     3 |
   1 |           3 |        7 |                  2 |
   |            75 | Operaciones de separación |        6 |     3 |
   1 |           3 |        7 |                  2 |
   +---------------+---------------------------+----------+-------+--------------+-------------+----------+--------------------+
   2 rows in set (0.00 sec)
   ```
   
     

### Consultas multitabla (Composición interna)

1. Devuelve un listado con los datos de todas las alumnas que se han matriculado alguna vez en el Grado en Ingeniería Informática (Plan 2015).

   ```mysql
   SELECT DISTINCT a.id_alumno, a.nif_alumno, a.nombre_alumno, a.apellido1_alumno, a.apellido2_alumno, a.fecha_nacimiento_alumno, a.id_sexo
   FROM alumno a
   INNER JOIN alumno_matricula_asignatura ama ON a.id_alumno = ama.id_alumno
   INNER JOIN asignatura asi ON ama.id_asignatura = asi.id_asignatura
   INNER JOIN grado g ON asi.id_grado = g.id_grado
   INNER JOIN sexo s ON a.id_sexo = s.id_sexo
   WHERE s.nombre_sexo = 'M'
   AND g.nombre_grado = 'Grado en Ingeniería Informática (Plan 2015)';
   
   Empty set (0,01 sec)
   ```

   

2. Devuelve un listado con todas las asignaturas ofertadas en el Grado en Ingeniería Informática (Plan 2015).

   ```mysql
   SELECT a.id_asignatura, a.nombre_asignatura, a.creditos, a.curso, a.cuatrimestre, a.id_profesor, a.id_grado, a.id_tipo_asignatura
   FROM asignatura a
   INNER JOIN grado g ON a.id_grado = g.id_grado
   WHERE g.nombre_grado = 'Grado en Ingeniería Informática (Plan 2015)';
   +---------------+----------------------------------------+----------+-------+--------------+-------------+----------+--------------------+
   | id_asignatura | nombre_asignatura                      | creditos | curso | cuatrimestre | id_profesor | id_grado | id_tipo_asignatura |
   +---------------+----------------------------------------+----------+-------+--------------+-------------+----------+--------------------+
   |             1 | Álgegra lineal y matemática discreta   |        6 |     1 |            1 |           5 |        4 |                  1 |
   |             2 | Cálculo                                |        6 |     1 |            1 |          23 |        4 |                  1 |
   |             5 | Organización y gestión de empresas     |        6 |     1 |            1 |          20 |        4 |                  1 |
   |            17 | Estructura de Datos y Algoritmos II    |        6 |     2 |            2 |          14 |        4 |                  2 |
   |            37 | Sistemas de tiempo real                |        6 |     3 |            2 |          10 |        4 |                  3 |
   |            44 | Procesos de Ingeniería del Software 1  |        6 |     4 |            1 |          18 |        4 |                  3 |
   |            49 | Inteligencia del Negocio               |        6 |     4 |            2 |           3 |        4 |                  1 |
   +---------------+----------------------------------------+----------+-------+--------------+-------------+----------+--------------------+
   7 rows in set (0,00 sec)
   ```

   

3. Devuelve un listado de los profesores junto con el nombre del departamento al que están vinculados. El listado debe devolver cuatro columnas, primer apellido, segundo apellido, nombre y nombre del departamento. El resultado estará ordenado alfabéticamente de menor a mayor por los apellidos y el nombre.

   ```mysql
   SELECT p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor, d.nombre_departamento
   FROM profesor p
   INNER JOIN departamento d ON p.id_departamento = d.id_departamento
   ORDER BY p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor;
   +--------------------+--------------------+-----------------+-----------------------+
   | apellido1_profesor | apellido2_profesor | nombre_profesor | nombre_departamento   |
   +--------------------+--------------------+-----------------+-----------------------+
   | Domínguez          | Hernández          | María           | Informática           |
   | Fahey              | Considine          | Antonio         | Educación             |
   | Guerrero           | Martínez           | Juan            | Matemáticas           |
   | Hamill             | Kozey              | Manolo          | Biología y Geología   |
   | Kohler             | Schoen             | Alejandro       | Matemáticas           |
   | Lemke              | Rutherford         | Cristina        | Economía y Empresa    |
   | Monahan            | Murray             | Micaela         | Filología             |
   | Ramirez            | Gea                | Zoe             | Informática           |
   | Ruecker            | Upton              | Guillermo       | Química y Física      |
   | Sánchez            | Ruiz               | Pepe            | Química y Física      |
   | Schmidt            | Fisher             | David           | Agronomía             |
   | Schowalter         | Muller             | Francesca       | Derecho               |
   | Spencer            | Lakin              | Esther          | Biología y Geología   |
   | Stiedemann         | Morissette         | Alfredo         | Filología             |
   | Streich            | Hirthe             | Carmen          | Informática           |
   +--------------------+--------------------+-----------------+-----------------------+
   15 rows in set (0,00 sec)
   ```

   

4. Devuelve un listado con el nombre de las asignaturas, año de inicio y año de fin del curso escolar del alumno con nif 26902806M.

   ```mysql
   SELECT asi.nombre_asignatura, c.anyo_inicio, c.anyo_fin
   FROM asignatura asi
   INNER JOIN alumno_matricula_asignatura ama ON asi.id_asignatura = ama.id_asignatura
   INNER JOIN curso_escolar c ON c.id_curso_escolar = ama.id_curso_escolar
   INNER JOIN alumno a ON a.id_alumno = ama.id_alumno
   WHERE a.nif_alumno = '26902806M';
   +----------------------------------------+-------------+----------+
   | nombre_asignatura                      | anyo_inicio | anyo_fin |
   +----------------------------------------+-------------+----------+
   | Álgegra lineal y matemática discreta   |        2014 |     2015 |
   | Cálculo                                |        2014 |     2015 |
   | Física para informática                |        2014 |     2015 |
   +----------------------------------------+-------------+----------+
   3 rows in set (0,00 sec)
   ```

   

5. Devuelve un listado con el nombre de todos los departamentos que tienen profesores que imparten alguna asignatura en el Grado en Ingeniería Informática (Plan 2015).

   ```mysql
   SELECT DISTINCT d.nombre_departamento
   FROM departamento d
   INNER JOIN profesor p ON p.id_departamento = d.id_departamento
   INNER JOIN asignatura a ON a.id_profesor = p.id_profesor
   INNER JOIN grado g ON g.id_grado = a.id_grado
   WHERE g.nombre_grado = 'Grado en Ingeniería Informática (Plan 2015)';
   +-----------------------+
   | nombre_departamento   |
   +-----------------------+
   | Agronomía             |
   | Informática           |
   | Derecho               |
   | Biología y Geología   |
   | Filología             |
   +-----------------------+
   5 rows in set (0,00 sec)
   ```

   

6. Devuelve un listado con todos los alumnos que se han matriculado en alguna asignatura durante el curso escolar 2018/2019.

   ```mysql
   SELECT a.id_alumno, a.nif_alumno, a.nombre_alumno, a.apellido1_alumno, a.apellido2_alumno, a.fecha_nacimiento_alumno, a.id_sexo
   FROM alumno a
   INNER JOIN alumno_matricula_asignatura ama ON ama.id_alumno = a.id_alumno
   INNER JOIN curso_escolar c ON c.id_curso_escolar = ama.id_curso_escolar
   WHERE c.anyo_inicio = 2018 AND c.anyo_fin = 2019;
   
   Empty set (0,00 sec)
   ```
   
   

### Consultas multitabla (Composición externa)

Resuelva todas las consultas utilizando las cláusulas LEFT JOIN y RIGHT JOIN.

1. Devuelve un listado con los nombres de todos los profesores y los departamentos que tienen vinculados. El listado también debe mostrar aquellos profesores que no tienen ningún departamento asociado. El listado debe devolver cuatro columnas, nombre del departamento, primer apellido, segundo apellido y nombre del profesor. El resultado estará ordenado alfabéticamente de menor a mayor por el nombre del departamento, apellidos y el nombre.

   ```mysql
   SELECT d.nombre_departamento, p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor
   FROM profesor p
   LEFT JOIN departamento d ON d.id_departamento = p.id_departamento
   ORDER BY d.nombre_departamento, p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor;
   +-----------------------+--------------------+--------------------+-----------------+
   | nombre_departamento   | apellido1_profesor | apellido2_profesor | nombre_profesor |
   +-----------------------+--------------------+--------------------+-----------------+
   | Agronomía             | Schmidt            | Fisher             | David           |
   | Biología y Geología   | Hamill             | Kozey              | Manolo          |
   | Biología y Geología   | Spencer            | Lakin              | Esther          |
   | Derecho               | Schowalter         | Muller             | Francesca       |
   | Economía y Empresa    | Lemke              | Rutherford         | Cristina        |
   | Educación             | Fahey              | Considine          | Antonio         |
   | Filología             | Monahan            | Murray             | Micaela         |
   | Filología             | Stiedemann         | Morissette         | Alfredo         |
   | Informática           | Domínguez          | Hernández          | María           |
   | Informática           | Ramirez            | Gea                | Zoe             |
   | Informática           | Streich            | Hirthe             | Carmen          |
   | Matemáticas           | Guerrero           | Martínez           | Juan            |
   | Matemáticas           | Kohler             | Schoen             | Alejandro       |
   | Química y Física      | Ruecker            | Upton              | Guillermo       |
   | Química y Física      | Sánchez            | Ruiz               | Pepe            |
   +-----------------------+--------------------+--------------------+-----------------+
   15 rows in set (0,00 sec)
   ```

   

2. Devuelve un listado con los profesores que no están asociados a un departamento.

   ```mysql
   SELECT p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor
   FROM profesor p
   LEFT JOIN departamento d ON d.id_departamento = p.id_departamento
   WHERE p.id_departamento IS NULL;
   
   Empty set (0,00 sec)
   ```

   

3. Devuelve un listado con los departamentos que no tienen profesores asociados.

   ```mysql
   SELECT d.nombre_departamento
   FROM profesor p
   RIGHT JOIN departamento d ON d.id_departamento = p.id_departamento
   WHERE p.id_departamento IS NULL;
   
   Empty set (0,00 sec)
   ```

   

4. Devuelve un listado con los profesores que no imparten ninguna asignatura.

   ```mysql
   SELECT p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor
   FROM profesor p
   LEFT JOIN asignatura a  ON a.id_profesor = p.id_profesor
   WHERE a.id_profesor IS NULL;
   
   Empty set (0,00 sec)
   ```

   

5. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

   ```mysql
   SELECT a.nombre_asignatura
   FROM asignatura a
   LEFT JOIN profesor p ON a.id_profesor = p.id_profesor
   WHERE a.id_profesor IS NULL;
   
   Empty set (0,00 sec)
   ```

   

6. Devuelve un listado con todos los departamentos que tienen alguna asignatura que no se haya impartido en ningún curso escolar. El resultado debe mostrar el nombre del departamento y el nombre de la asignatura que no se haya impartido nunca.

   ```mysql
   SELECT d.nombre_departamento, a.nombre_asignatura
   FROM departamento d
   INNER JOIN profesor p ON d.id_departamento = p.id_departamento
   INNER JOIN asignatura a ON p.id_profesor = a.id_profesor
   LEFT JOIN alumno_matricula_asignatura ama ON a.id_asignatura = ama.id_asignatura
   WHERE ama.id_curso_escolar IS NULL;
   +---------------------+------------------------------------------------------------------------+
   | nombre_departamento | nombre_asignatura
                      |
   +---------------------+------------------------------------------------------------------------+
   | Informática         | Arquitectura de Computadores
                      |
   | Informática         | Estructura de Datos y Algoritmos I
                      |
   | Informática         | Sistemas Inteligentes
                      |
   | Informática         | Fundamentos de Redes de Computadores
                      |
   | Informática         | Planificación y Gestión de Proyectos Informáticos                      |
   | Informática         | Inteligencia del Negocio
                      |
   | Informática         | Procesos de Ingeniería del Software 2
                      |
   | Informática         | Fisiología animal
                      |
   | Informática         | Operaciones de separación
                      |
   | Informática         | Teoría de códigos y criptografía
                      |
   | Informática         | Tecnologías de acceso a red
                      |
   | Informática         | Biologia celular
                      |
   | Informática         | Fisiología vegetal
                      |
   | Informática         | Biorreactores
                      |
   | Informática         | Procesos biotecnológicos
                      |
   | Informática         | Desarrollo Rápido de Aplicaciones
                      |
   | Informática         | Biología vegetal y animal
                      |
   | Informática         | Matemáticas II
                      |
   | Informática         | Ingeniería genética
                      |
   | Informática         | Genómica y proteómica
                      |
   | Matemáticas         | Ingeniería de Sistemas de Información
                      |
   | Matemáticas         | Tratamiento digital de imágenes
                      |
   | Matemáticas         | Genética molecular
                      |
   | Matemáticas         | Seguridad y cumplimiento normativo
                      |
   | Matemáticas         | Gestión de la Calidad y de la Innovación Tecnológica                   |
   | Matemáticas         | Seguridad Informática
                      |
   | Matemáticas         | Microbiología
                      |
   | Matemáticas         | Biotecnología microbiana
                      |
   | Matemáticas         | Biotecnología vegetal
                      |
   | Economía y Empresa  | Integración de las Tecnologías de la Información en las Organizaciones |
   | Economía y Empresa  | Administración de bases de datos
                      |
   | Economía y Empresa  | Fiabilidad y Gestión de Riesgos
                      |
   | Economía y Empresa  | Tecnologías multimedia
                      |
   | Economía y Empresa  | Botánica agrícola
                      |
   | Economía y Empresa  | Virología
                      |
   | Economía y Empresa  | Biotecnología de los productos hortofrutículas                         |
   | Educación           | Administración de redes y sistemas operativos                          |
   | Educación           | Química orgánica
                      |
   | Agronomía           | Ingeniería de Requisitos
                      |
   | Agronomía           | Bases moleculares del desarrollo vegetal
                      |
   | Agronomía           | Metabolismo y biosíntesis de biomoléculas
                      |
   | Agronomía           | Patología molecular de plantas
                      |
   | Química y Física    | Modelado y Diseño del Software 1
                      |
   | Química y Física    | Líneas de Productos Software
                      |
   | Química y Física    | Informática industrial y robótica
                      |
   | Química y Física    | Análisis y planificación de las TI
                      |
   | Química y Física    | Física
                      |
   | Filología           | Herramientas y Métodos de Ingeniería del Software                      |
   | Filología           | Multiprocesadores
                      |
   | Filología           | Modelado y Diseño del Software 2
                      |
   | Filología           | Procesos de Ingeniería del Software 1
                      |
   | Filología           | Bioquímica
                      |
   | Derecho             | Sistema de Información para las Organizaciones                         |
   | Derecho             | Matemáticas I
                      |
   | Derecho             | Genética
                      |
   | Derecho             | Ingeniería bioquímica
                      |
   | Derecho             | Técnicas instrumentales básicas
                      |
   | Biología y Geología | Tecnologías web
                      |
   | Biología y Geología | Negocio Electrónico
                      |
   | Biología y Geología | Sistemas de tiempo real
                      |
   | Biología y Geología | Almacenes de Datos
                      |
   | Biología y Geología | Termodinámica y cinética química aplicada
                      |
   | Biología y Geología | Inmunología
                      |
   | Biología y Geología | Técnicas instrumentales avanzadas
                      |
   | Biología y Geología | Ingeniería del Software
                      |
   | Biología y Geología | Sistemas Operativos
                      |
   | Biología y Geología | Bases de Datos
                      |
   | Biología y Geología | Estructura de Datos y Algoritmos II
                      |
   | Biología y Geología | Programación de Servicios Software
                      |
   | Biología y Geología | Desarrollo de interfaces de usuario
                      |
   | Biología y Geología | Periféricos e interfaces
                      |
   | Biología y Geología | Química general
                      |
   | Biología y Geología | Bioinformática
                      |
   +---------------------+------------------------------------------------------------------------+
   73 rows in set (0.00 sec)
   ```
   
   

###   Consultas resumen

1. Devuelve el número total de alumnas que hay.

   ```mysql
   SELECT COUNT(id_alumno) total_alumnas
   FROM alumno, sexo
   WHERE sexo.id_sexo = alumno.id_sexo AND sexo.nombre_sexo = 'M';
   +---------------+
   | total_alumnas |
   +---------------+
   |             3 |
   +---------------+
   1 row in set (0,00 sec)
   ```

   

2. Calcula cuántos alumnos nacieron en 1999.

   ```mysql
   SELECT COUNT(id_alumno) nacidos_1999
   FROM alumno
   WHERE YEAR(fecha_nacimiento_alumno) = 1999;
   +--------------+
   | nacidos_1999 |
   +--------------+
   |            2 |
   +--------------+
   1 row in set (0,00 sec)
   ```

   

3. Calcula cuántos profesores hay en cada departamento. El resultado sólo debe mostrar dos columnas, una con el nombre del departamento y otra con el número de profesores que hay en ese departamento. El resultado sólo debe incluir los departamentos que tienen profesores asociados y deberá estar ordenado de mayor a menor por el número de profesores.

   ```mysql
   SELECT d.nombre_departamento, COUNT(p.id_departamento) cantidad_profesores
   FROM profesor p
   INNER JOIN departamento d ON d.id_departamento = p.id_departamento
   GROUP BY d.nombre_departamento
   ORDER BY cantidad_profesores DESC;
   +-----------------------+---------------------+
   | nombre_departamento   | cantidad_profesores |
   +-----------------------+---------------------+
   | Informática           |                   3 |
   | Matemáticas           |                   2 |
   | Química y Física      |                   2 |
   | Filología             |                   2 |
   | Biología y Geología   |                   2 |
   | Economía y Empresa    |                   1 |
   | Educación             |                   1 |
   | Agronomía             |                   1 |
   | Derecho               |                   1 |
   +-----------------------+---------------------+
   9 rows in set (0,00 sec)
   ```

   

4. Devuelve un listado con todos los departamentos y el número de profesores que hay en cada uno de ellos. Tenga en cuenta que pueden existir departamentos que no tienen profesores asociados. Estos departamentos también tienen que aparecer en el listado.

   ```mysql
   SELECT d.nombre_departamento, COUNT(p.id_departamento) cantidad_profesores
   FROM profesor p
   RIGHT JOIN departamento d ON d.id_departamento = p.id_departamento
   GROUP BY d.nombre_departamento;
   
   +-----------------------+---------------------+
   | nombre_departamento   | cantidad_profesores |
   +-----------------------+---------------------+
   | Informática           |                   3 |
   | Matemáticas           |                   2 |
   | Química y Física      |                   2 |
   | Filología             |                   2 |
   | Biología y Geología   |                   2 |
   | Economía y Empresa    |                   1 |
   | Educación             |                   1 |
   | Agronomía             |                   1 |
   | Derecho               |                   1 |
   +-----------------------+---------------------+
   9 rows in set (0,00 sec)
   ```

   

5. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno. Tenga en cuenta que pueden existir grados que no tienen asignaturas asociadas. Estos grados también tienen que aparecer en el listado. El resultado deberá estar ordenado de mayor a menor por el número de asignaturas.

   ```mysql
   SELECT g.nombre_grado, COUNT(a.id_asignatura) numero_asignaturas
   FROM grado g
   LEFT JOIN asignatura a ON g.id_grado = a.id_grado
   GROUP BY g.nombre_grado
   ORDER BY numero_asignaturas DESC;
   +--------------------------------------------------------+--------------------+
   | nombre_grado                                           | numero_asignaturas |
   +--------------------------------------------------------+--------------------+
   | Grado en Ingeniería Mecánica (Plan 2010)               |                 12 |
   | Grado en Ingeniería Agrícola (Plan 2015)               |                 11 |
   | Grado en Ingeniería Eléctrica (Plan 2014)              |                 10 |
   | Grado en Química (Plan 2009)                           |                 10 |
   | Grado en Biotecnología (Plan 2015)                     |
   9 |
   | Grado en Matemáticas (Plan 2010)                       |
   8 |
   | Grado en Ingeniería Informática (Plan 2015)            |                  7 |
   | Grado en Ingeniería Electrónica Industrial (Plan 2010) |
   6 |
   | Grado en Ingeniería Química Industrial (Plan 2010)     |
   6 |
   | Grado en Ciencias Ambientales (Plan 2009)              |
   4 |
   +--------------------------------------------------------+--------------------+
   10 rows in set (0.01 sec)
   ```

   

6. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno, de los grados que tengan más de 40 asignaturas asociadas.

   ```mysql
   SELECT g.nombre_grado, COUNT(a.id_asignatura) numero_asignaturas
   FROM grado g
   LEFT JOIN asignatura a ON g.id_grado = a.id_grado
   GROUP BY g.nombre_grado
   HAVING COUNT(a.id_asignatura)>40;
   
   Empty set (0.00 sec)
   ```

   

7. Devuelve un listado que muestre el nombre de los grados y la suma del número total de créditos que hay para cada tipo de asignatura. El resultado debe tener tres columnas: nombre del grado, tipo de asignatura y la suma de los créditos de todas las asignaturas que hay de ese tipo. Ordene el resultado de mayor a menor por el número total de créditos.

   ```mysql
   SELECT g.nombre_grado, ta.nombre_tipo_asignatura tipo_asig, SUM(a.creditos) tot_creditos
   FROM grado g
   INNER JOIN asignatura a ON g.id_grado = a.id_grado
   INNER JOIN tipo_asignatura ta ON a.id_tipo_asignatura = ta.id_tipo_asignatura
   GROUP BY g.nombre_grado, ta.nombre_tipo_asignatura
   ORDER BY tot_creditos DESC;
   +--------------------------------------------------------+-------------+--------------+
   | nombre_grado                                           | tipo_asig   | tot_creditos |
   +--------------------------------------------------------+-------------+--------------+
   | Grado en Ingeniería Mecánica (Plan 2010)               | Obligatoria |           39 |
   | Grado en Ingeniería Eléctrica (Plan 2014)              | Optativa    |           36 |
   | Grado en Ingeniería Agrícola (Plan 2015)               | Obligatoria |           30 |
   | Grado en Ingeniería Informática (Plan 2015)            | Basica      |           24 |
   | Grado en Biotecnología (Plan 2015)                     | Basica      |           24 |
   | Grado en Ingeniería Agrícola (Plan 2015)               | Optativa    |           24 |
   | Grado en Ingeniería Mecánica (Plan 2010)               | Optativa    |           24 |
   | Grado en Química (Plan 2009)                           | Optativa    |           24 |
   | Grado en Matemáticas (Plan 2010)                       | Obligatoria |         22.5 |
   | Grado en Química (Plan 2009)                           | Basica      |           18 |
   | Grado en Ingeniería Electrónica Industrial (Plan 2010) | Optativa    |           18 |
   | Grado en Ingeniería Química Industrial (Plan 2010)     | Obligatoria |         16.5 |
   | Grado en Biotecnología (Plan 2015)                     | Obligatoria |         16.5 |
   | Grado en Química (Plan 2009)                           | Obligatoria |           15 |
   | Grado en Ingeniería Agrícola (Plan 2015)               | Basica      |           12 |
   | Grado en Ingeniería Eléctrica (Plan 2014)              | Basica      |           12 |
   | Grado en Ciencias Ambientales (Plan 2009)              | Basica      |           12 |
   | Grado en Matemáticas (Plan 2010)                       | Basica      |           12 |
   | Grado en Ingeniería Química Industrial (Plan 2010)     | Basica      |           12 |
   | Grado en Ingeniería Eléctrica (Plan 2014)              | Obligatoria |           12 |
   | Grado en Ingeniería Informática (Plan 2015)            | Optativa    |           12 |
   | Grado en Biotecnología (Plan 2015)                     | Optativa    |           12 |
   | Grado en Matemáticas (Plan 2010)                       | Optativa    |           12 |
   | Grado en Ingeniería Electrónica Industrial (Plan 2010) | Obligatoria |         10.5 |
   | Grado en Ingeniería Electrónica Industrial (Plan 2010) | Basica      |            6 |
   | Grado en Ingeniería Mecánica (Plan 2010)               | Basica      |            6 |
   | Grado en Ingeniería Informática (Plan 2015)            | Obligatoria |            6 |
   | Grado en Ciencias Ambientales (Plan 2009)              | Obligatoria |            6 |
   | Grado en Ciencias Ambientales (Plan 2009)              | Optativa    |            6 |
   | Grado en Ingeniería Química Industrial (Plan 2010)     | Optativa    |            6 |
   +--------------------------------------------------------+-------------+--------------+
   30 rows in set (0.00 sec)
   ```

   

8. Devuelve un listado que muestre cuántos alumnos se han matriculado de alguna asignatura en cada uno de los cursos escolares. El resultado deberá mostrar dos columnas, una columna con el año de inicio del curso escolar y  otra con el número de alumnos matriculados. 

   ```mysql
   SELECT ce.anyo_inicio, COUNT(DISTINCT ama.id_alumno) AS num_alumnos_matriculados
   FROM curso_escolar ce
   LEFT JOIN alumno_matricula_asignatura ama ON ce.id_curso_escolar = ama.id_curso_escolar
   GROUP BY ce.anyo_inicio
   ORDER BY ce.anyo_inicio;
   +-------------+--------------------------+
   | anyo_inicio | num_alumnos_matriculados |
   +-------------+--------------------------+
   |        2014 |                        3 |
   |        2015 |                        2 |
   |        2016 |                        1 |
   |        2017 |                        1 |
   +-------------+--------------------------+
   4 rows in set (0.01 sec)
   ```

   

9. Devuelve un listado con el número de asignaturas que imparte cada profesor. El listado debe tener en cuenta aquellos profesores que no imparten ninguna asignatura. El resultado mostrará cinco columnas: id, nombre, primer apellido, segundo apellido y número de asignaturas. El resultado estará ordenado de mayor a menor por el número de asignaturas.

   ```mysql
   SELECT p.id_profesor, p.nombre_profesor, p.apellido1_profesor, p.apellido2_profesor, COUNT(a.id_asignatura) AS num_asignaturas
   FROM profesor p
   LEFT JOIN asignatura a ON p.id_profesor = a.id_profesor
   GROUP BY p.id_profesor, p.nombre_profesor, p.apellido1_profesor, p.apellido2_profesor
   ORDER BY num_asignaturas DESC;
   +-------------+-----------------+--------------------+--------------------+-----------------+
   | id_profesor | nombre_profesor | apellido1_profesor | apellido2_profesor | num_asignaturas |
   +-------------+-----------------+--------------------+--------------------+-----------------+
   |           3 | Zoe             | Ramirez            | Gea                |              10 |
   |           8 | Cristina        | Lemke              | Rutherford         |               9 |
   |          14 | Manolo          | Hamill             | Kozey              |               9 |
   |           5 | David           | Schmidt            | Fisher             |               7 |
   |          10 | Esther          | Spencer            | Lakin              |               7 |
   |          12 | Carmen          | Streich            | Hirthe             |               6 |
   |          20 | Francesca       | Schowalter         | Muller             |               6 |
   |          22 | Juan            | Guerrero           | Martínez           |               6 |
   |          23 | María           | Domínguez          | Hernández          |               6 |
   |          18 | Micaela         | Monahan            | Murray             |               4 |
   |          21 | Pepe            | Sánchez            | Ruiz               |               4 |
   |          15 | Alejandro       | Kohler             | Schoen             |               3 |
   |          17 | Guillermo       | Ruecker            | Upton              |               3 |
   |          16 | Antonio         | Fahey              | Considine          |               2 |
   |          13 | Alfredo         | Stiedemann         | Morissette         |               1 |
   +-------------+-----------------+--------------------+--------------------+-----------------+
   15 rows in set (0.01 sec)
   ```
   
     

### Subconsultas

1. Devuelve todos los datos del alumno más joven. 

   ```mysql
   SELECT id_alumno, nif_alumno, nombre_alumno, apellido1_alumno, apellido2_alumno, fecha_nacimiento_alumno fecha_nac, id_sexo
   FROM alumno
   WHERE fecha_nacimiento_alumno = (
   	SELECT MIN(fecha_nacimiento_alumno)
   	FROM alumno
   );
   +-----------+------------+---------------+------------------+------------------+------------+---------+
   | id_alumno | nif_alumno | nombre_alumno | apellido1_alumno | apellido2_alumno | fecha_nac  | id_sexo |
   +-----------+------------+---------------+------------------+------------------+------------+---------+
   |         2 | 26902806M  | Salvador      | Sánchez          | Pérez
      | 1991-03-28 | 1       |
   +-----------+------------+---------------+------------------+------------------+------------+---------+
   1 row in set (0.00 sec)
   ```

   

2. Devuelve un listado con los profesores que no están asociados a un departamento. 

   ```mysql
   SELECT id_profesor, nif_profesor, nombre_profesor, apellido1_profesor, apellido2_profesor, fecha_nacimiento_profesor, id_sexo, id_departamento
   FROM profesor
   WHERE id_profesor NOT IN (
   	SELECT id_profesor
       FROM profesor
       WHERE id_departamento IS NOT NULL
   );
   
   Empty set (0.00 sec)
   ```

   

3. Devuelve un listado con los departamentos que no tienen profesores asociados. 

   ```mysql
   SELECT id_departamento, nombre_departamento
   FROM departamento
   WHERE id_departamento NOT IN (
   	SELECT id_departamento
       FROM profesor
   );
   
   Empty set (0.01 sec)
   ```

   

4. Devuelve un listado con los profesores que tienen un departamento asociado y que no imparten ninguna asignatura.

   ```mysql
   SELECT id_profesor, nif_profesor, nombre_profesor, apellido1_profesor, apellido2_profesor, fecha_nacimiento_profesor, id_sexo, id_departamento
   FROM profesor
   WHERE id_departamento IS NOT NULL
   AND id_profesor NOT IN (
       SELECT id_profesor
       FROM asignatura
   );
       
   Empty set (0.00 sec)
   ```

   

5. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

   ```mysql
   SELECT id_asignatura, nombre_asignatura, creditos, curso, cuatrimestre, id_profesor, id_grado, id_tipo_asignatura
   FROM asignatura
   WHERE id_profesor IS NULL;
   
   Empty set (0.00 sec)
   ```

   

6. Devuelve un listado con todos los departamentos que no han impartido asignaturas en ningún curso escolar.

   ```mysql
   SELECT id_departamento, nombre_departamento
   FROM departamento
   WHERE id_departamento NOT IN (
       SELECT DISTINCT d.id_departamento
       FROM departamento d
       INNER JOIN profesor p ON d.id_departamento = p.id_departamento
       INNER JOIN asignatura a ON p.id_profesor = a.id_profesor
       INNER JOIN alumno_matricula_asignatura ama ON a.id_asignatura = ama.id_asignatura
   );
   +-----------------+---------------------+
   | id_departamento | nombre_departamento |
   +-----------------+---------------------+
   |               2 | Matemáticas         |
   |               4 | Educación           |
   |               7 | Filología           |
   |               9 | Biología y Geología |
   +-----------------+---------------------+
   4 rows in set (0.00 sec)
   ```
   
     

#### VISTAS

1. Vista para la consulta Devuelve un listado con todos los departamentos y el número de profesores que hay en cada uno de ellos. Tenga en cuenta que pueden existir departamentos que no tienen profesores asociados. Estos departamentos también tienen que aparecer en el listado.

   ```mysql
   -- Creación Vista
   CREATE VIEW profesores_departamentos AS
   SELECT d.nombre_departamento, COUNT(p.id_departamento) cantidad_profesores
   FROM profesor p
   RIGHT JOIN departamento d ON d.id_departamento = p.id_departamento
   GROUP BY d.nombre_departamento;
   
   -- Llamado de la vista
   SELECT nombre_departamento, cantidad_profesores FROM profesores_departamentos;
   ```

   

2. Vista para la consulta Devuelve un listado con el primer apellido, segundo apellido y el nombre de todos los alumnos. El listado deberá estar ordenado alfabéticamente de menor a mayor por el primer apellido, segundo apellido y nombre.

   ```mysql
   -- Creación Vista
   CREATE VIEW all_alumnos AS
   SELECT apellido1_alumno, apellido2_alumno, nombre_alumno
   FROM alumno
   ORDER BY apellido1_alumno ASC, apellido2_alumno ASC, nombre_alumno ASC;
   
   -- Llamado de la vista
   SELECT apellido1_alumno, apellido2_alumno, nombre_alumno FROM all_alumnos;
   ```

   

3. Vista para la consulta Devuelve un listado de los profesores junto con el nombre del departamento al que están vinculados. El listado debe devolver cuatro columnas, primer apellido, segundo apellido, nombre y nombre del departamento. El resultado estará ordenado alfabéticamente de menor a mayor por los apellidos y el nombre.

   ```mysql
   -- Creación Vista
   CREATE VIEW profesor_departamento AS
   SELECT p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor, d.nombre_departamento
   FROM profesor p
   INNER JOIN departamento d ON p.id_departamento = d.id_departamento
   ORDER BY p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor;
   
   -- Llamado de la vista
   SELECT apellido1_profesor, apellido2_profesor, nombre_profesor, nombre_departamento FROM profesor_departamento;
   ```

   

4. Vista para la consulta Devuelve un listado con todas las asignaturas ofertadas en el Grado en Ingeniería Informática (Plan 2015).

   ```mysql
   -- Creación Vista
   CREATE VIEW pensum_ing_infor_2015 AS
   SELECT a.id_asignatura, a.nombre_asignatura, a.creditos, a.curso, a.cuatrimestre, a.id_profesor, a.id_grado, a.id_tipo_asignatura
   FROM asignatura a
   INNER JOIN grado g ON a.id_grado = g.id_grado
   WHERE g.nombre_grado = 'Grado en Ingeniería Informática (Plan 2015)';
   
   -- Llamado de la vista
   SELECT id_asignatura, nombre_asignatura, creditos, curso, cuatrimestre, id_profesor, id_grado, id_tipo_asignatura FROM pensum_ing_infor_2015;
   ```

   

5. Vista para la consulta Devuelve todos los datos del alumno más joven. 

   ```mysql
   -- Creación Vista
   CREATE VIEW alumno_joven AS
   SELECT id_alumno, nif_alumno, nombre_alumno, apellido1_alumno, apellido2_alumno, fecha_nacimiento_alumno fecha_nac, id_sexo
   FROM alumno
   WHERE fecha_nacimiento_alumno = (
   	SELECT MIN(fecha_nacimiento_alumno)
   	FROM alumno
   );
   
   -- Llamado de la vista
   SELECT id_alumno, nif_alumno, nombre_alumno, apellido1_alumno, apellido2_alumno, fecha_nac, id_sexo FROM  alumno_joven;
   ```

   

6. Vista para la consulta Devuelve un listado que muestre el nombre de los grados y la suma del número total de créditos que hay para cada tipo de asignatura. El resultado debe tener tres columnas: nombre del grado, tipo de asignatura y la suma de los créditos de todas las asignaturas que hay de ese tipo. Ordene el resultado de mayor a menor por el número total de créditos.

   ```mysql
   -- Creación Vista
   CREATE VIEW grado_asignatura AS
   SELECT g.nombre_grado, ta.nombre_tipo_asignatura tipo_asig, SUM(a.creditos) tot_creditos
   FROM grado g
   INNER JOIN asignatura a ON g.id_grado = a.id_grado
   INNER JOIN tipo_asignatura ta ON a.id_tipo_asignatura = ta.id_tipo_asignatura
   GROUP BY g.nombre_grado, ta.nombre_tipo_asignatura
   ORDER BY tot_creditos DESC;
   
   -- Llamado de la vista
   SELECT nombre_grado, tipo_asig, tot_creditos FROM grado_asignatura;
   ```

   

7. Vista para la consulta Devuelve un listado con los nombres de todos los profesores y los departamentos que tienen vinculados. El listado también debe mostrar aquellos profesores que no tienen ningún departamento asociado. El listado debe devolver cuatro columnas, nombre del departamento, primer apellido, segundo apellido y nombre del profesor. El resultado estará ordenado alfabéticamente de menor a mayor por el nombre del departamento, apellidos y el nombre.

   ```mysql
   -- Creación Vista
   CREATE VIEW profesor_departamento AS
   SELECT d.nombre_departamento, p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor
   FROM profesor p
   LEFT JOIN departamento d ON d.id_departamento = p.id_departamento
   ORDER BY d.nombre_departamento, p.apellido1_profesor, p.apellido2_profesor, p.nombre_profesor;
   
   -- Llamado de la vista
   SELECT nombre_departamento, apellido1_profesor, apellido2_profesor,nombre_profesor FROM profesor_departamento;
   ```

   

8. Vista para la consulta Devuelve un listado con las asignaturas que no tienen un profesor asignado.

   ```mysql
   -- Creación Vista
   CREATE VIEW asignatura_sin_profesor AS
   SELECT id_asignatura, nombre_asignatura, creditos, curso, cuatrimestre, id_profesor, id_grado, id_tipo_asignatura
   FROM asignatura
   WHERE id_profesor IS NULL;
   
   -- Llamado de la vista
   SELECT id_asignatura, nombre_asignatura, creditos, curso, cuatrimestre, id_profesor, id_grado, id_tipo_asignatura FROM asignatura_sin_profesor;
   ```

   

9. Vista para la consulta Devuelve un listado con los profesores que no están asociados a un departamento. 

   ```mysql
   -- Creación Vista
   CREATE VIEW profesor_sin_departamento AS
   SELECT id_profesor, nif_profesor, nombre_profesor, apellido1_profesor, apellido2_profesor, fecha_nacimiento_profesor, id_sexo, id_departamento
   FROM profesor
   WHERE id_profesor NOT IN (
   	SELECT id_profesor
       FROM profesor
       WHERE id_departamento IS NOT NULL
   );
   
   -- Llamado de la vista
   SELECT id_profesor, nif_profesor, nombre_profesor, apellido1_profesor, apellido2_profesor, fecha_nacimiento_profesor, id_sexo, id_departamento FROM profesor_sin_departamento;
   ```

   

10. Vista para la consulta Devuelve un listado con el número de asignaturas que imparte cada profesor. El listado debe tener en cuenta aquellos profesores que no imparten ninguna asignatura. El resultado mostrará cinco columnas: id, nombre, primer apellido, segundo apellido y número de asignaturas. El resultado estará ordenado de mayor a menor por el número de asignaturas.

    ```mysql
    -- Creación Vista
    CREATE VIEW asignaturas_por_profesor AS
    SELECT p.id_profesor, p.nombre_profesor, p.apellido1_profesor, p.apellido2_profesor, COUNT(a.id_asignatura) AS num_asignaturas
    FROM profesor p
    LEFT JOIN asignatura a ON p.id_profesor = a.id_profesor
    GROUP BY p.id_profesor, p.nombre_profesor, p.apellido1_profesor, p.apellido2_profesor
    ORDER BY num_asignaturas DESC;
    
    -- Llamado de la vista
    SELECT id_profesor, nombre_profesor, apellido1_profesor, apellido2_profesor, num_asignaturas FROM asignaturas_por_profesor;
    ```






### PROCEDIMIENTOS ALMACENADOS

1. Insertar país

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS insert_pais;
   CREATE PROCEDURE insert_pais(
   	IN id VARCHAR(5),
   	IN nombre_pais VARCHAR(45)
   )
   BEGIN
   	INSERT INTO pais VALUES (id,nombre_pais);
   END $$
   
   DELIMITER ;
   
   CALL insert_pais('PER','PERU');
   ```

2. Editar pais

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS editar_pais;
   CREATE PROCEDURE editar_pais(
   	IN id VARCHAR(5),
   	IN nombre VARCHAR(45)
   )
   BEGIN 
   	UPDATE pais SET nombre_pais = nombre WHERE id_pais = id;
   END $$
   
   DELIMITER ;
   
   CALL editar_pais('PER','Perú');
   ```

   

3. Eliminar pais

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS eliminar_pais;
   CREATE PROCEDURE eliminar_pais(
   	IN id VARCHAR(5)
   )
   BEGIN
   	DELETE FROM pais WHERE id_pais = id;
   END $$
   
   DELIMITER ;
   
   CALL eliminar_pais ('PER');
   ```

   

4. Insertar sexo

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS insert_sexo;
   CREATE PROCEDURE insert_sexo(
   	IN id VARCHAR(3),
   	IN nombre_sexo VARCHAR(45)
   )
   BEGIN
   	INSERT INTO sexo VALUES (id,nombre_sexo);
   END $$
   
   DELIMITER ;
   
   CALL insert_sexo (3,'No Binarie');
   ```

   

5. Editar sexo

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS editar_sexo;
   CREATE PROCEDURE editar_sexo(
   	IN id VARCHAR(3),
   	IN nombre VARCHAR(45)
   )
   BEGIN 
   	UPDATE sexo SET nombre_sexo = nombre WHERE id_sexo = id;
   END $$
   
   DELIMITER ;
   
   CALL editar_sexo(3,'NO Binario');
   ```

   

6. Eliminar sexo

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS eliminar_sexo;
   CREATE PROCEDURE eliminar_sexo(
   	IN id VARCHAR(3)
   )
   BEGIN
   	DELETE FROM sexo WHERE id_sexo = id;
   END $$
   
   DELIMITER ;
   
   CALL eliminar_sexo ('3');
   ```

   

7. Contar alumnas 

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS conteo_alumnas;
   CREATE PROCEDURE conteo_alumnas(
   	OUT total INT
   )
   BEGIN 
   	SET total = (
          SELECT COUNT(id_alumno) 
          FROM alumno 
          WHERE id_sexo = (SELECT id_sexo FROM sexo WHERE nombre_sexo = 'M')
       );
   END $$
   
   DELIMITER ;
   
   
   CALL conteo_alumnas (@total);
   SELECT @total total_alumnas;
   ```

   

8. Alumnos por año de nacimiento

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS alumnos_nacidos_anio;
   CREATE PROCEDURE alumnos_nacidos_anio(
       IN anio INT
   )
   BEGIN 
      SELECT COUNT(id_alumno) nacidos_anio
      FROM alumno
      WHERE YEAR(fecha_nacimiento_alumno) = anio;
   END $$
   
   DELIMITER ;
   
   CALL alumnos_nacidos_anio (2000);
   
   ```

   

9. Alumnos matriculados por grado

   ```mysql
   DELIMITER $$
   DROP PROCEDURE IF EXISTS alumnos_por_grado$$
   CREATE PROCEDURE alumnos_por_grado()
   BEGIN
       SELECT g.nombre_grado, COUNT(ama.id_alumno) AS total_alumnos
       FROM grado g
       LEFT JOIN asignatura a ON g.id_grado = a.id_grado
       LEFT JOIN alumno_matricula_asignatura ama ON a.id_asignatura = ama.id_asignatura
       GROUP BY g.nombre_grado;
   END$$
   
   DELIMITER ;
   
   
   CALL alumnos_por_grado();
   ```

   

10. Asignaturas por alumno

    ```mysql
    DELIMITER $$
    DROP PROCEDURE IF EXISTS asignaturas_alumno$$
    CREATE PROCEDURE asignaturas_alumno(
        IN nif_alumno VARCHAR(50)
    )
    BEGIN
        SELECT asi.nombre_asignatura, c.anyo_inicio, c.anyo_fin
        FROM asignatura asi
        INNER JOIN alumno_matricula_asignatura ama ON asi.id_asignatura = ama.id_asignatura
        INNER JOIN curso_escolar c ON c.id_curso_escolar = ama.id_curso_escolar
        INNER JOIN alumno a ON a.id_alumno = ama.id_alumno
        WHERE a.nif_alumno = nif_alumno;
    END$$
    
    DELIMITER ;
    
    
    
    CALL asignaturas_alumno('26902806M');
    
    ```

    
