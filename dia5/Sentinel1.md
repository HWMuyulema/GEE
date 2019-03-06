# Sentinel-1 – Imágenes RADAR

El satélite sentinel 1 formó parte de la **primera misión espacial Copernico**, Los instrumentos a bordo de la misión Sentinel-1 emplean antenas RADAR SAR (Radar de Apertura Sintica, por sus siglas en inglés) ara el estudio de la superficie terrestre y oceánica. 


Gracias a que los datos RADAR no se ven afectados por las condiciones atmosféricas o por el hecho de ser de día o de noche,   
la monitorización es constante .

El lanzamiento del Sentinel-1A tuvo lugar el 3 de abril de 2014 y el del Sentinel-1B el 25 de abril de 2016.  
Estando a día de hoy ambos operativos, el ciclo de revisita es de 6 días.


|     |     |
|---|---|
**Parámetros** | **Sentinel 1**
Altutud de Órbita | 639km.
Bandas de radar | C-Band
Modo de imagen SAR | Stripmap (GSD:5m,80 km swath), Interferometric Wide Swath Mode(GSD:5x20m, 240 km swath, extra Wide Swath Mode (400km,single-look),Wave Mode (20x5)
Polarización | VV+Vh o HH+HV
Distancia de Muestreo (GSD)| 5 - 20m
Lanzamiento/espectativa de vida | 2014 - 20121
Tiempo Revisita | 12 días



## Los productos que podemos encontrar se dividen por una parte según el tipo de proceso:



Single Look Complex (L-1 SLC): productos SAR de nivel 1 georreferenciados utilizando datos de la órbita y la altitud del satélite.  
Ground Range Detected (L-1 GRD): productos SAR de nivel 1 que han sido proyectados usando un modelo elipsoidal de La Tierra.  


Estos productos podemos encontrarlos además con 3 resoluciones: Full Resolution (FR), High Resolution (HR) y  
Medium Resolution (MR).


Ocean (L-2 OCN): productos oceánicos de nivel 2 con información sobre la velocidad y la dirección del viento. 


## Por otra parte, habrá que tener en cuenta el modo en que se tomen los datos: 



**Stripmap (SM)**:

Que proporcionará datos con una resolución de 5x5m y un ancho de escena máximo 80km.


**Interferometric Wide Swath (IW)**:

Este modo combina un ancho de escena de 250km con una resolución moderada de 5x20m.  
Este es el modo por defecto sobre tierra.  


**Extra-Wide Swath (EW)**:

Este modo se emplea sobre zonas marítimas y polares, donde se necesita una gran cobertura y tiempos  
de revisita cortos. El ancho de escena máximo en este modo es de 400km con una resolución de 20x40m.  

**Wave Mode (WV)**:

Este modo pretende ayudar en la determinación de la dirección y altura de olas en el océano.  
Se compone de imágenes con una resolución de 20x20km que se adquieren alternativamente con dos ángulos de incidencia   
cada 100km, es decir, dos imágenes con el mismo ángulo de incidencia están separadas 200km. 
Además hay que tener en cuenta la polarización. Los productos en modo WV solo estarán disponibles para  
polarización simple (VV o HH) y para los demás modos, SM, IW y EW, estarán disponibles en polarización dual   
(VV+VH o HH+HV) o simple.
