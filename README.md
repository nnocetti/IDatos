# Integración de Datos

## Objetivo

El objetivo que planteamos para el trabajo del curso consiste en integrar varios conjuntos de datos relacionados, publicados de manera desagregada, para verificar la consistencia de los mismos.  
Partimos de la Encuesta Continua de Hogares, ECH de ahora en más, publicada por el Insituto Nacional de Estadística del Uruguay (INE) y a traves de la misma tratamos de verficiar varios conjuntos de datos publicados por el Ministerio de Desarrollo Social del Uruguay (MIDES), en relación a la tenencia de la vivienda en el Uruguay.

### Objetivos secundarios

Publicar los datos obtenidos como datos abiertos, siguiendo el estándar RDF.

## Fuentes

Algo a destacar de los conjuntos a trabajar, es que todos ellos constan de diccionario de datos, y han sido publicados como datos abiertos. El contar con diccionario de datos es sumamente relevante para entender el dominio y sus entidades.

### MIDES

Los datos a utilizar en el trabajo fueron obtenidos del portal https://catalogodatos.gub.uy/ dentro de la categoría Desarrollo Social. Específicamente relacionan hogares con su situación de tenencia, nivel de ingresos, entre otros. A continuación, detallamos los conjuntos de datos:

C1. Relación cuota de compra o cuota alquiler de la vivienda e ingresos del hogar según departamento. Total país.

Descripción de metadatos:

* Relación Compra Alquiler: Describe si la relación es compra o alquiler.  
* Departamento: El departamento, dentro del Uruguay, asociado a la información.  
* Año: El año correspondiente a la información  
* Valor: El porcentaje de compra o alquiler para el departamento en el año indicado.  

https://catalogodatos.gub.uy/dataset/mides-indicador-14093 

C2. Distribución porcentual de las personas según régimen de tenencia de la vivienda por tramos de edad. Total país.

Descripción de metadatos:

* Edad: Franja de edad en la que se basa la información.  
* Tenencia De Vivienda: Relación del sujeto sobre la vivienda que ocupa.  
* Año: El año correspondiente a la información.  
* Valor: Porcentaje de personas en la franja de edad indicada con la tenencia de vivienda indicada en el año indicado.  

https://catalogodatos.gub.uy/dataset/mides-indicador-12533

C3. Distribución porcentual de los hogares según régimen de tenencia por tipo de hogar. Total país.

Descripción de metadatos:

* Tipo de Hogar: Composición del núcleo familiar del hogar.  
* Tenencia De Vivienda: Relación del hogar sobre la vivienda que ocupa.  
* Año: El año correspondiente a la información.  
* Valor: Porcentaje de hogares con la tenencia de vivienda indicada en el año indicado.  

https://catalogodatos.gub.uy/dataset/mides-indicador-12471 

C4. Porcentaje de hogares que destinan más del 30% de sus ingresos al pago de la cuota de compra o de alquiler de la vivienda según quintiles de ingreso per cápita del hogar. Total país. 

Descripción de metadatos:

* Compra_alquiler: Describe si la relación es compra o alquiler.  
* Quintiles: Quintil al cual pertenece el hogar.  
* Año: El año correspondiente a la información.  
* Valor: Porcentaje de hogares que destinan más del 30% de sus ingresos al pago de la cuota en el quintil y año indicado.  

https://catalogodatos.gub.uy/dataset/mides-indicador-7787 

C5. Distribución porcentual de los hogares según régimen de tenencia por quintiles de ingreso per cápita del hogar. Total país.

Descripción de metadatos: 

* Quintiles: Quintil del hogar.  
* Tenencia De Vivienda: Relación del sujeto sobre la vivienda que ocupa.  
* Año: El año correspondiente a la información.  
* Valor: Porcentaje de hogares con la tenencia de vivienda indicada, en el quintil y año indicado.  

https://catalogodatos.gub.uy/dataset/mides-indicador-7809 