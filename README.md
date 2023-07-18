# Seguimiento de brotes de enfermedades a partir de titulares de noticias
En este proyecto se busca analizar cientos de titulares de noticias que contienen información sobre brotes de enfermedades y lugar donde ocurren. monitorear brotes de enfermedades dentro de los Estados Unidos y en el resto del mundo basándose en cientos de titulares de noticias que describen el tipo de enfermedad y el lugar donde ocurre, donde estos titulares son demasiados para poder analizarlos a mano. 

## Descripción General del Proyecto

El objetivo general de este proyecto es monitorear brotes de enfermedades dentro de los Estados Unidos y en el resto del mundo basándose en cientos de titulares de noticias que describen el tipo de enfermedad y el lugar donde ocurre, donde estos titulares son demasiados para poder analizarlos a mano. Esto se hará buscando las ciudades y/o países que se encuentran en los titulares, usando match entre palabras, y separando segun la geolocalización.

## Instalación
- Para clonar este repositorio se puede escribir en la terminal: git clone https://github.com/your-username/project-name.git
- Instalar el requirement.txt con: pip install -r requirements.txt
- Asegurarse que el archivo 'headlines.txt' se encuentra en el directorio correcto.
- Y, correr con: python main.py
## Datos utilizados 

Se utilizará el archivo llamado headlines.txt, que contiene 650 titulares de noticias a analizar. 

## Análisis y preprocesamiento

- **Análisis datos**: Se importan los datos y se comienzan a revisar, se ve que tipo de ciudades y países hay, que datos se repiten y su frecuencia y que tipo de datos son. Además de identificar las ciudades/países de los titulares y separarlos.  
- **Preprocesamiento**: Se limpian los datos, se eliminan los duplicados y usando la base de datos GeonamesCache se encuentran los códigos de los países para poder geolocalizar los brotes de enfermedades.
- **Modelos**: Se crean clusters utilizando DBSCAN y KMeans, además de medidas de distancia,  y técnicas para poder identificar palabras con el fin de hacer match con el nombre de ciudades/países y poder localizarlas.

# Estructura 

Está separado por: 

- **Data**: Esta carpeta contiene los datos sin procesar, es decir, headlines.txt, además de los headlines convertidos en un archivo csv. Aparte de esto, están guardados los datos creados a lo largo del proyecto, dado que mientras se iba trabajando se iban guardando los datos para poder utiizarlos posteriormente.
- **Maps_png**: Esta carpeta contiene imagenes de mapas creados a lo largo del trabajo en formato png, en estas se encuentran los clusters que se fueron analizando.
- **Notebooks**: Aquí se encuentran 4  Jupyter Notebooks que juntos son la totalidad del trabajo. Estos se separaron por: Análisis de datos, procesamiento de las locaciones de los titulares, distribución geográfica de los clusters y la identificación de loos brotes de enfermedades.
- **shapefiles**: En esta carpeta se encuentran archivos necesarios para poder trabajar con la base de datos geográficos.
- **README.md**: Archivo que contiene una descripción general del proyecto, su objetivo principal, el proceso que se llevó a cabo y los resultados del trabajo.
- **requirements.txt**: Este archivo contiene una lista de paquetes y sus versiones correspondientes que son requeridos para que el proyecto funcione correctamente.

#  Desarrollo y Resultados 

Ya utilizando GeonamesCache, se limpiaron los datos y se trabajó con ciudades duplicadas usando la técnica del 'match' que llamamos en el código, donde si hay duplicados, se elije la ciudad con mayor población, dado que, aunque pueda provocar errores en ciertos casos, es más probable que el titular se refiera a la ciudad con mayor población a que la con menor población. 
Se obtuvieron las longitudes y latitudes de cada ciudad y país, para luego hacer un scatter plot y darnos cuenta que los puntos creaban lo que se podía reconocer como un mapa de la Tierra:

![Latitud y longitud](https://github.com/Encinita7/Tracking-disease-outbreaks/assets/126094613/b8e86c8b-d5e7-4792-a9cd-8d852d3efd1f)

Con esto se procede a crear los clusters para poder identificar los brotes de enfermedades. Se crearon clusters con dos modelos distintos: DBSCAN y KMeans y por ende, también se usaron dos formas distintas de medir distancias, método Euclidiano y Great Circle Clusterer. Para poder elegir cual podría ser el modelo elegido, se plotearon los clusters en un Basemap de la Tierra, dado que anteriormente nos dimos cuenta que las latitudes y longitudes lo formaban.
Se obtuvieron los clusters de los titulares con DBSCAN:
![Locations of headlines](https://github.com/Encinita7/Tracking-disease-outbreaks/assets/126094613/b35442e4-5679-47d3-8cab-011d45d7e756)
y con KMeans:
![Locations of headlines KMeans](https://github.com/Encinita7/Tracking-disease-outbreaks/assets/126094613/a7249e32-d56b-4a66-a934-66f3bc95e0c8)
Comparandolos, se ve que el mejor clustering lo hizo DBSCAN, dado que KMeans incorpora en cllusters a titulares que no deberían pertenecer ahí.

Se vió también, que hay muchos titulares que se encuentran en Estados Unidos, por lo que es conveniente separar los datos en Estados Unidos y en el resto del mundo. Se plotean y se obtienen:

![US headlines](https://github.com/Encinita7/Tracking-disease-outbreaks/assets/126094613/426e0dae-6316-42df-9313-707f8864a910)

![World headlines](https://github.com/Encinita7/Tracking-disease-outbreaks/assets/126094613/bc412780-e129-4d5d-a4c9-469363d8cdb1)

Luego de esto, se buscan los centros de los clusters para relacionarlos con los titulares. 
Con esto se buscarán patrones entre los titulares con el fin de identificar los brotes en cada cluster. Viendo el mapa del resto del mundo, se ve que:

![Headlines and outbreaks](https://github.com/Encinita7/Tracking-disease-outbreaks/assets/126094613/e4efed7d-3671-4263-a3ff-4452bca6db7c)

Y en Estados Unidos:

![Brotes Estados Unidos](https://github.com/Encinita7/Tracking-disease-outbreaks/assets/126094613/c3b7eeb0-c690-4d10-91a0-ded2c0657d99)

Quedando claro que el mayor brote de enfermedad en el mundo es del virus Zika:

![Brotes de Zika en el mundo](https://github.com/Encinita7/Tracking-disease-outbreaks/assets/126094613/eb817fbd-5064-4bff-bf44-88f6e4d02898)

Se encontraron:
- 106 titulares en Estados Unidos y América Central.
- 39 titulares en Asia
- 18 titulares en América del Sur
- 9 titulares en India.

# Conclusión del problema sanitario
- Se puede notar que el virus Zika, que es conocido por ser transmitido por un mosquito, tiene brotes donde generalmente se conoce que puede ser cuna de tal virus, lugares húmedos y sombríos, como selva, además, que coincide con lo dicho por la OMS, que este virus se encuentra en América, Asia y África.
- Teniendo estos datos, se les puede dar aviso a las autoridades sanitarias sobre donde se encuentran los mayores brotes, y ahí poder enviar mucha más ayuda para poder terminar con los brotes.

# Conclusión sobre el programa

Para este trabajo en particular fue necesario el trabajar si o si con clustering para poder con identificación de texto, ya que solo identificando las enfermedades y locaciones, podíamos quedar cortos en términos de analizar exactamente donde estaban los brotes.
