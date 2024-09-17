# Algunos de los ejercicios del curso de Unión de Datos en SQL

Puedes acceder al curso  [aquí](https://app.datacamp.com/learn/courses/joining-data-in-sql), tiene una duración de 4 horas, 14 videos y 47 ejercicios. 👩‍💻

Certificado  [aquí](https://www.datacamp.com/completed/statement-of-accomplishment/course/d9c8fd756b09d5e313149fc7dd41f3cd3eabee66)

## Descripción del curso:

Aprenderás a trabajar con más de una tabla en SQL; a utilizar uniones internas, uniones externas y uniones cruzadas; a aprovechar la teoría de conjuntos, incluidas las cláusulas de unión, intersección y excepción; y a crear consultas anidadas. Cada paso va acompañado de ejercicios y oportunidades para aplicar la teoría y aumentar tu confianza en SQL.

## 1️⃣ Introducción a las uniones internas

Conocerás el concepto de unión de tablas y explorarás todas las formas en las que puedes enriquecer tus consultas utilizando uniones, empezando por las uniones internas.

### INNER JOIN
-- Selecciona todas las columnas de cities

```
SELECT * 
FROM cities;
```
#### Unir con tablas con alias

```
-- Selecciona campos con alias
SELECT c.code AS country_code, c.name, e.year, e.inflation_rate
FROM countries AS c
-- Unir a economies (alias e)
INNER JOIN economies AS e
-- Une usando code y los alias de las tablas
ON c.code = e.code;
```

#### USING 

```
SELECT c.name AS country, l.name AS language, official
FROM countries AS c
INNER JOIN languages AS l
-- Une usando la columna code
USING(code);
```

### Definir Relaciones

```
-- Seleccione los nombres de países e idiomas, con alias
SELECT c.name AS country, l.name AS language
-- From countries (con alias)
FROM countries AS c 
-- Inner join a languages (con alias)
INNER JOIN languages AS l
-- Usa code como campo de unión con la palabra clave USING
USING(code);
```

### Uniones Múltiples

Tu tarea en este ejercicio es unir tablas para obtener el nombre del país, el año, la tasa de fertilidad y la tasa de desempleo en un único resultado a partir de las tablas countries, populations y economies.

```
-- Selecciona los campos pertinentes
SELECT name, year, fertility_rate
FROM countries AS c 
-- Inner join countries y populations, con alias, uniendo en code
INNER JOIN populations AS p
ON c.code = p.country_code;
```

### Comprobación de uniones multitabla

```
SELECT name, e.year, fertility_rate, unemployment_rate
FROM countries AS c
INNER JOIN populations AS p
ON c.code = p.country_code
INNER JOIN economies AS e
ON c.code = e.code
-- Agrega una condición de unión adicional para que también se una con `year`
AND e.year = p.year;
```

## 2️⃣ Uniones externas, uniones cruzadas y autouniones

Aprenderás sobre las uniones cruzadas y las situaciones en las que puedes unir una tabla consigo misma.

### LEFT Y RIGHT JOING 

-- Realiza una union interna con cities AS c1 a la izquierda y countries as c2 a la derecha. Utiliza code como campo por el que fusionar tus tablas.

```
SELECT
c1.name AS city,
code
c2.name AS country,
region,
city_proper_pop
FROM cities AS c1
-- Realiza una inner join con cities as c1 y countries as c2 uniendo en country code
INNER JOIN countries As c2
ON c1.country_code = c2.code
ORDER BY code DESC;
```

-- Completa la LEFT JOIN con la tabla countries a la izquierda y la tabla econonies a la derecha en el campo code. Filtra los registros desde year 2010.

```
SELECT name, region, gdp_percapita
FROM countries AS c
LEFT JOIN economies AS e
-- Uniendo con el campo code
ON c.code = e.code
-- Filtra para year 2010
WHERE year = 2010;
```

-- Escribe una nueva consulta utilizando RIGHT JOIN que produzca un resultado identico al de la LEFT JOIN proporcionada.

```
F- Modifica esta consulta para usar RIGHT JOIN en Lugar de LEFT JOIN
SELECT countries.name AS country, Languages.name AS language, percent
FROM Languages
RIGHT JOIN countries
USING (code)
ORDER BY Language;
```


## 3️⃣ Teoría de conjuntos para uniones de SQL

Aprenderás a utilizar las operaciones de la teoría de conjuntos en SQL, con una introducción a las cláusulas UNION, UNION ALL, INTERSECT y EXCEPT. Explorarás las formas predominantes en las que las operaciones de la teoría de conjuntos difieren de las operaciones de unión.

## 4️⃣ Subconsultas

Empezarás investigando las semiuniones y las antiuniones. A continuación, aprenderás a utilizar las consultas anidadas. Por último, pero no por ello menos importante, terminarás el curso con algunos desafíos.
