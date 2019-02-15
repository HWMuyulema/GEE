# Introducción a Google Earth Engine (GEE)

## Día 1

Descripción general Google Earth Engine (Componentes de GE. Code Editor, Explorer y Clientes API). Analizaremos un script complejo con un producto final.

Colecciones disponibles. Herramientas de clasificación. Ventajas y limitaciones frente a otras plataformas.

Nociones básicas de Code Editor: Introducción al lenguaje Javascript. Visión General del Code Editor. API Javascript de GEE. Convenciones: Guía de Estilo. Acceso a las funcionalidades de la API. Consultar la documentación de las funciones. Tipo de datos. (Listas, Diccionarios, ). Funciones definidas por el usuario.

### Inicio del tutorial

  * [Manejo de datos vectoriales](https://github.com/HWMuyulema/GEE/blob/master/datos_vectoriales.md): Manejo de geometrías y generación de features. Creación y administración de colecciones de features. Carga y visualización de vectores utilizando Google Fusion Table (FT). Manejo de iteraciones sobre colecciones de features. Exportar como tabla de datos. Realizar gráficos.
  * [Manejo de datos rasters](https://github.com/HWMuyulema/GEE/blob/master/datos_rasters.md): Seleccionar colecciones, filtros por áreas, por fechas y por nubes. Construir máscaras. Visualización. Cómo exportar imágenes (ventajas y limitaciones del servicio). Funciones de agregación. Extracción de información a partir de features (agregación por medias, máximos, mínimos, etc.). Exportar como tabla de datos. Realizar gráficos.
  * [Manejo de datos radar](https://github.com/HWMuyulema/GEE/blob/master/datos_rasters.md): Seleccionar colecciones, filtros por áreas y por fechas. Construir máscaras. Visualización. Cómo exportar imágenes (ventajas y limitaciones del servicio). Funciones de agregación. Cálculos de índices (NDVI, spectral unmixing e indicadores MapBiomas ndfi, por ejemplo, etc.). Generación de expresiones. Extracción de información a partir de features (agregación por medias, máximos, mínimos, etc.). Exportar como tabla de datos. Realizar gráficos.

## Día 2

### Casos de uso: Tutorial

#### Caso 1: Análisi de imágenes radar

Incorporación de datos de campo y generación de datos de entrenamiento desde la interfaz de GE. Imágenes RADAR SAR Sentinel 1 del área de estudio. Muestreo a partir de los datos de campo. Separación en training/testing.

#### Caso 2: Análisis de datos 

Por los trabajos realizados en la dirección el alumno realizar las siguientes actividades:

 1. Generar un conjunto muestreado en la carta de interés.
 2. Construir una colección de imágenes que a su criterio sean apropiadas para las fechas de los relevamientos. Recuerde que las colecciones son parte de objetos ImageCollection y estos contienen las diferentes funcionalidades para realizar filtrados.
 3. Construya un mosaico del área de estudio donde estén incluidas todas las variables (Polaridad, Seleccion, etc.) que ud considere necesarias.
 4. Extraiga información y analicela. Realice gráficos para determinar bandas de mayor separabilidad y umbrales de separación.
 5. Visualizar los resultados.

