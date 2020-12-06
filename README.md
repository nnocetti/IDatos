# Integración de Datos

## Objetivo

El objetivo que planteamos para el trabajo del curso consiste en integrar varios conjuntos de datos relacionados, publicados de manera desagregada, para verificar la consistencia de los mismos.  
Partimos de la Encuesta Continua de Hogares publicada por el Insituto Nacional de Estadística del Uruguay (INE) y a traves de la misma tratamos de verficiar varios conjuntos de datos publicados por el Ministerio de Desarrollo Social del Uruguay (MIDES), en relación a la tenencia de la vivienda en el Uruguay.

### Objetivos secundarios

Publicar los datos obtenidos como datos abiertos, siguiendo el estándar RDF.

## Fuentes

Algo a destacar de los conjuntos a trabajar, es que todos ellos constan de diccionario de datos, y han sido publicados como datos abiertos. El contar con diccionario de datos es sumamente relevante para entender el dominio y sus entidades.

### MIDES

Los datos a utilizar en el trabajo fueron obtenidos del portal https://catalogodatos.gub.uy/ dentro de la categoría Desarrollo Social. Específicamente relacionan hogares con su situación de tenencia, nivel de ingresos, entre otros. A continuación, detallamos los conjuntos de datos:

#### C1. Relación cuota de compra o cuota alquiler de la vivienda e ingresos del hogar según departamento. Total país.

Descripción de metadatos:

* Relación Compra Alquiler: Describe si la relación es compra o alquiler.  
* Departamento: El departamento, dentro del Uruguay, asociado a la información.  
* Año: El año correspondiente a la información  
* PROMEDIO(RAI/RCI)-ECH: El porcentaje de compra o alquiler para el departamento en el año indicado.  

https://catalogodatos.gub.uy/dataset/mides-indicador-14093 

#### C2. Distribución porcentual de las personas según régimen de tenencia de la vivienda por tramos de edad. Total país.

Descripción de metadatos:

* Edad: Franja de edad en la que se basa la información.  
* Tenencia De Vivienda: Relación del sujeto sobre la vivienda que ocupa.  
* Año: El año correspondiente a la información.  
* PROMEDIO(RAI/RCI)-ECH: Porcentaje de personas en la franja de edad indicada con la tenencia de vivienda indicada en el año indicado.  

https://catalogodatos.gub.uy/dataset/mides-indicador-12533

#### C3. Distribución porcentual de los hogares según régimen de tenencia por tipo de hogar. Total país.

Descripción de metadatos:

* Tipo de Hogar: Composición del núcleo familiar del hogar.  
* Tenencia De Vivienda: Relación del hogar sobre la vivienda que ocupa.  
* Año: El año correspondiente a la información.  
* PROMEDIO(RAI/RCI)-ECH: Porcentaje de hogares con la tenencia de vivienda indicada en el año indicado.  

https://catalogodatos.gub.uy/dataset/mides-indicador-12471 

#### C4. Porcentaje de hogares que destinan más del 30% de sus ingresos al pago de la cuota de compra o de alquiler de la vivienda según quintiles de ingreso per cápita del hogar. Total país. 

Descripción de metadatos:

* Compra_alquiler: Describe si la relación es compra o alquiler.  
* Quintiles: Quintil al cual pertenece el hogar.  
* Año: El año correspondiente a la información.  
* PROMEDIO(RAI/RCI)-ECH: Porcentaje de hogares que destinan más del 30% de sus ingresos al pago de la cuota en el quintil y año indicado.  

https://catalogodatos.gub.uy/dataset/mides-indicador-7787 

#### C5. Distribución porcentual de los hogares según régimen de tenencia por quintiles de ingreso per cápita del hogar. Total país.

Descripción de metadatos: 

* Quintiles: Quintil del hogar.  
* Tenencia De Vivienda: Relación del sujeto sobre la vivienda que ocupa.  
* Año: El año correspondiente a la información.  
* PROMEDIO(RAI/RCI)-ECH: Porcentaje de hogares con la tenencia de vivienda indicada, en el quintil y año indicado.  

https://catalogodatos.gub.uy/dataset/mides-indicador-7809 

### INE - ECH

En principio se buscó relacionar únicamente los conjuntos anteriores, agregando según diferentes atributos y comparando los valores, pero mediante un análisis posterior llegamos a la conclusión de que eso no es posible, dado que los conjuntos ya están agregados.  
Buscando cómo sortear estos problemas intentamos encontrar un conjunto que no tuviera los datos agregados y nos encontramos con la Encuesta Continua de Hogares del INE https://www.ine.gub.uy/web/guest/encuesta-continua-de-hogares1, ECH de ahora en adelante. Estudiamos el diccionario de datos y creemos que basándonos en esta podríamos verificar la mayoría de los conjuntos de datos iniciales.

Así que planteamos tomar como base la Encuesta continua de Hogares del INE e intentar verificar los datos presentados en los conjuntos brindados por el Mides



Un cojunto similar a C2 no se puede obtener desde la ECH porque el conjunto C2 esta basado en personas, no hogares.
Un conjunto similar a C3 no se puede obtener desde la ECH porque el atributo TipoDeHogar no se pude derivar de esta.


Consulta SQL para generar conjunto correspondiente a los datos de C1
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

Consulta SQL para generar conjunto correspondiente a los datos de C4

Calculo de Quintiles:

Para cada ano se calculo el ingreso del hogar per capita y se ordenaron de menor a mayor.
Luego se agruparon en 5 grupos con la misma cantidad de hogares y se obtuvo el menor y mayor valor de cada grupo.


Ano 2012
 Q1 
 Q2 
 Q3 
 Q4 
 Q5 

Ano 2013
Q1 0 - 8841,86
Q2 8841,86 - 13000
Q3 13000 - 18047,91
Q4 18047,91 - 27061,82
Q5 27061,82 - 1090833,34

Ano 2014
Q1 0 - 10245,69
Q2 10245,69 - 14901,5
Q3 14901,5 - 20643
Q4 20643 - 30571
Q5 30571 - 567883,33

Ano 2015
Q1 0 - 11070,1
Q2 11070,1 - 16429
Q3 16429 - 22704
Q4 22704 - 33684
Q5 33684 - 3840590,165

Ano 2016
Q1 0 - 12225,25
Q2 12225,25 - 17990,33
Q3 17990,33 - 24848,67
Q4 24848,67 - 37059,33
Q5 37059,33 - 1244925

Ano 2017
Q1 0 - 13584
Q2 13584 - 19812
Q3 19812 - 27256
Q4 27256 - 40711,33
Q5 40711,33 - 2950268,33

Ano 2018
Q1 0 - 14425
Q2 14425 - 21046
Q3 21046 - 29216,67
Q4 29216,67 - 43649
Q5 43649 - 1876916

Ano 2019
Q1 0 - 15107 TOTAL
Q2 15107 - 22144
Q3 22144 - 30623
Q4 30623 - 45970
Q5 45970 - 1386666,67


SELECT MAX(YSVL), MIN(YSVL)
FROM H_2019_Terceros;

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

```



Ingreso total del hogar / costo 



El Ratio Alquiler Ingreso, RAI, es una
medición del porcentaje del ingreso que
los hogares inquilinos destinan al pago
del alquiler. (rental-to-income ratio)
El Ratio Cuota Ingreso, RCI, es una
medición del porcentaje del ingreso que
destinan al pago de la cuota los hogares
que compraron su vivienda con un
préstamo. (mortgage-to-income ratio)

RDF Repository URL: http://192.168.11.125:7200/repositories/IDatosGroup12