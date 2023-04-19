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

<image src="capturas/DFM-Page-1.drawio.png" alt="DFM">

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

    Esta dimensión representa los equipos que participan en los partidos. Se compone de los atributos **nombre**, **ciudad** y **entrenador**. Esta dimensión tiene una doble asociación con el hecho Partido para distinguir entre **equipo local y visitante**. La inclusión de esta dimensión permite analizar el rendimiento de los equipos en distintos partidos y en diferentes ciudades, así como la influencia de los entrenadores en el desempeño del equipo.

- **Dimensión Estadio**

    Esta dimensión representa el lugar donde se lleva a cabo el partido. Se compone del atributo **ubicación** y el atributo descriptivo **nombre**. Con esta dimensión se permite analizar la influencia que el estadio tiene en el rendimiento de los equipos.

- **Dimensión Competición**

    Esta dimensión representa la competición en la que se disputa el partido. Se compone de los atributos **país** y **temporada** y el atributo descriptivo **nombre**. Usar esta dimensión permite analizar el rendimiento de los equipos en distintas competiciones, así como en diferentes países y temporadas.

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

<image src="capturas/DisenioLogico.drawio.png" alt="Diseño Logico">

Para esta tarea se ha utilizado el **modelo en estrella**, el cual, tiene una tabla de hechos central que se conecta a una serie de tablas de dimensiones independientes. Esta estructura simplifica la escritura de consultas y las hace más rápidas y eficientes.

Además, es adecuado para conjuntos de datos que tienen medidas bien definidas y datos relacionales de baja complejidad. En el caso del modelo de fútbol que estoy utilizando, las medidas son claramente definidas (goles, posesión, tarjetas, etc.) y las relaciones entre las tablas son simples y directas.

Además, como no se espera una gran cantidad de cambios en los atributos de las dimensiones, se optó por no implementar Slowly Changing Dimensions (SCDs), lo que simplifica el diseño y la implementación del modelo.

También se decidió utilizar claves primarias subrogadas para cada tabla de dimensión, ya que esto simplifica la gestión de las relaciones entre las tablas y evita posibles problemas de rendimiento al utilizar claves naturales.

Una vez hecho el modelo lógico, creamos las tablas resultantes en Oracle. El script de creación de las tablas es el siguiente:

