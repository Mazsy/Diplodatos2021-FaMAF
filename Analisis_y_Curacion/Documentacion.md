  ## Características seleccionadas
  ### Características categóricas 
  1. `Type`: tipo de propiedad. 3 valores posibles: `['h', 't', 'u']` correspondientes a `house`, `unidad/duplex`, y `Townhouse` respectivamente.
  2. `Suburb`: Suburbio o Barrio de Melbourne
  3. `Postcode`: Código postal
  4. `CouncilArea`: Municipio
  3. `Regionname`: ubicación por región.

  Todas las características categóricas fueron codificadas con el método OneHotEncoder de sklearn.

  ### Características numéricas
  1. `Rooms`: cantidad de habitaciones.
  2. `Distance`: distancia al centro de la ciudad.
  3. `Bathroom`: cantidad de baños de cada propiedad.
  4. `Review_scores_location` [0-10]: Se agrega un promedio de la valoración de la zona usando el zipcode. Esta nueva variable fue obtenida de publicaciones de la [plataforma AirBnB](https://www.kaggle.com/Tylerx/melbourne-airbnb-open-data?select=cleansed_listings_dec18.csv), e hicimos un merge utilizando el código postal.

## Criterios de exclusión de ejemplos
  1. Las variables `Car` y `Bathroom` fueron filtradas mediante el método de Rango Intercuartil.
  2. Filtramos los valores de `zipcode` del Dataset `AirBnB` de modo de elegir aquellos ejemplos que tenían más de 5 anotaciones (por código postal) y poder hacer un promedio de `Review_scores_location`.

  ## Criterios de exclusión de columnas
  1. Se eliminan  `Address`, `Date` y `SellerG` debido a que son variables poco relevantes al problema.
  2. Consideramos la variable `rooms` en lugar de `Bedroom2` debido a tenía valores más coherentes. 

  ### Transformaciones:

  1. Todas las características numéricas fueron estandarizadas usando `MinMaxScaler` de Sklearn.
  2. Pensando que hay por lo menos un baño en cada propiedad, se imputó con valor `1` a la variable `Bathrooms` (en aquellos ejemplos con datos faltantes).
  3. Los valores faltantes de `Car` fueron imputados con 0 debido a que quizás la omisión de dato es por no tener cochera.
  4. Para `RegionName` agrupamos los ejemplos de todas las subregiones de Virginia en una sola Región (Virginia) debido al bajo número de ejemplos de cada subregión.
  5. Los valores de `Landsize=0` fueron imputados con el valor de `BuildingArea` de la misma propiedad, dado que consideramos más sensato tomar al menos el área construida (en vez de poner cero).

6. Las columnas `YearBuilt` y `BuildingArea` fueron imputadas con `IterativeImputer` utilizando el estimador `KNeighborsRegressor` de sklearn. Este algoritmo se ejecuta de la siguiente forma:

- Calcular la distancia entre el item a clasificar y el resto de items del dataset de entrenamiento.
- Seleccionar los “k” elementos más cercanos (con menor distancia, según la función que se use)
- Realizar una “votación de mayoría” entre los k puntos.

7. Se logró usar fit en PCA, para transformar valores antes realizados.

	
  ### Datos aumentados
  1. Se agregan las 5 primeras columnas obtenidas a través del
     método de PCA, aplicado sobre el conjunto de datos
     totalmente procesado.
