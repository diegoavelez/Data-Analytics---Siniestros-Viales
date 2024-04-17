
<p align="center"><img src="https://camo.githubusercontent.com/a8afd26d487e19995da36a49959dc1b8286e07a898e1e27d296fde19b23fb461/68747470733a2f2f7374617469632e6c616a6f726e61646165737461646f64656d657869636f2e636f6d2f77702d636f6e74656e742f75706c6f6164732f323032322f30382f53696e69657374726f732d7669616c65732e6a7067"></p>

<h1 align='center'> Proyecto Individual N°2</h1>

<h2 align='center'> Data Analytics - Siniestros Viales</h2>

<h2 align='center'>Diego Alejandro Vélez, DATAPT07</h2>

Análisis de datos de los accidentes mortales del Observatorio de Movilidad y Seguridad Vial (OMSV) de la Ciudad Autónoma de Buenos Aires (CABA)


## **Tabla de Contenidos**

- [Contexto](#Contexto)
- [Rol desarrollado](#rol-desarrollado)
- [Diccionario de datos](#diccionario-de-datos)
- [Desarrollo del proyecto](#desarrollo-del-proyecto)
    - [Extracción y limpieza](#extracción-y-limpieza-de-datos)
    - [Análisis exploratorio de los datos](#análisis-exploratorio-de-los-datos-eda)
    - [Bases de datos e información adicional](#base-de-datos-e-información-adicional)
    - [Dashboard](#dashboard)
    - [KPIs](#kpis)
    - [Recomendaciones](#recomendaciones)
    - [Conclusiones](#conclusiones)


# Contexto

En Argentina, cada año mueren cerca de 4.000 personas en siniestros viales. Aunque muchas jurisdicciones han logrado disminuir la cantidad de accidentes de tránsito, esta sigue siendo la principal causa de muertes violentas en el país. Los informes del Sistema Nacional de Información Criminal (SNIC), del Ministerio de Seguridad de la Nación, revelan que entre 2018 y 2022 se registraron 19.630 muertes en siniestros viales en todo el país. Estas cifras equivalen a 11 personas por día que resultaron víctimas fatales por accidentes de tránsito.

Solo en 2022, se contabilizaron 3.828 muertes fatales en este tipo de hechos. Los expertos en la materia indican que en Argentina es dos o tres veces más alta la probabilidad de que una persona muera en un siniestro vial que en un hecho de inseguridad delictiva.

# Rol desarrollado

El Observatorio de Movilidad y Seguridad Vial (OMSV), centro de estudios que se encuentra bajo la órbita de la Secretaría de Transporte del Gobierno de la Ciudad Autónoma de Buenos Aires, nos solicita la elaboración de un proyecto de anális de datos, con el fin de generar información que le permita a las autoridades locales tomar medidas para disminuir la cantidad de víctimas fatales de los siniestros viales. 

Para ello, nos disponibilizan un dataset sobre homicidios en siniestros viales acaecidos en la Ciudad de Buenos Aires durante el periodo 2016-2021. Este dataset se encuentra en formato xlsx y contiene dos hojas llamadas: hechos y víctimas. Asimismo, observarán que incluye otras dos hojas adicionales de diccionarios de datos, que les podrá servir de guía para un mayor entendimiento de la data compartida.


# Diccionario de datos

#### Hoja Homicidios

| Variables     | Descripción                      |
|---------------|----------------------------------|
| ID            | Identificador unico del siniestro|
| N_VICTIMAS    | Cantidad de víctimas|
| FECHA         | Fecha en formato dd/mm/aaaa|
| AAAA          | Año |
| MM| Mes|
| DD| Día del mes|
| HORA | Hora del siniestro|
| LUGAR_DEL_HECHO| Dirección del hecho|
| TIPO_DE_CALLE| Tipo de arteria. En el caso de intersecciones a nivel se clasifica según la mayor jerarquía|
| Calle| Nombre de la arteria donde se produjo el hecho|
| Altura| Altura de la arteria donde se produjo el hecho|
| Cruce| Cruce en caso de que sea una encrucijada|
| Dirección Normalizada| Dirección en formato normalizado USIG|
| COMUNA| Comuna de la ciudad (1 a 15)|
| XY (CABA)| Geocodificación plana|
| pos x| Longitud con separador punto. WGS84|
| pos y| Latitud con separador punto. WGS84|
| PARTICIPANTES| Conjunción de víctima y acusado|
| VICTIMA| Vehículo que ocupaba quien haya fallecido a se haya lastimado a raíz del hecho, o bien peatón/a. Clasificación agregada del tipo de vehículos|
| ACUSADO|	Vehículo que ocupaba quien resultó acusado/a del hecho, sin implicar culpabilidad legal|


#### Hoja Victimas

| Variables| Descripción|
|---------------|----------------------------------|
| ID_hecho|	Identificador unico del siniestro|
| FECHA| Fecha en formato dd/mm/aaaa|
| AAAA|	Año|
| MM| Mes|
| DD| Día del mes|
| ROL| Posición relativa al vehículo que presentaba la víctima en el momento del siniestro|
| VICTIMA| Vehículo que ocupaba quien haya fallecido a se haya lastimado a raíz del hecho, o bien peatón/a. Clasificación agregada del tipo de vehículos.|
| SEXO| Sexo informado por fuente policial de la víctima|
| EDAD|	Edad de la víctima al momento del siniestro|
| FECHA_FALLECIMIENTO| Fecha de fallecimiento de la víctima|


# Desarrollo del proyecto

## Extracción y limpieza de los datos

- A partir del archivo homicidios.xlsx se extrajeron las dos hojas relevantes para el proyecto: Hoja Homicidios y Hoja Victimas descritas anteriormente en el diccionario de datos.

Estas dos hojas fueron guardadas en dos datasets correspondientes: homicidios y victimas que fueron posteriormente fusionados en un solo dataset homicidios_con_victimas_df usando la clave ID del dataset homicidios con el ID_hecho de victimas, repitiendo la información del accidente por cada víctima.

Este dataset resultante fue analizado y depurado, dejando solamente las columnas que se consideraron relevantes para el análisis de los accidentes mortales: 'ID','FECHA_x', 'AAAA_x', 'MM_x', 'HORA', 'pos x', 'pos y', 'TIPO_DE_CALLE', 'COMUNA', 'VICTIMA_x', 'ACUSADO', 'N_VICTIMAS', 'ROL', 'SEXO', 'EDAD'.

Se convirtieron los valores de la posición de longitud (pos x) y de latitud (pos y) a valores numéricos y en el caso de valores faltantes se escribió la expresión SD para no eliminar ningún registro, ya que todos los registros son relevantes para el análisis completo. Se hizo de igual forma para los otros valores faltantes en 'Victima', 'Acusado', 'Rol', 'Sexo' y 'Edad'.

Se hizo el ejercicio de renombrar algunas columnas para mejor legibilidad, también se corrigió el campo de Hora con una función.

Se examinaron registros duplicados pero no obtuvimos ninguno.

## Análisis Exploratorio de los datos (EDA)

- Se hicieron estadísticas descriptivas básicas de las columnas numéricas del dataset resultante. En esta encontramos dos registros con la Comuna 0, la cual correspondía a registros del 2016 (año más antiguo) del dataset y fueron inputados a la Comuna 1 que era la Comuna más frecuente en los registros para ese año.

- Se realizó un análisis mensual de los accidentes mortales, encontrando un pico en el diciembre de 2020. Encontrando que las principales causas para el pico de ese mes fueron pasajeros y autos, levantando la primera alerta para estas fechas de fiestas de fin de año que debe ser consideradas.

- Se realizó un análisis de la evolución de los accidentes mortales en las comunas de CABA, donde encontramos que algunas comunas tienen un número significativamente mayor de homicidios como la comuna 1, 3, 4 y 9 en comparación con otras, en el último año.

- Se hizo un análisis de las horas en las que ocurren los accidentes mortales, donde se encontró que las horas donde más se presentan accidentes mortales es durante algunas horas pico de la mañana. Esto corresponde a las horas que comienza la jornada laboral y en algunas comunas al finalizar la jornada alrededor de las 5pm.

- Procediendo con el análisis de tipo de victima, se observó que las principales victimas de los homicidios en siniestros viales son las motos y los peatones, con una diferencia bastante grande con respecto a las otras categorías. 

- Continuando con el análisis de tipo de acusado, se observó que los autos continuan a lo largo del tiempo como los principales acusados de los homicidios en siniestros viales en CABA.

- Se analizaron los accidentes causados por objetos fijos para determinar si tenía alguna concentración geográfica pero no se encontró ninguna concentración de accidentes en ninguna comuna, distribuidos casi uniformemente.

- Se construyó un rango etario para examinar los rangos de edades de las víctimas; encontrando que a pesar de que a lo largo de los años se ve una disminución de las muertes en casi todas las edades, continúa siendo importante la participación de las edades de 18-29, 30-44 y 60+.

- La mayoría de las víctimas son del género masculino (+77%), tendencia que se ha mantenido a lo largo de los años.

- Aunque la mayoría de los casos son masculinos, se puede evidenciar que en las comunas dos y cinco hay una alta participación de las mujeres como víctimas mortales de los siniestros.

- Los siniestros son más comunes en avenidas, indicando una posible correlación entre el volumen de tráfico y la incidencia de accidentes.

- La edad media de las víctimas varía según el tipo de calle, con calles locales mostrando víctimas de mayor edad y autopistas con víctimas más jóvenes, lo que podría reflejar diferentes patrones de uso.

## Base de Datos e información adicional
- Se creó una base de datos en el motor MySQL que se puede consultar en la carpeta SQL>CreaciónBD.mysql con las sentencias de creación de las tablas y la carga de datos en el archivo notebooks>load_data.ipynb.

- Sin embargo, como se creó el dashboard en la herramienta Tableau Public de licencia gratuita, no se pudieron tomar los datos directamente del motor MySQL sino que fueron exportados a archivos csv para su lectura desde Tableau.

## Dashboard

- Se creó un dashboard funcional y coherente con el análisis exploratorio de los datos anteriormente expuesto en la herramienta Tableau Public. El dashboard contiene filtros e interaccciones que permiten explorar detalladamente los datos.

- El proyecto en Tableau contiene una introducción para la presentación de los datos, una descripción de la información suministrada y un contexto.

- El diseño también está implementado de fácil interpretación de la información, donde se cuenta con diseño estético, funcional y es coherente con los gráficos y las variables usadas.

## KPIs
- Se grafican y miden 3 KPIs, representándolos adecuadamente en el dashboard.

- Reducir en un 10% la tasa de homicidios en siniestros viales de los últimos seis meses, en CABA, en comparación con la tasa de homicidios en siniestros viales del semestre anterior.

Se define a la tasa de homicidios en siniestros viales como el número de víctimas fatales en accidentes de tránsito por cada 100,000 habitantes en un área geográfica durante un período de tiempo específico. Su fórmula es: (Número de homicidios en siniestros viales / Población total) * 100,000

Se tomó la información de la población de un conjunto de datos externo, extraído de la Dirección General de Estadística y Censos (Ministerio de Hacienda GCBA). Proyecciones de población.

- Reducir en un 7% la cantidad de accidentes mortales de motociclistas en el último año, en CABA, respecto al año anterior.

Se define a la cantidad de accidentes mortales de motociclistas en siniestros viales como el número absoluto de accidentes fatales en los que estuvieron involucradas víctimas que viajaban en moto en un determinado periodo temporal. Su fórmula para medir la evolución de los accidentes mortales con víctimas en moto es: (Número de accidentes mortales con víctimas en moto en el año actual - Número de accidentes mortales con víctimas en moto en el año anterior) / (Número de accidentes mortales con víctimas en moto en el año anterior) * 100

- (Propuesto) Reducir en un 10% la cantidad de accidentes mortales de peatones en el último año, en CABA, respecto al año anterior.

Se define a la cantidad de accidentes mortales de peatones en siniestros viales como el número absoluto de accidentes fatales en los que estuvieron involucradas víctimas que se movilizaban a pie en un determinado periodo temporal. Su fórmula para medir la evolución de los accidentes mortales con víctimas peatonales es: (Número de accidentes mortales con víctimas peatonales en el año actual - Número de accidentes mortales con víctimas peatonales en el año anterior) / (Número de accidentes mortales con víctimas peatonales en el año anterior) * 100


## Recomendaciones

- Se sugiere un mejor cuidado para el mes de diciembre en los próximos años, donde mediante el diálogo, campañas de sensibilización y seguridad vial con la ciudadanía se pueda evitar un pico nuevamente.

- Por el análisis de comunas, se aconseja revisar el estado la infraestructura de las comunas 1, 3, 4 y 9; sus condiciones de seguridad y factores socioeconómicos que puedan afectar la ocurrencia de accidentes.

- Se recomiendo basado en el análisis de tipo de victima, realizar campañas y acciones orientadas al cuidado de las motos y los peatones, siendo estos los más vulnerables y que tendría gran impacto.

- Como el auto es el tipo de acusado más frecuente se recomiendo continuar desarrollando políticas públicas, campañas de sensibilización, tecnología y nueva información que permitar mitigar el impacto que este tipo de vehículos tiene. 

- El transporte de carga también debe considerarse por el total de accidentes a lo largo de los años, categoría foco de atención y de análisis para mitigar el impacto de este tipo de vehículos. Considerar igualmente que estos vehiculos pueden ser pesados y robustos, lo que incremente la posibilidad de un accidente mortal.

- La mayoría de las acciones deben ser orientadas principalmente al género masculino por su alta representatividad en las víctimas (+77%).

- Incrementar la señalización y medidas de calma de tráfico en avenidas principales donde se concentran los accidentes. Considerar la implementación de más semáforos, señales de stop y pasos peatonales elevados.

- Aplicar medidas de seguridad específicas en las comunas más afectadas, como mejor infraestructura, mayor presencia policial y sistemas de alerta de velocidad.

- Implementar programas de seguridad durante meses y días con alta incidencia de siniestros, que podrían incluir restricciones temporales de velocidad y aumentos en los controles de tráfico.

## Conclusiones

- Se puede concluir por el análisis de horas que la iluminación no es un factor que causa los accidentes mortales, ya que la mayoría de los casos se reportan en las horas de la mañana.

- Las edades en las que debe ser dirigido el diálogo ciudadano y los análisis más profundos corresponde a las edades productivas de 18-29 y 30-44. Con especial atención en los accidentes con adultos mayores 60+ que por análisis exploratorio encontramos tienen bastante riesgo de accidentes.

- El principal género de atención para las campañas de socialización y para el seguimiento de acciones es el masculino. 

- La utilización de análisis de datos para dirigir las intervenciones de movilidad y seguridad puede aumentar significativamente la eficacia de las políticas públicas. Cada recomendación aquí presentada está basada en patrones y correlaciones específicas identificadas a través del análisis de los datos recopilados.

- Integrar herramientas analíticas y tecnológicas para la recopilación y análisis continuo de datos puede ayudar a adaptar rápidamente las estrategias de movilidad a condiciones cambiantes y mejorar los tiempos de respuesta ante emergencias.

- Fomentar la colaboración entre diferentes entidades gubernamentales, organizaciones no gubernamentales y el sector privado puede enriquecer la base de datos con la que trabaja el Observatorio, permitiendo un entendimiento más completo y multifacético de los desafíos de movilidad urbana.


--------


