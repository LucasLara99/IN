# Inteligencia de Negocio: diseño y explotación 
## Memoria de prácticas
### Lucas Lara García
***

## **Ejercicio 1 (1,2 ptos):**
*Diseñar y explotar un modelo dimensional que recoja información relacionada con el análisis de estadísticas deportivas. El objetivo debe ser un análisis de carácter histórico, relativo por ejemplo a entrentamientos, resultados económicos o deportivos o rendimiento de jugadores a lo largo de la historia.
Las tareas específicas a realizar se detallan a continuación.*
***
### **Tarea 1.1 (0,4 ptos)** 

*Definir de forma más concreta el caso seleccionado y construir un diseño conceptual que refleje las necesidades especificadas.*

**Solución:**  

<image src="capturas/DFM.png" alt="DFM">

El **hecho** principal de este modelo de datos es **Partido**, que representa un evento deportivo, concretamente un encuentro de fútbol, en el que dos equipos compiten entre sí. Un Partido tiene los siguientes atributos:

- **Goles equipo local**: Número de goles marcados por el equipo local en el partido.
- **Goles equipo visitante**: Número de goles marcados por el equipo visitante en el partido.
- **Posesión equipo local**: Porcentaje de tiempo que el equipo local ha tenido la posesión del balón durante el partido.
- **Posesión equipo visitante**: Porcentaje de tiempo que el equipo visitante ha tenido la posesión del balón durante el partido.
- **Tiros equipo local**: Número de tiros realizados por el equipo local durante el partido.
- **Tiros equipo visitante**: Número de tiros realizados por el equipo visitante durante el partido.
- **Tarjetas amarillas local**: Número de tarjetas amarillas mostradas al equipo local durante el partido.
- **Tarjetas amarillas visitante**: Número de tarjetas amarillas mostradas al equipo visitante durante el partido.
- **Tarjetas rojas local**: Número de tarjetas rojas mostradas al equipo local durante el partido.
- **Tarjetas rojas visitante**: Número de tarjetas rojas mostradas al equipo visitante durante el partido.

Estos atributos permiten realizar un análisis detallado de los partidos, y obtener información relevante para el análisis de los equipos y jugadores.  

A continuación se detallan las dimensiones que se han definido para este modelo de datos:

- **Dimensión Fecha**

    Esta dimensión representa la fecha en que se llevó a cabo el partido. Se compone de los atributos **día**, **mes** y **año**, y tiene una jerarquía entre ellos. Además, desde el atributo mes sale un atributo **temporada** que nos permite relacionar los partidos disputados durante una misma. El hecho de que dependa de mes es porque dado un año no podemos asegurar de que temporada se trata, sin embargo, con el mes y el año sí, por ejemplo, marzo de 2023 corresponde a la temporada 22/23 mientras que noviembre de 2023 corresponde a la 23/24. El uso de esta dimensión permite analizar el rendimiento de los equipos en distintos momentos del año y en diferentes temporadas.

- **Dimensión Equipo**

    Esta dimensión representa los equipos que participan en los partidos. Se compone de los atributos **nombre**, **pais** y **entrenador**. Esta dimensión tiene una doble asociación con el hecho Partido para distinguir entre **equipo local y visitante**. La inclusión de esta dimensión permite analizar el rendimiento de los equipos en distintos partidos y en diferentes paises, así como la influencia de los entrenadores en el desempeño del equipo.

- **Dimensión Estadio**

    Esta dimensión representa el lugar donde se lleva a cabo el partido. Se compone del atributo **ubicación** y el atributo descriptivo **nombre**. Con esta dimensión se permite analizar la influencia que el estadio tiene en el rendimiento de los equipos.

- **Dimensión Competición**

    Esta dimensión representa la competición en la que se disputa el partido. Se compone del atributo **país** y el atributo descriptivo **nombre**. Usar esta dimensión permite analizar el rendimiento de los equipos en distintas competiciones, así como en diferentes países y temporadas.

- **Dimensión Árbitro**

    Esta dimensión representa al colegiado encargado de dirigir el partido. Se compone del atributo **nacionalidad** y el atributo descriptivo **nombre**. La dimensión árbitro permite consultar datos y estadísticas como por ejemplo la influencia que el mismo tiene en el resultado del partido, el número de tarjetas que promedia, la cantidad de encuentros arbitrados por temporada, etc.