```sql
-- DROP USER "lucas.lara";

CREATE USER "lucas.lara"
-- IDENTIFIED BY <password>
;

CREATE TABLE "lucas.lara"."DIM_ESTADIO" 
   (	"ID_ESTADIO" NUMBER(*,0) NOT NULL ENABLE, 
	"ESTADIO" VARCHAR2(100) NOT NULL ENABLE, 
	"UBICACION" VARCHAR2(200) NOT NULL ENABLE, 
	"NOMBRE" VARCHAR2(100) NOT NULL ENABLE, 
	 CONSTRAINT "DIM_ESTADIO_PK" PRIMARY KEY ("ID_ESTADIO")
  USING INDEX PCTFREE 10 INITRANS 2 MAXTRANS 255 COMPUTE STATISTICS 
  TABLESPACE "USERS"  ENABLE
   ) SEGMENT CREATION DEFERRED 
  PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 
 NOCOMPRESS LOGGING
  TABLESPACE "USERS" ;

CREATE UNIQUE INDEX "lucas.lara"."DIM_ESTADIO_PK" ON "lucas.lara"."DIM_ESTADIO" ("ID_ESTADIO") 
  PCTFREE 10 INITRANS 2 MAXTRANS 255 COMPUTE STATISTICS 
  TABLESPACE "USERS" ;
;

CREATE TABLE "lucas.lara"."DIM_EQUIPO" 
   (	"ID_EQUIPO" NUMBER(*,0) NOT NULL ENABLE, 
	"EQUIPO" VARCHAR2(100) NOT NULL ENABLE, 
	"NOMBRE" VARCHAR2(100) NOT NULL ENABLE, 
	"CIUDAD" VARCHAR2(100) NOT NULL ENABLE, 
	"ENTRENADOR" VARCHAR2(100) NOT NULL ENABLE, 
	 CONSTRAINT "DIM_EQUIPO_PK" PRIMARY KEY ("ID_EQUIPO")
  USING INDEX PCTFREE 10 INITRANS 2 MAXTRANS 255 
  TABLESPACE "USERS"  ENABLE
   ) SEGMENT CREATION DEFERRED 
  PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 
 NOCOMPRESS LOGGING
  TABLESPACE "USERS" ;

CREATE UNIQUE INDEX "lucas.lara"."DIM_EQUIPO_PK" ON "lucas.lara"."DIM_EQUIPO" ("ID_EQUIPO") 
  PCTFREE 10 INITRANS 2 MAXTRANS 255 
  TABLESPACE "USERS" ;
;

CREATE TABLE "lucas.lara"."DIM_FECHA" 
   (	"ID_FECHA" NUMBER(*,0) NOT NULL ENABLE, 
	"FECHA" VARCHAR2(100) NOT NULL ENABLE, 
	"DIA" VARCHAR2(100) NOT NULL ENABLE, 
	"MES" VARCHAR2(100) NOT NULL ENABLE, 
	"TEMPORADA" VARCHAR2(100) NOT NULL ENABLE, 
	"AÑO" NUMBER(*,0) NOT NULL ENABLE, 
	 CONSTRAINT "DIM_FECHA_PK" PRIMARY KEY ("ID_FECHA")
  USING INDEX PCTFREE 10 INITRANS 2 MAXTRANS 255 
  TABLESPACE "USERS"  ENABLE
   ) SEGMENT CREATION DEFERRED 
  PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 
 NOCOMPRESS LOGGING
  TABLESPACE "USERS" ;

CREATE UNIQUE INDEX "lucas.lara"."DIM_FECHA_PK" ON "lucas.lara"."DIM_FECHA" ("ID_FECHA") 
  PCTFREE 10 INITRANS 2 MAXTRANS 255 
  TABLESPACE "USERS" ;
;

CREATE TABLE "lucas.lara"."DIM_LESION" 
   (	"ID_LESION" NUMBER(*,0) NOT NULL ENABLE, 
	"LESION" VARCHAR2(150) NOT NULL ENABLE, 
	"JUGADOR" VARCHAR2(100) NOT NULL ENABLE, 
	"TIEMPO_RECUPERACION" VARCHAR2(100) NOT NULL ENABLE, 
	 CONSTRAINT "DIM_LESION_PK" PRIMARY KEY ("ID_LESION")
  USING INDEX PCTFREE 10 INITRANS 2 MAXTRANS 255 
  TABLESPACE "USERS"  ENABLE
   ) SEGMENT CREATION DEFERRED 
  PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 
 NOCOMPRESS LOGGING
  TABLESPACE "USERS" ;

CREATE UNIQUE INDEX "lucas.lara"."DIM_LESION_PK" ON "lucas.lara"."DIM_LESION" ("ID_LESION") 
  PCTFREE 10 INITRANS 2 MAXTRANS 255 
  TABLESPACE "USERS" ;
;

CREATE TABLE "lucas.lara"."DIM_COMPETICION" 
   (	"ID_COMPETICION" NUMBER(*,0) NOT NULL ENABLE, 
	"COMPETICION" VARCHAR2(100) NOT NULL ENABLE, 
	"PAIS" VARCHAR2(100) NOT NULL ENABLE, 
	"TEMPORADA" VARCHAR2(100) NOT NULL ENABLE, 
	"NOMBRE" VARCHAR2(100) NOT NULL ENABLE, 
	 CONSTRAINT "DIM_COMPETICION_PK" PRIMARY KEY ("ID_COMPETICION")
  USING INDEX PCTFREE 10 INITRANS 2 MAXTRANS 255 
  TABLESPACE "USERS"  ENABLE
   ) SEGMENT CREATION DEFERRED 
  PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 
 NOCOMPRESS LOGGING
  TABLESPACE "USERS" ;

CREATE UNIQUE INDEX "lucas.lara"."DIM_COMPETICION_PK" ON "lucas.lara"."DIM_COMPETICION" ("ID_COMPETICION") 
  PCTFREE 10 INITRANS 2 MAXTRANS 255 
  TABLESPACE "USERS" ;
;

CREATE TABLE "lucas.lara"."DIM_ARBITRO" 
   (	"ID_ARBITRO" NUMBER(*,0) NOT NULL ENABLE, 
	"ARBITRO" VARCHAR2(100) NOT NULL ENABLE, 
	"NACIONALIDAD" VARCHAR2(100) NOT NULL ENABLE, 
	"NOMBRE" VARCHAR2(100) NOT NULL ENABLE, 
	 CONSTRAINT "DIM_ARBITRO_PK" PRIMARY KEY ("ID_ARBITRO")
  USING INDEX PCTFREE 10 INITRANS 2 MAXTRANS 255 
  TABLESPACE "USERS"  ENABLE
   ) SEGMENT CREATION DEFERRED 
  PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 
 NOCOMPRESS LOGGING
  TABLESPACE "USERS" ;

CREATE UNIQUE INDEX "lucas.lara"."DIM_ARBITRO_PK" ON "lucas.lara"."DIM_ARBITRO" ("ID_ARBITRO") 
  PCTFREE 10 INITRANS 2 MAXTRANS 255 
  TABLESPACE "USERS" ;
;

CREATE TABLE "lucas.lara"."FACT_PARTIDO" 
   (	"ID_FECHA" NUMBER(*,0) NOT NULL ENABLE, 
	"ID_EQUIPOLOCAL" NUMBER(*,0) NOT NULL ENABLE, 
	"ID_EQUIPOVISITANTE" NUMBER(*,0) NOT NULL ENABLE, 
	"ID_ESTADIO" NUMBER(*,0) NOT NULL ENABLE, 
	"ID_LESION" NUMBER(*,0) NOT NULL ENABLE, 
	"ID_COMPETICION" NUMBER(*,0) NOT NULL ENABLE, 
	"ID_ARBITRO" NUMBER(*,0) NOT NULL ENABLE, 
	"GOLESLOCAL" NUMBER(*,0) NOT NULL ENABLE, 
	"GOLESVISITANTE" NUMBER(*,0) NOT NULL ENABLE, 
	"POSESIONLOCAL" VARCHAR2(100) NOT NULL ENABLE, 
	"POSESIONVISITANTE" VARCHAR2(100) NOT NULL ENABLE, 
	"TIROSLOCAL" NUMBER(*,0) NOT NULL ENABLE, 
	"TIROSVISITANTE" NUMBER(*,0) NOT NULL ENABLE, 
	"TARJETASAMARILLASLOCAL" NUMBER(*,0) NOT NULL ENABLE, 
	"TARJETASAMARILLASVISITANTE" NUMBER(*,0) NOT NULL ENABLE, 
	"TARJETASROJASLOCAL" NUMBER(*,0) NOT NULL ENABLE, 
	"TARJETASROJASVISITANTE" NUMBER(*,0) NOT NULL ENABLE, 
	 CONSTRAINT "FACT_PARTIDO_PK" PRIMARY KEY ("ID_FECHA", "ID_EQUIPOLOCAL", "ID_EQUIPOVISITANTE", "ID_ESTADIO", "ID_LESION", "ID_COMPETICION", "ID_ARBITRO")
  USING INDEX PCTFREE 10 INITRANS 2 MAXTRANS 255 
  TABLESPACE "USERS"  ENABLE
   ) SEGMENT CREATION DEFERRED 
  PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 
 NOCOMPRESS LOGGING
  TABLESPACE "USERS" ;

ALTER TABLE "lucas.lara"."FACT_PARTIDO" ADD CONSTRAINT "FK_DIM_ARBITRO" FOREIGN KEY ("ID_ARBITRO")
	  REFERENCES "lucas.lara"."DIM_ARBITRO" ("ID_ARBITRO") ENABLE;
  ALTER TABLE "lucas.lara"."FACT_PARTIDO" ADD CONSTRAINT "FK_DIM_COMPETICION" FOREIGN KEY ("ID_COMPETICION")
	  REFERENCES "lucas.lara"."DIM_COMPETICION" ("ID_COMPETICION") ENABLE;
  ALTER TABLE "lucas.lara"."FACT_PARTIDO" ADD CONSTRAINT "FK_DIM_EQUIPO_LOCAL" FOREIGN KEY ("ID_EQUIPOLOCAL")
	  REFERENCES "lucas.lara"."DIM_EQUIPO" ("ID_EQUIPO") ENABLE;
  ALTER TABLE "lucas.lara"."FACT_PARTIDO" ADD CONSTRAINT "FK_DIM_EQUIPO_VISITANTE" FOREIGN KEY ("ID_EQUIPOVISITANTE")
	  REFERENCES "lucas.lara"."DIM_EQUIPO" ("ID_EQUIPO") ENABLE;
  ALTER TABLE "lucas.lara"."FACT_PARTIDO" ADD CONSTRAINT "FK_DIM_ESTADIO" FOREIGN KEY ("ID_ESTADIO")
	  REFERENCES "lucas.lara"."DIM_ESTADIO" ("ID_ESTADIO") ENABLE;
  ALTER TABLE "lucas.lara"."FACT_PARTIDO" ADD CONSTRAINT "FK_DIM_FECHA" FOREIGN KEY ("ID_FECHA")
	  REFERENCES "lucas.lara"."DIM_FECHA" ("ID_FECHA") ENABLE;
  ALTER TABLE "lucas.lara"."FACT_PARTIDO" ADD CONSTRAINT "FK_DIM_LESION" FOREIGN KEY ("ID_LESION")
	  REFERENCES "lucas.lara"."DIM_LESION" ("ID_LESION") ENABLE;

CREATE UNIQUE INDEX "lucas.lara"."FACT_PARTIDO_PK" ON "lucas.lara"."FACT_PARTIDO" ("ID_FECHA", "ID_EQUIPOLOCAL", "ID_EQUIPOVISITANTE", "ID_ESTADIO", "ID_LESION", "ID_COMPETICION", "ID_ARBITRO") 
  PCTFREE 10 INITRANS 2 MAXTRANS 255 
  TABLESPACE "USERS" ;
;

```

Y las tablas resultantes de Oracle, con sus correspondientes claves primarias y foráneas, son las siguientes:

<image src="capturas/DiagramaOracle.png" alt="Tablas Oracle">

***
### **Tarea 1.3 (0,4 ptos)**

*Plantear y resolver 6 consultas analíticas sobre el almacén de datos diseñado.*
- *Resolver 3 consultas en SQL.*
- *Resolver 3 consultas utilizando MDX.*

**Solución:**  



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