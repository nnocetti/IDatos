# Integración de Datos

## Objetivo

El objetivo que planteamos consiste en verificar la consistencia del conjunto "Relación cuota de compra o cuota alquiler de la vivienda e ingresos del hogar según departamento. Total país."  
Partimos de la Encuesta Continua de Hogares publicada por el Insituto Nacional de Estadística del Uruguay (INE) y a traves de la misma tratamos de verficiar el conjunto de datos publicado por el Ministerio de Desarrollo Social del Uruguay (MIDES), en relación a la tenencia de la vivienda en el Uruguay.

### Objetivos secundarios

Publicar los datos como datos abiertos, siguiendo el estándar RDF.

## Fuentes

Algo a destacar de los conjuntos a trabajar, es que todos ellos constan de diccionario de datos, y han sido publicados como datos abiertos, utilizando la licencia DAG de Uruguay.  
https://www.gub.uy/agencia-gobierno-electronico-sociedad-informacion-conocimiento/comunicacion/publicaciones/licencia-datos-abiertos-uruguay-0  
El contar con diccionario de datos es sumamente relevante para entender el dominio y sus entidades.

### MIDES

Los datos a utilizar en el trabajo fueron obtenidos del portal https://catalogodatos.gub.uy/ dentro de la categoría Desarrollo Social. Específicamente relacionan hogares con su situación de tenencia, nivel de ingresos, entre otros. A continuación, detallamos el conjunto de datos:

#### C1. Relación cuota de compra o cuota alquiler de la vivienda e ingresos del hogar según departamento. Total país.

Descripción de metadatos:

* Relación Compra Alquiler: Describe si la relación es compra o alquiler.  
* Departamento: El departamento, dentro del Uruguay, asociado a la información.  
* Año: El año correspondiente a la información  
* Valor: El porcentaje de compra o alquiler para el departamento en el año indicado.  

https://catalogodatos.gub.uy/dataset/mides-indicador-14093 


El valor que se presenta en el conjunto de datos corresponde al promedio de RAI o RCI, dependiendo si la relación del hogar con la vivienda es de alguiler o compra, para un departamento en un año dado.  
El Ratio Alquiler Ingreso, RAI, es una medición del porcentaje del ingreso que los hogares inquilinos destinan al pago del alquiler. (rental-to-income ratio)  
El Ratio Cuota Ingreso, RCI, es una medición del porcentaje del ingreso que destinan al pago de la cuota los hogares que compraron su vivienda con un préstamo. (mortgage-to-income ratio)

### INE - ECH

Planteamos tomar como base la Encuesta continua de Hogares del INE https://www.ine.gub.uy/web/guest/encuesta-continua-de-hogares1, ECH de ahora en adelante e intentar verificar los datos presentados en el conjunto C1 realizado por el Mides.

### Trabajo

Integramos las ECH de los diferentes años en una unica base de datos y realizamos la consulta SQL qye se indica en el anexo para obtener el RAI/RCI en cada caso.  

Luego integramos los datos obtenidos en la consulta con los del conjunto C1 y calculamos la diferencia entre el RAI/RCI obtenido por el MIDES y el RAI/RCI calculado por nosotros a traves de la ECH utilizando la siguiente formula de porcentaje de diferencia:  

(| valor nuevo - valor antiguo |)/(| valor antiguo |)*100

Finalmente calculamos el promedio del procentaje de la diferencia, para todo el conjunto de datos obtuvimos:  
6.11%

#### Objetivo Secundario

Planteamos publicar los datos del MIDES en formato RDF, organizandolos en un grafo rdf como se muestra en la siguiente figura.  

![Alt text](/RDF-diagram.png?raw=true "Title")

En el diagrama el blank node _:registroRAI respresenta una sentencia que indica que para un departamente el promedio de RAI es un valor literal, y esa sentencia se realiza en un determinado año.
Similarmente el blank node _:registroRCI respresenta una sentencia que indica que para un departamente el promedio de RCI es un valor literal, y esa sentencia se realiza en un determinado año.

La definicion el conjunto RDF se encuentra en el archivo Promedio_RAI_RCI.nt en formato N-Triples

### Anexo

Consulta SQL
```sql
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2019 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2019_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2019 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2019_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2018 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2018_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2018 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2018_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2017 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2017_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2017 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2017_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2016 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2016_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2016 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2016_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2015 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2015_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2015 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2015_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2014 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2014_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2014 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2014_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2013 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2013_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2013 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2013_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2012 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2012_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2012 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2012_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2011 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2011_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2011 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2011_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2010 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2010_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2010 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2010_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2009 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2009_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2009 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2009_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2008 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2008_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2008 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2008_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2007 AS Año, AVG((D8_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2007_Terceros
WHERE D8_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2007 AS Año, AVG((D8_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2007_Terceros
WHERE (D8_1=1 OR D8_1=3) AND (YSVL <> 0)
GROUP BY Departamento;

UNION
SELECT 'Alquiler' AS RelaciónCompraAlquiler, Departamento, 2006 AS Año, AVG((D7_3/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2006_Terceros
WHERE D7_1=5 AND YSVL <> 0
GROUP BY Departamento
UNION
SELECT 'Compra' AS RelaciónCompraAlquiler, Departamento, 2006 AS Año, AVG((D7_2/YSVL)*100) AS PROMEDIO(RAI/RCI)-ECH
FROM H_2006_Terceros
WHERE (D7_1=1 OR D7_1=3) AND (YSVL <> 0)
GROUP BY Departamento;
```