- **Dimensión Lesión**

    Esta dimensión representa las lesiones que se producen durante el partido. Se compone de los atributos **jugador** y **tiempo de recuperación**. Con esta dimensión se pueden obtener consultas como la influencia que las lesiones tienen en el desempeño del equipo y en el resultado del partido o calcular el número de lesiones por temporada.

Es conveniente hacer una mención especial al atributo **temporada** de la dimensión fecha permite analizar todos los datos del modelo a lo largo del tiempo porque sirve como una forma de agrupar los partidos en períodos temporales más grandes y significativos. Esto es especialmente importante en el contexto del fútbol, ya que los partidos se juegan en temporadas regulares que tienen una duración predefinida y que están claramente separadas entre sí.

Al agregar el atributo temporada a la dimensión fecha, se puede analizar la información de todos los partidos que se jugaron en una temporada específica en lugar de analizar cada partido de forma individual. Esto es útil para obtener información a nivel de temporada, como la cantidad de goles marcados en una temporada, la cantidad de tarjetas rojas recibidas por los equipos en una temporada, entre otros.

Además, el atributo temporada también permite comparar el desempeño de los equipos en diferentes temporadas, lo que puede ayudar a identificar tendencias y patrones a lo largo del tiempo. Por ejemplo, se puede analizar la cantidad de goles marcados por un equipo en una temporada determinada y compararlo con la cantidad de goles marcados en temporadas anteriores o posteriores para ver si hay una tendencia en el desempeño del equipo.

En resumen, el uso de estas dimensiones y atributos permite analizar de forma detallada el rendimiento de los equipos y otros factores que influyen en el resultado de un partido y que tienen lugar a lo largo de una temporada.

***
### **Tarea 1.2 (0,4 ptos)**

*Construir el modelo lógico, implementarlo en Oracle y cargar el esquema en icCube.*  

**Solución:**  

<image src="capturas/ModeloLogico.png" alt="Modelo Logico">

Para esta tarea se ha utilizado el **modelo en estrella**, el cual, tiene una tabla de hechos central que se conecta a una serie de tablas de dimensiones independientes. Esta estructura simplifica la escritura de consultas y las hace más rápidas y eficientes.

Además, es adecuado para conjuntos de datos que tienen medidas bien definidas y datos relacionales de baja complejidad. En el caso del modelo de fútbol que estoy utilizando, las medidas son claramente definidas (goles, posesión, tarjetas, etc.) y las relaciones entre las tablas son simples y directas.

Además, como no se espera una gran cantidad de cambios en los atributos de las dimensiones, se optó por no implementar Slowly Changing Dimensions (SCDs), lo que simplifica el diseño y la implementación del modelo.

También se decidió utilizar claves primarias subrogadas para cada tabla de dimensión, ya que esto simplifica la gestión de las relaciones entre las tablas y evita posibles problemas de rendimiento al utilizar claves naturales.

Una vez hecho el modelo lógico, creamos las tablas resultantes en Oracle. El script de creación de las tablas se puede consultar en el archivo de la entrega ***CreacionTablas.sql***.

Y las tablas resultantes de Oracle, con sus correspondientes claves primarias y foráneas, son las siguientes:

<image src="capturas/Diagrama_Oracle.png" alt="Tablas Oracle">  

Una vez creadas las tablas, el siguiente paso es cargar datos en ellas. A continuación se muestran los scripts de inserción de datos en cada una de las tablas:

**DIM_ARBITRO :**

```sql
INSERT INTO "lucas.lara".DIM_ARBITRO (ID_ARBITRO,ARBITRO,NACIONALIDAD,NOMBRE) VALUES
	 (1,'Carlos Del Cerro Grande - España','España','Carlos Del Cerro Grande'),
	 (2,'Michael Oliver - Inglaterra','Inglaterra','Michael Oliver'),
	 (3,'Sergey Karasev - Rusia','Rusia','Sergey Karasev'),
	 (4,'Felix Zwayer - Alemania','Alemania','Felix Zwayer'),
	 (5,'Jesus Gil Manzano - España','España','Jesus Gil Manzano');
```

**DIM_COMPETICION :**

