# Algunos de los ejercicios del curso de Unión de Datos en SQL

Puedes acceder al curso  [aquí](https://app.datacamp.com/learn/courses/joining-data-in-sql), tiene una duración de 4 horas, 14 videos y 47 ejercicios. 👩‍💻

Certificado  [aquí](https://www.datacamp.com/completed/statement-of-accomplishment/course/d9c8fd756b09d5e313149fc7dd41f3cd3eabee66)

🔖 Puede seguir la ruta de los cursos en el siguiente orden: 

1- [SQL_Intermedio](https://github.com/MFlorenciaLoCascio/SQL_Intermedio)

2- [Unión de Datos](https://github.com/MFlorenciaLoCascio/SQL_Union_de_Datos) Aquí esta ahora

3- [Manipulación de Datos](https://github.com/MFlorenciaLoCascio/SQL_Manipulacion_de_Datos)

4- [Resumen de estadísticas y funciones de ventana](https://github.com/MFlorenciaLoCascio/SQL_Funciones_de_Ventana)

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
-- Modifica esta consulta para usar RIGHT JOIN en Lugar de LEFT JOIN
SELECT countries.name AS country, Languages.name AS language, percent
FROM Languages
RIGHT JOIN countries
USING (code)
ORDER BY Language;
```

### FULL JOIN 

Supongamos que estas haciendo una investigación sobre Melanesia y Micronesia, y te interesa incluir información sobre idiomas y monedas en los datos que vemos sobre estas regiones en la tabla countries. Como los idiomas y las monedas estan en tablas diferentes, para ello seran necesarias dos uniones completas consecutivas en las que intervendran las tablas countries, Languages y currencies.

-- Completa la FULL JOIN con countries as c1 a la izquierda y Languages as 1 a la derecha, utilizando code para realizar esta unión. Encadena esta unión con otra FULL JOIN , colocando currencies a la derecha y aplicando On a code de nuevo.

```
SELECT
c1.name AS country,
region,
L.name AS Language,
basic_unit,
frac_unit
FROM countries as c1
-- Full join con Languages (alias L)
FULL JOIN Languages as L
USING (code)
-- Full join con currencies (alias c2)
FULL JOIN currencies as c2
USING (code)
WHERE region LIKE 'M%esia';
```

-- Completa el código para realizar una INNER JOIN de countries AS c con languages AS l, utilizando el campo code para obtener los idiomas que se hablan actualmente en los dos países.

```
SELECT c.name AS country, L.name AS Language
FROM countries AS C
-- Inner join countries as c con languages as L en el campo code
INNER JOIN Languages AS L
USING (code)
WHERE c.code IN ('PAK', 'IND')
AND L.code in ('PAK', 'IND');
```

-- Completa la union de countries AS c con populations as p. Filtra por el año 2010. Ordena tus resultados por esperanza de vida de menos a más. Limita el resultado a cinco países.

```
SELECT
c.name AS country,
region,
Life_expectancy AS Life_exp
FROM countries AS C
-- Une a populations (alias p) usando una unión apropiada
FULL JOIN populations AS p
ON c.code = p.country_code
-- Filtro para solo resultados en year 2010
WHERE year = 2010
-- Ordena por Life_exp
ORDER BY Life_expectancy ASC
-- Limita a cinco registros
LIMIT 5;
```

### Comparar una tabla consigo misma

-- Realiza una unión interna de populations consigo misma aplicando ON a country_code , alias p1 y p2, respectivamente. Selecciona country_code de p1 y el campo size tanto de p1 como de p2 , asignando a p1.size el alias size2010 y a p2.size el alias size2015 (en ese orden).

```
-- Selecciona campos con alias de populations como p1
SELECT p1.country_code, p1.size AS size2010, p2.size AS size2015
FROM populations AS p1
-- Inner Join populations como p1 a si misma, con el alias p2, uniendo en country code
INNER JOIN populations AS p2
USING (country_code)
```

## 3️⃣ Teoría de conjuntos para uniones de SQL

Aprenderás a utilizar las operaciones de la teoría de conjuntos en SQL, con una introducción a las cláusulas UNION, UNION ALL, INTERSECT y EXCEPT. Explorarás las formas predominantes en las que las operaciones de la teoría de conjuntos difieren de las operaciones de unión.

### UNION: no devuelve duplicados

En este ejercicio, tienes a tu disposición dos tablas, economies2015 y economies2019, en las pestañas de la consola. Realizarás una operación de conjuntos para apilar todos los registros de estas dos tablas unos sobre otros, excluyendo los duplicados.

```
-- Selecciona todos los campos de economies2015
SELECT *
FROM economies2015
-- Operacion de conjuntos
UNION
-- Selecciona todos los campos de economies2019
SELECT *
FROM economies2019
ORDER BY code, year;
```

Realiza una operación de conjuntos adecuada que determine todos los pares de código de país y año (en ese orden) de economies y populations , excluyendo los duplicados. Ordena por código de país y año.

```
-- Consulta que determina todos los pares de code y year de economies y populations, sin duplicados
SELECT code, year
FROM economies
UNION
SELECT country_code, year
FROM populations
ORDER BY code, year;
```

### UNION ALL: devuelve duplicados

### INTERSECT: 

```
-- Devuelve todas las ciudades con el mismo nombre que un pais
SELECT name
FROM countries
INTERSECT
SELECT name
FROM cities;
```

### EXCEPT 

```
-- Devuelve todas las ciudades que no tienen el mismo nombre que un pais
SELECT name
FROM cities
EXCEPT
SELECT name
FROM countries
ORDER BY name;
```

## 4️⃣ Subconsultas

Empezarás investigando las semiuniones y las antiuniones. A continuación, aprenderás a utilizar las consultas anidadas. Por último, pero no por ello menos importante, terminarás el curso con algunos desafíos.

### SEMI UNION

Supongamos que te interesa identificar los idiomas hablados en Oriente Medio. La tabla Languages contiene informacion sobre idiomas y paises, pero no te dice a que region pertenecen los paises. Puedes construir una semiunion filtrando la tabla countries por una region concreta y, a continuación, utilizarla para filtrar la tabla Languages.

```
-- Selecciona el code de país para los países de Middle East
SELECT code
FROM countries
WHERE region = 'Middle East';
```

### SUBCONSULTAS DENTRO DE WHERE

-- Empieza calculando la esperanza de vida media a partir de la tabla populations. Filtra tu respuesta para utilizar solo registros de 2015.

```
-- Selecciona el promedio de Life_expectancy de la tabla populations
SELECT AVG(Life_expectancy)
FROM populations
-- Filtrar por year 2015
WHERE year = 2015;
```

-- Obten name, country_code y urbanarea_pop de todas las capitales (sin alias).

```
-- Selecciona los campos relevantes de la tabla cities
SELECT name, country_code, urbanarea_pop
FROM cities
-- Filtra mediante una subconsulta en la tabla countries
WHERE name IN
(SELECT capital
FROM countries)
ORDER BY urbanarea_pop DESC;
```

### SUBCONSULTAS DENTRO DE SELECT

Escribe una LEFT JOIN con countries a la izquierda y cities a la derecha, aplicando Join On al código de país. En la declaracion SELECT de tu union, incluye los nombres de pais como country y cuenta las ciudades de cada pais, alias cities_num. Ordena por cities_num (de mas a menos) y country (de menos a mas), limitandote a los nueve primeros registros.

```
SELECT countries.name AS country, COUNT(*) AS cities_num
FROM countries
LEFT JOIN cities
ON countries.code = cities.country_code
GROUP BY countries.name
ORDER BY cities_num DESC, country
LIMIT 9;
```

### SUBCONSULTAS DENTRO DE FROM

Empieza con una consulta que agrupe por cada code de pais de Languages y cuente los idiomas habladas en cada pais como Lang_num. En tu declaracion SELECT , obten code y Lang_nun (en ese orden).

```
-- Selecciona code y el numero de idiomas como lang_num
SELECT code, COUNT (name) AS Lang_num
FROM Languages
GROUP BY code;
```

-- Selecciona code , inflation_rate y unemployment_rate de pais en economies. Filtra code por el conjunto de paises que contienen las palabras "Republic" o "Monarchy" en su gov_form.

```
-- Selecciona Los campos pertinentes
SELECT code, inflation_rate, unemployment_rate
FROM economies
WHERE year = 2015
AND code NOT IN
-- Subconsulta que devuelve los code de país filtrados en gov_form
(SELECT code
FROM countries
WHERE (gov_form LIKE '%Republic%' OR gov_form LIKE '%Monarchy%'))
ORDER BY inflation_rate;
```

-- En cities, selecciona el nombre de ciudad, el codigo de pais, la poblacion de la ciudad propiamente dicha y la poblacion del area metropolitana, asi como el campo city_perc, que calcula la población de la ciudad propiamente dicha como porcentaje de la población del área metropolitana para cada ciudad. 
Filtra el nombre de ciudad con una subconsulta que seleccione ciudades capital en countries de 'Europe' o continentes con 'America" al final de su nombre.
Excluye los valores NULL en metroarea_pop
Ordena por city_perc (de mas a menos) y obten solo las 10 primeras filas.

```
SELECT
name
country_code,
city_proper_pop,
metroarea_pop,
(city_proper_pop
FROM cities
-- Usa una subconsulta para filtrar el name de la ciudad
WHERE name IN (
SELECT capital
FROM countries
WHERE continent LIKE 'Europe%' OR continent LIKE '%America%'

-- Agrega una condición de filtro tal que metroarea_pop no tenga valores nulos
AND metroarea_pop IS NOT NULL
-- Ordena por city_perc en orden descendente y limita el resultado
ORDER BY city_perc DESC
LIMIT 10;
```
