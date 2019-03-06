## [Sensores Activos](http://www.un-spider.org/sites/default/files/Presentacion_Microondas.pdf)

Este tipo de sensores están equipados con sistemas de transmisión y recepción que captan la señal reflejada sobre la superficie iluminada.

La utilización de microondas reduce drásticamente el impacto de fenómenos como el humo, las nubes o la lluvia sobre las imágenes resultantes.

De este modo, se puede trabajar en condiciones de falta total de luz y cualquier estado de la meteorología.

|   |   |    |
|---|---|---|
**BANDA** | **FRECUENCIA** | **LONG. DE ONDA**
p | 300 MHz- 1 GHz | 100 - 30 cm
L | 1 - 2 Ghz | 30 - 15 cm
S | 2 - 4 GHz | 15 - 7.5 cm
C | 4- 8 GHz | 7.5 - 3.75 cm
X | 8 - 12.5 GHz | 3.75 - 2.4 cm
kU | 12.5 - 18 GHz | 2.4 - 1.67 cm
k | 18 - 26.5 GHz | 1.67 - 1.1 cm
ka | 26.5 - 40 GHz | 1.1 - 0.75 cm

## pOLARIZACIÓN
Las antenas de los sistemas de radar se pueden configurar para transmitir y recibir radiación electromagnética polarizada ya sea horizontal o verticalmente.
Cuando la energía transmitida es polarizada en la misma dirección que la recibida, al sistema se le conoce como de polarización similar. HH indica que la energía se transmite y se recibe horizontalmente polarizada; VV que la energía se transmite y se recibe verticalmente polarizada.
La reflexión de una onda de radar al chocar en una superficie puede modificar la polarización, dependiendo de las propiedades de la superficie misma.

## [Interferometría RADAR](http://luciovilla.blogspot.com/2017/01/radar-sentinel-1-aplicado-al-monitoreo.html) 


Las imágenes SAR (Radar de Apertura Sintética), funcionan efectivamente haciendo uso de la emisión y recepción de ondas electromagnéticas (OEM). 

En consecuencia, cada píxel que conforma una imagen SAR es resultado de este proceso de emisión de ondas que inciden sobre
una superficie, para luego ser reflejadas en dirección del radar (retrodispersadas), recepcionadas por la anterna del radar, 
registradas y digitalizadas en formato bruto (raw o Nivel 0), para su posterior pretratamiento y generación de una imagen focalizada 
en formato complejo, conocida también como SLC (Single-look-complex); la cual posee información de fase (ɸ) y amplitud (A).

Por lo tanto, al comparar la fase de los píxeles de dos o más imágenes de radar (ello es,  data SAR SLC), registradas en diferentes momentos y puntos del espacio exterior (pertenecientes a ondas electromagnéticas previamente emitidas, retrodispersadas por un mismo punto de una superficie en proceso de deformación y recepcionadas por el radar), aparece un cambio (i.e. cambio de fase: Δɸ), equivalente a la deformación ocurrida en el terreno.

Se debe recalcar que no todos los interferogramas generados presentarán franjas perfectamente definidas, ya que este se ve afectado por factores que condicionan la estabilidad de un píxel en dos imágenes SAR (tomados en diferentes fechas), como es el caso de la presencia de vegetación, la geometría de adquisición, entre otros. 