```sql
INSERT INTO "lucas.lara".DIM_COMPETICION (ID_COMPETICION,COMPETICION,PAIS,NOMBRE) VALUES
	 (1,'Liga Santader - España','España','Liga Santander'),
	 (2,'Serie A - Italia','Italia','Serie A'),
	 (3,'Premier League - Inglaterra','Inglaterra','Premier League'),
	 (4,'Bundesliga - Alemania','Alemania','Bundesliga'),
	 (5,'Ligue One - Francia','Francia','Ligue One');

```  

**DIM_LESION :**

```sql
INSERT INTO "lucas.lara".DIM_LESION (ID_LESION,LESION,JUGADOR,TIEMPO_RECUPERACION) VALUES
	 (1,'Esguince de tobillo - Lionel Messi - 21 días','Lionel Messi','21 días'),
	 (2,'Lesión muscular - Neymar Jr - 10 días','Neymar Jr','10 días'),
	 (3,'Rotura de ligamentos - Eden Hazard - 6 meses','Eden Hazard','6 meses'),
	 (4,'Fractura de pierna - Hector Bellerin - 8 meses','Hector Bellerin','8 meses'),
	 (5,'Esguince de rodilla - Sergio Ramos - 1 mes','Sergio Ramos','1 mes'),
	 (6,'Lesión en la espalda - Kylian Mbappe - 2 semanas','Kylian Mbappe','2 semanas'),
	 (7,'Pubalgia - Luis Suarez - 4 semanas','Luis Suarez','4 semanas'),
	 (8,'Lesión en el hombro - Kevin De Bruyne - 3 semanas','Kevin De Bruyne','3 semanas'),
	 (9,'Lesión en la muñeca - Marco Asensio - 2 semanas','Marco Asensio','2 semanas'),
	 (10,'Distensión de ligamentos - Paulo Dybala - 4 semanas','Paulo Dybala','4 semanas');
```

**DIM_ESTADIO :**

```sql
INSERT INTO "lucas.lara".DIM_ESTADIO (ID_ESTADIO,ESTADIO,UBICACION,NOMBRE) VALUES
	 (1,'Camp Nou - España','España','Camp Nou'),
	 (2,'Santiago Bernabeu - España','España','Santiago Bernabeu'),
	 (3,'Parc des Princes - Francia','Francia','Parc des Princes'),
	 (4,'Estade Velodrome - France','Francia','Estade Velodrome'),
	 (5,'Emirates Stadium - Inglaterra','Inglaterra','Emirates Stadium'),
	 (6,'Old Trafford - Inglaterra','Inglaterra','Old Trafford'),
	 (7,'San Siro - Italia','Italia','San Siro'),
	 (8,'San Siro - Italia','Italia','San Siro'),
	 (9,'Allianz Arena - Alemania','Alemania','Allianz Arena'),
	 (10,'Signal Iduna Park - Alemania','Alemania','Signal Iduna Park');
```

Para la tabla **DIM_FECHA** se han añadido 8 fechas, 2 jornadas por 4 de las últimas temporadas. El script de inserción de datos es el siguiente:

```sql
INSERT INTO "lucas.lara".DIM_FECHA (ID_FECHA,FECHA,DIA,MES,TEMPORADA,AÑO) VALUES
	 (1,'25/01/2020','25','1','2019/2020',2020),
	 (2,'14/03/2020','14','3','2019/2020',2020),
	 (3,'27/02/2021','27','2','2020/2021',2021),
	 (4,'17/04/2021','17','4','2020/2021',2021),
	 (5,'05/03/2022','5','3','2021/2022',2022),
	 (6,'28/05/2022','28','5','2021/2022',2022),
	 (7,'11/02/2023','11','2','2022/2023',2023),
	 (8,'28/05/2023','28','5','2022/2023',2023);

```

En la **DIM_EQUIPO** se han añadido 2 equipos de cada liga, haciendo referencia a los 2 primeros clasificados de cada una de ellas. El script de inserción de datos es el siguiente:

```sql
INSERT INTO "lucas.lara".DIM_EQUIPO (ID_EQUIPO,EQUIPO,NOMBRE,PAIS,ENTRENADOR) VALUES
	 (1,'FC Barcelona - España - Xavi Hernández','Fc Barcelona','España','Xavi Hernández'),
	 (2,'Real Madrid - España - Carlo Ancelotti','Real Madrid','España','Carlo Ancelotti'),
	 (3,'Paris Saint-Germain - Francia - Mauricio Pochettino','Paris Saint-Germain','Francia','Mauricio Pochettino'),
	 (4,'Olympique de Marsella - Francia - Jorge Sampaoli','Olympique de Marsella','Francia','Jorge Sampaoli'),
	 (5,'Manchester City - Inglaterra - Pep Guardiola','Manchester City','Inglaterra','Pep Guardiola'),
	 (6,'Manchester United - Inglaterra - Erik ten Hag','Manchester United','Inglaterra','Erik ten Hag'),
	 (7,'AC Milan - Italia - Stefano Pioli','Ac Milan','Italia','Stefano Pioli'),
	 (8,'Inter Milan - Italia - Simone Inzaghi','Inter Milan','Italia','Simone Inzaghi'),
	 (9,'Bayern Munich - Alemania - Thomas Tuchel','Bayern Munich','Alemania','Thomas Tuchel'),
	 (10,'Borussia Dortmund - Alemania - Marco Rose','Borussia Dortmund','Alemania','Marco Rose');
```

**Importante:** Para la tabla **FACT_PARTIDO** se han añadido 10 partidos para cada temporada, es decir, 2 jornadas distintas para cada competición en cada temporada. Por lo tanto la tabla FACT_PARTIDO tiene 40 filas. Se ha seguido una lógica en la que cada temporada se dan dos partidos de cada liga, concretamente el mismo enfrentamiento, pero en diferentes condiciones, por ejemplo, para la temporada 19/20 podemos ver el partido Barcelona vs Madrid en una fecha y en la otra el Madrid Barcelona. También es conveniente aclarar que cada partido se juega en el estadio del equipo local. El script de inserción de datos es el siguiente:

```sql
INSERT INTO "lucas.lara".FACT_PARTIDO (ID_FECHA,ID_EQUIPOLOCAL,ID_EQUIPOVISITANTE,ID_ESTADIO,ID_LESION,ID_COMPETICION,ID_ARBITRO,GOLESLOCAL,GOLESVISITANTE,POSESIONLOCAL,POSESIONVISITANTE,TIROSLOCAL,TIROSVISITANTE,TARJETASAMARILLASLOCAL,TARJETASAMARILLASVISITANTE,TARJETASROJASLOCAL,TARJETASROJASVISITANTE) VALUES
	 (1,1,2,1,10,1,1,2,0,'71','29',7,5,2,1,0,1),
	 (2,2,1,2,9,1,3,0,0,'52','48',2,0,6,2,0,0),
	 (1,3,4,3,8,2,4,4,3,'62','38',0,1,4,3,0,0),
	 (2,4,3,4,7,2,5,0,1,'39','61',9,7,2,3,1,0),
	 (1,5,6,5,6,3,2,3,1,'55','45',11,12,0,2,0,0),
	 (2,6,5,6,5,3,3,1,0,'80','20',3,4,1,1,0,1),
	 (1,7,8,7,4,4,1,1,1,'28','72',4,3,5,6,0,0),
	 (2,8,7,8,3,4,5,2,2,'64','36',7,10,4,0,2,0),
	 (1,9,10,9,2,5,4,2,3,'57','43',10,0,4,0,0,0),
	 (2,10,9,10,1,5,2,0,0,'36','64',5,1,3,4,0,0);
INSERT INTO "lucas.lara".FACT_PARTIDO (ID_FECHA,ID_EQUIPOLOCAL,ID_EQUIPOVISITANTE,ID_ESTADIO,ID_LESION,ID_COMPETICION,ID_ARBITRO,GOLESLOCAL,GOLESVISITANTE,POSESIONLOCAL,POSESIONVISITANTE,TIROSLOCAL,TIROSVISITANTE,TARJETASAMARILLASLOCAL,TARJETASAMARILLASVISITANTE,TARJETASROJASLOCAL,TARJETASROJASVISITANTE) VALUES
	 (3,1,2,1,9,1,3,1,2,'67','33',1,4,0,2,0,1),
	 (4,2,1,2,5,1,1,1,1,'51','49',14,9,6,5,1,0),
	 (3,3,4,3,1,2,5,2,1,'29','71',4,11,0,3,0,0),
	 (4,4,3,4,2,2,2,5,0,'48','52',8,2,2,4,0,0),
	 (3,5,6,5,3,3,4,3,0,'38','62',2,8,1,2,0,0),
	 (4,6,5,6,4,3,3,0,1,'61','39',6,4,3,1,0,0),
	 (3,7,8,7,5,4,1,2,2,'45','55',5,6,4,5,1,1),
	 (4,8,7,8,6,4,3,0,4,'20','80',0,7,5,6,0,0),
	 (3,9,10,9,7,5,2,1,3,'72','28',7,1,5,2,0,0),
	 (4,10,9,10,8,5,4,6,3,'36','64',12,9,1,0,0,0);
INSERT INTO "lucas.lara".FACT_PARTIDO (ID_FECHA,ID_EQUIPOLOCAL,ID_EQUIPOVISITANTE,ID_ESTADIO,ID_LESION,ID_COMPETICION,ID_ARBITRO,GOLESLOCAL,GOLESVISITANTE,POSESIONLOCAL,POSESIONVISITANTE,TIROSLOCAL,TIROSVISITANTE,TARJETASAMARILLASLOCAL,TARJETASAMARILLASVISITANTE,TARJETASROJASLOCAL,TARJETASROJASVISITANTE) VALUES
	 (5,1,2,1,9,1,5,0,0,'43','57',4,3,0,0,0,1),
	 (6,2,1,2,1,1,3,1,1,'64','36',3,5,4,1,0,0),
	 (5,3,4,3,2,2,5,1,2,'33','67',6,4,2,4,0,0),
	 (6,4,3,4,3,2,1,0,5,'49','51',8,2,0,2,1,0),
	 (5,5,6,5,10,3,4,2,0,'71','29',7,5,2,1,0,1),
	 (6,6,5,6,9,3,2,0,0,'52','48',2,0,6,2,0,0),
	 (5,7,8,7,8,4,5,4,3,'62','38',0,1,4,3,0,0),
	 (6,8,7,8,7,4,4,0,1,'39','61',9,7,2,3,1,0),
	 (5,9,10,9,6,5,3,3,1,'55','45',11,12,0,2,0,0),
	 (6,10,9,10,5,5,1,1,0,'80','20',3,4,1,1,0,1);
INSERT INTO "lucas.lara".FACT_PARTIDO (ID_FECHA,ID_EQUIPOLOCAL,ID_EQUIPOVISITANTE,ID_ESTADIO,ID_LESION,ID_COMPETICION,ID_ARBITRO,GOLESLOCAL,GOLESVISITANTE,POSESIONLOCAL,POSESIONVISITANTE,TIROSLOCAL,TIROSVISITANTE,TARJETASAMARILLASLOCAL,TARJETASAMARILLASVISITANTE,TARJETASROJASLOCAL,TARJETASROJASVISITANTE) VALUES
	 (7,1,2,1,4,1,5,1,1,'28','72',4,3,5,6,0,0),
	 (8,2,1,2,3,1,2,2,2,'64','36',7,10,4,0,2,0),
	 (7,3,4,3,2,2,4,2,3,'57','43',10,0,4,0,0,0),
	 (8,4,3,4,1,2,1,0,0,'36','64',5,1,3,4,0,0),
	 (7,5,6,5,9,3,4,1,2,'67','33',1,4,0,2,0,1),
	 (8,6,5,6,5,3,1,1,1,'51','49',14,9,6,5,1,0),
	 (7,7,8,7,1,4,3,2,1,'29','71',4,11,0,3,0,0),
	 (8,8,7,8,2,4,2,5,0,'48','52',8,2,2,4,0,0),
	 (7,9,10,9,3,5,3,3,0,'38','62',2,8,1,2,0,0),
	 (8,10,9,10,4,5,5,0,1,'61','39',6,4,3,1,0,0);
```

Una vez cargados los datos en la base de datos se procede a cargar el esquema generado en icCube. En primer lugar se configura el datasource con nuestro servidor oracle. En segundo lugar se crean las DataTables. Posteriormente se crean las relaciones entre las DataTables, las dimensiones y las measures. Finalmente se crea el cubo y se cargan los datos en él. El esquema se puede consultar en el archivo ***IN_Lucas.icc-schema*** adjunto.

***
### **Tarea 1.3 (0,4 ptos)**

*Plantear y resolver 6 consultas analíticas sobre el almacén de datos diseñado.*
- *Resolver 3 consultas en SQL.*
- *Resolver 3 consultas utilizando MDX.*

**Solución:**  

He de aclarar antes de la resolución de los ejercicios que algunos de los resultados de ejecutar las consultas no reflejan datos realistas ya que para ello se debería de contar con un dataset que englobase el total de los partidos de todas las temporadas de todas las competiciones. No obstante las consultas son correctas y lógicas en cuanto a su resultado dados los datos disponibles, y si se ejecutasen con un dataset más completo se obtendrían resultados más realistas.

**1ª Consulta SQL**

```sql
SELECT dim_competicion.competicion, dim_fecha.temporada, 
       SUM(fact_partido.tarjetasamarillaslocal) AS amarillas_local, 
       SUM(fact_partido.tarjetasrojaslocal) AS rojas_local, 
       SUM(fact_partido.tarjetasamarillasvisitante) AS amarillas_visitante, 
       SUM(fact_partido.tarjetasrojasvisitante) AS rojas_visitante
FROM fact_partido
INNER JOIN dim_fecha ON fact_partido.id_fecha = dim_fecha.id_fecha
INNER JOIN dim_competicion ON fact_partido.id_competicion = dim_competicion.id_competicion
GROUP BY ROLLUP(dim_competicion.competicion, dim_fecha.temporada)
ORDER BY dim_competicion.competicion, dim_fecha.temporada
```

Esta consulta realiza una consulta utilizando agrupamientos analíticos, en este caso la función ROLLUP, para obtener la cantidad de tarjetas amarillas y rojas recibidas por los equipos locales y visitantes en cada temporada de cada competición. 

Primero, se seleccionan las columnas que se van a mostrar en la consulta: el nombre de la competición, la temporada, la suma de tarjetas amarillas y rojas recibidas por los equipos locales y visitantes. 

Luego, se realizan tres INNER JOIN con las tablas dim_fecha, dim_competicion y fact_partido para obtener la información requerida.

A continuación, se agrupa la información utilizando la función ROLLUP, lo que genera una jerarquía de grupos anidados en los que se muestran los totales para cada nivel de agregación. En este caso, se agrupa primero por la competición y la temporada, y luego se agrega un nivel de total global que incluye todas las competiciones y temporadas.

Por último, se ordena la información por competición y temporada.

El resultado de la consulta es el siguiente:

<image src="capturas/ConsultaSQL1.JPG" alt="Consulta 1 SQL">

***

**2ª Consulta SQL**

```sql
SELECT 
    e.nombre AS equipo,
    SUM(CASE WHEN p.goleslocal > p.golesvisitante AND e.id_equipo = p.id_equipolocal THEN 1 ELSE 0 END) AS victorias_local,
    SUM(CASE WHEN p.goleslocal < p.golesvisitante AND e.id_equipo = p.id_equipovisitante THEN 1 ELSE 0 END) AS victorias_visitante,
    SUM(CASE WHEN p.goleslocal = p.golesvisitante THEN 1 ELSE 0 END) AS empates,
    SUM(CASE WHEN (e.id_equipo = p.id_equipolocal AND p.golesvisitante > p.goleslocal) OR (e.id_equipo = p.id_equipovisitante AND p.golesvisitante < p.goleslocal) THEN 1 ELSE 0 END) AS derrotas
FROM 
    dim_equipo e
    JOIN fact_partido p ON e.id_equipo = p.id_equipolocal OR e.id_equipo = p.id_equipovisitante
GROUP BY 
    e.nombre
ORDER BY 
    victorias_local DESC;
```	

Esta consulta SQL analítica utiliza funciones de agregación condicionales (SUM y CASE) para calcular el número de victorias como local y visitante, empates y derrotas para cada equipo en la tabla dim_equipo.

La consulta se une con la tabla fact_partido usando las columnas id_equipolocal e id_equipovisitante para obtener los partidos en los que cada equipo participó ya sea como local o visitante.

Luego, se usa una función condicional CASE dentro de la función de agregación SUM para contar el número de victorias locales, visitantes, empates y derrotas para cada equipo. Si el equipo es el equipo local y ganó el partido, se suma 1 a victorias_local; si el equipo es el visitante y ganó el partido, se suma 1 a victorias_visitante; si el partido resultó en empate, se suma 1 a empates; si el equipo perdió el partido, se suma 1 a derrotas.

Finalmente, se agrupa el resultado por el nombre del equipo y se ordena en orden descendente según el número de victorias locales, como se muestra en la siguiente imagen:

<image src="capturas/ConsultaSQL2.JPG" alt="Consulta 2 SQL">

***

**3ª Consulta SQL**

```sql
WITH p AS (
  SELECT 
    id_equipolocal AS id_equipo, 
    goleslocal AS goles, 
    golesvisitante AS goles_rival, 
    CASE WHEN goleslocal > golesvisitante THEN 3 
         WHEN goleslocal = golesvisitante THEN 1 
         ELSE 0 END AS puntos
  FROM fact_partido
  UNION ALL
  SELECT 
    id_equipovisitante AS id_equipo, 
    golesvisitante AS goles, 
    goleslocal AS goles_rival, 
    CASE WHEN goleslocal < golesvisitante THEN 3 
         WHEN goleslocal = golesvisitante THEN 1 
         ELSE 0 END AS puntos
  FROM fact_partido
), 
c AS (
  SELECT 
    id_equipo, 
    SUM(goles) AS goles_favor, 
    SUM(goles_rival) AS goles_contra, 
    SUM(puntos) AS puntos, 
    ROW_NUMBER() OVER (ORDER BY SUM(puntos) DESC, SUM(goles) DESC, SUM(goles) - SUM(goles_rival) DESC, id_equipo ASC) AS posicion
  FROM p
  GROUP BY id_equipo
)
SELECT 
  d.nombre AS equipo, 
  c.puntos, 
  c.goles_favor, 
  c.goles_contra, 
  c.goles_favor - c.goles_contra AS diferencia_goles, 
  c.posicion
FROM 
  c 
  JOIN dim_equipo d ON c.id_equipo = d.id_equipo
ORDER BY 
  c.posicion ASC;
```	

Esta consulta realiza una tabla de posiciones de un torneo de fútbol. La consulta utiliza dos expresiones comunes (CTEs) llamadas "p" y "c".

La expresión común "p" utiliza una unión de dos consultas para obtener los goles y los puntos de cada equipo en cada partido. La primera consulta se enfoca en los goles y puntos anotados por el equipo local, mientras que la segunda consulta se enfoca en los goles y puntos anotados por el equipo visitante. La columna "puntos" se calcula utilizando una estructura CASE, que asigna 3 puntos por victoria, 1 punto por empate y 0 puntos por derrota.

La expresión común "c" utiliza la tabla generada por "p" para calcular los goles a favor, los goles en contra, los puntos y la posición de cada equipo en la tabla de posiciones. La función de ventana ROW_NUMBER() se utiliza para asignar una posición a cada equipo en función de su puntuación total, diferencia de goles y ID de equipo.

La consulta final utiliza un JOIN con la tabla de dimensión "dim_equipo" para obtener el nombre del equipo correspondiente a cada ID de equipo y muestra las columnas de puntos, goles a favor, goles en contra, diferencia de goles y posición, ordenadas por la posición. El resultado se muestra en la siguiente imagen:

<image src="capturas/ConsultaSQL3.JPG" alt="Consulta 3 SQL">

***

**1ª Consulta MDX**

```mdx

```

***
## **Ejercicio 2 (0,8 ptos):**

*Construir visualizaciones utilizando herramientas de BI analítico. Se abordará el análisis de plataformas de streaming de películas/series, orientado al análisis y comparación de sus ofertas de contenido.*  

*El trabajo requerirá el uso de dos herramientas diferentes, según se detalla en las tareas a continuación.*

***
### **Tarea 2.1 (0,5 ptos)** 

*Construir visualizaciones en Tableau que permitan comparar dos (o más) plataformas de streaming, Las visualizaciones deben permitir reconocer los aspectos más relevantes de la oferta de contenidos de cada una de las plataformas, y explorar dicha información mediante filtros y otros elementos interactivos. Las visualizaciones deben agruparse en uno o más
dashboards.*

**Solución:**  

***
### **Tarea 2.2 (0,3 ptos)** 

*Construir una visualización específica de las principales características de una de las plataformas. La plataforma a analizar queda a elección de cada grupo.*

*Se valorará que se utilice una herramienta diferente de Tableau para esta tarea. La herramienta recomendada por su simplicidad de uso es Microsoft PowerBI, pero puede utilizarse cualquier herramienta de otros fabricantes (Microstrategy, Klik, SAP, SAS, Sisense...)*

**Solución:**  