
Guía de referencia para la práctica de campo
============================================

> Los viajes de campo nos ayudan a ratificar que sabemos poco o nada acerca de la naturaleza

No deberían existir obstáculos para salir al campo porque, a fin de cuentas, es allí donde podemos recoger datos y donde nos surgirán nuevas preguntas. Normalmente, durante una fase de exploración, los planes son inútiles, incluso molestos.

Sin embargo, cuando se trata de responder preguntas concretas, entonces hay que añadir una capa de planificación, que incluya preguntas de investigación y diseño de muestreo, preferiblemente con un enfoque de reproducibilidad (Buej! luego los administrativos te dirán que es más importante hacer 20 solicitudes de cheques, escribir 30 oficios y 10 informes, buscar 50 cotizaciones y cualquier otra vaina con tal de que no viajes; corre por tu vida en el momento que veas más papeles que datos).

Este repo aspira a cumplir con la misión de asesorarte mínimamente a planificar tu recogida de datos, reconociendo en todo caso que tanto el repo como la propia colecta de hormigas/datos son "trabajos en desarrollo".

Enfoques
--------

Basado en las preguntas de cada estudiante, se puede asumir que hay al menos dos enfoques en los estudios a realizar sobre hormigas:

### Relación con el hábitat

Varias personas plantearon preguntas sobre la posible variación en la composición de las comunidades de hormigas respecto de los hábitats del campus de la UASD. No disponemos de mucha información ambiental públicamente accesible sobre los hábitats, más allá de la cobertura estimada a partir de imágenes de satélite. Información ambiental adicional podrá colectarse por cada estudiante en el terreno, mediante los múltiples campos disponibles para ello en los formularios de ODK.

### Nidos

Surgió esta interesante propuesta de estudiar hormigueros. Si a esto se le añade un componente espacial, se podría obtener información interesante sobre patrones espaciales. La única complejidad en este caso consiste en tener que censar, con coordenadas y toma de muestras, cada hormiguero en las parcelas asignadas.

### ¿Acaso hay un tercer enfoque relacionado con cebos?

Parecería que surje un tercero, que es el tema del efecto que pueden tener los diferentes cebos en la preferencia según especies. Sin embargo, no encontré mucha "fuerza" en las preguntas formuladas sobre este tema, y pienso que se pueden agrupar en el enfoque "relación con el hábitat".

Enfoques de trabajo y coberturas
--------------------------------

Las parcelas asignadas a cada persona las obtuve ejecutando los trozos de código que verás a continuación, y que explico paso a paso. Si quieres saltar directamente a la lista de parcelas asignadas, presiona [aquí](#Parcelas-asignadas).

Creé una función para asignar las parcelas, que puedes consultar en [este *script*](parcelalea.R). La asignación se realiza siguiendo un muestreo estratificado-aleatorio. La siguiente línea de código carga la función `parcelalea` a la memoria para su uso posterior.

``` r
source('parcelalea.R')
```

A continuación verás que cargo a memoria el archivo de parcelas 50x50 del campus UASD clasificadas de acuerdo a su cobertura predominante.

``` r
library(sf)
library(tidyverse)
library(kableExtra)
parcelas_uasd <- st_read('c50mpctgrp_para_googlemaps.gpkg')
```

    ## Reading layer `c50mpctgrp_para_googlemaps' from data source `/home/jr/Documentos/clases_UASD/201902/biogeografia/asignaciones/unidad-0-asignacion-4-practica-campo-material-y-odk/c50mpctgrp_para_googlemaps.gpkg' using driver `GPKG'
    ## Simple feature collection with 189 features and 3 fields
    ## geometry type:  POLYGON
    ## dimension:      XY
    ## bbox:           xmin: 402650 ymin: 2041050 xmax: 403650 ymax: 2041900
    ## epsg (SRID):    32619
    ## proj4string:    +proj=utm +zone=19 +datum=WGS84 +units=m +no_defs

![](mapa_quadrats.png)

[Aquí](https://drive.google.com/open?id=171pW12jdkDwmzJuwjocoVjnC7ns&usp=sharing) alojé, en modo público, el mapa estilizado de GoogleMaps que ves arriba. Lo puedes consultar con el navegador desde cualquier PC con conexión a Internet, y también puedes visualizarlo en el campo con el teléfono usando aplicaciones como Maps, o incluso en el navegador de tu dispositivo.

Alternativamente, si prevés que no dispondrás de conexión a Internet en el terreno, puedes descargar el archivo `.kmz` que contiene el diseño de parcelas desde [aquí](quadrats-uasd-50-x-50-m.kmz). Podrás verlo usando tu aplicación preferida de SIG (GIS) en tu movil (o en una tableta o PC). La forma más fácil de cargarlo es mediante [GoogleEarth](https://play.google.com/store/apps/details?id=com.google.earth). Puedes usar también [OruxMaps](www.oruxmaps.com) (no está disponible en la tienda de aplicaciones), o cualquier otra aplicación disponible que permita cargar archivos `.kmz`.

El siguiente bloque de código define qué tipos de coberturas deberá visitar cada persona. Estos fueron definidos en función en función de las preguntas de investigación.

``` r
unique(parcelas_uasd$nombre) #Informativo, tipos de coberturas disponibles
```

    ## [1] construido, mobiliario (bordes edificios, acerado, bancos, postes...)
    ## [2] edificación erguida                                                  
    ## [3] suelo, herbáceas, no edificado ni cubierto                           
    ## [4] dosel                                                                
    ## 4 Levels: construido, mobiliario (bordes edificios, acerado, bancos, postes...) ...

``` r
estfuente <- paste0(
  'https://raw.githubusercontent.com/biogeografia-201902/',
  'miembros-y-colaboradores/master/suscripciones_github.txt'
)#Lista de estudiantes
estudiantes <- readLines(estfuente)
df <- data.frame(usuariogh = gsub(' .*$', '', estudiantes))
df[df$usuario=='BidelkisCastillo','tipos'][[1]] <- list(c('construido', 'suelo'))
df[df$usuario=='dahianagb07','tipos'][[1]] <- list(c('construido', 'suelo'))
df[df$usuario=='emdilone','tipos'][[1]] <- list(c('dosel', 'suelo'))
df[df$usuario=='enrique193','tipos'][[1]] <- list(c('dosel', 'construido'))
df[df$usuario=='jimenezsosa','tipos'][[1]] <- list(c('dosel', 'construido'))
df[df$usuario=='Jorge-Mutonen','tipos'][[1]] <- list(c('dosel', 'suelo'))
df[df$usuario=='Mangoland','tipos'][[1]] <- list(c('construido', 'suelo'))
df[df$usuario=='maritzafg','tipos'][[1]] <- list(c('construido', 'suelo'))
df[df$usuario=='merali-rosario','tipos'][[1]] <- list(c('construido', 'dosel'))
df[df$usuario=='yanderlin','tipos'][[1]] <- list(c('dosel', 'suelo'))
#La semilla ayuda a generar números aleatorios de forma reproducible
df$semilla <- sapply(
  df$usuario,
  function(x)
    gsub('\\D', '', substr(digest::digest(x, algo = 'md5'), 1, 10))
)
```

El siguiente bloque de código asigna enfoques de trabajo ("relación con el hábitat" o "nidos") en función de las preguntas de investigación:

``` r
df[df$usuario=='BidelkisCastillo','enfoque'] <- 'relación con el hábitat'
df[df$usuario=='dahianagb07','enfoque'] <- 'nidos'
df[df$usuario=='emdilone','enfoque'] <- 'relación con el hábitat'
df[df$usuario=='enrique193','enfoque'] <- 'nidos'
df[df$usuario=='jimenezsosa','enfoque'] <- 'relación con el hábitat'
df[df$usuario=='Jorge-Mutonen','enfoque'] <- 'relación con el hábitat'
df[df$usuario=='Mangoland','enfoque'] <- 'relación con el hábitat'
df[df$usuario=='maritzafg','enfoque'] <- 'nidos'
df[df$usuario=='merali-rosario','enfoque'] <- 'relación con el hábitat'
df[df$usuario=='yanderlin','enfoque'] <- 'relación con el hábitat'
```

Coberturas y parcelas asignadas
-------------------------------

El siguiente bloque muestra la tabla por personas, según tipos de coberturas y enfoques (lee a continuación de la tabla para consultar las parcelas que te tocaron).

``` r
kable(df)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
usuariogh
</th>
<th style="text-align:left;">
tipos
</th>
<th style="text-align:left;">
semilla
</th>
<th style="text-align:left;">
enfoque
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
AbigailCP
</td>
<td style="text-align:left;">
NULL
</td>
<td style="text-align:left;">
5437183
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
BidelkisCastillo
</td>
<td style="text-align:left;">
c("construido", "suelo")
</td>
<td style="text-align:left;">
43850
</td>
<td style="text-align:left;">
relación con el hábitat
</td>
</tr>
<tr>
<td style="text-align:left;">
dahianagb07
</td>
<td style="text-align:left;">
c("construido", "suelo")
</td>
<td style="text-align:left;">
60258
</td>
<td style="text-align:left;">
nidos
</td>
</tr>
<tr>
<td style="text-align:left;">
emdilone
</td>
<td style="text-align:left;">
c("dosel", "suelo")
</td>
<td style="text-align:left;">
24920155
</td>
<td style="text-align:left;">
relación con el hábitat
</td>
</tr>
<tr>
<td style="text-align:left;">
enrique193
</td>
<td style="text-align:left;">
c("dosel", "construido")
</td>
<td style="text-align:left;">
064
</td>
<td style="text-align:left;">
nidos
</td>
</tr>
<tr>
<td style="text-align:left;">
Erasbel05
</td>
<td style="text-align:left;">
NULL
</td>
<td style="text-align:left;">
61547643
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
geofis
</td>
<td style="text-align:left;">
NULL
</td>
<td style="text-align:left;">
0839355
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
hoyodepelempito
</td>
<td style="text-align:left;">
NULL
</td>
<td style="text-align:left;">
397
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
jimenezsosa
</td>
<td style="text-align:left;">
c("dosel", "construido")
</td>
<td style="text-align:left;">
383025
</td>
<td style="text-align:left;">
relación con el hábitat
</td>
</tr>
<tr>
<td style="text-align:left;">
Jorge-Mutonen
</td>
<td style="text-align:left;">
c("dosel", "suelo")
</td>
<td style="text-align:left;">
99867
</td>
<td style="text-align:left;">
relación con el hábitat
</td>
</tr>
<tr>
<td style="text-align:left;">
JuanJoseGH06
</td>
<td style="text-align:left;">
NULL
</td>
<td style="text-align:left;">
654858
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
Mangoland
</td>
<td style="text-align:left;">
c("construido", "suelo")
</td>
<td style="text-align:left;">
727149007
</td>
<td style="text-align:left;">
relación con el hábitat
</td>
</tr>
<tr>
<td style="text-align:left;">
maritzafg
</td>
<td style="text-align:left;">
c("construido", "suelo")
</td>
<td style="text-align:left;">
19840786
</td>
<td style="text-align:left;">
nidos
</td>
</tr>
<tr>
<td style="text-align:left;">
merali-rosario
</td>
<td style="text-align:left;">
c("construido", "dosel")
</td>
<td style="text-align:left;">
963659
</td>
<td style="text-align:left;">
relación con el hábitat
</td>
</tr>
<tr>
<td style="text-align:left;">
ramosramos1886
</td>
<td style="text-align:left;">
NULL
</td>
<td style="text-align:left;">
956743
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
sanchez26
</td>
<td style="text-align:left;">
NULL
</td>
<td style="text-align:left;">
80801
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
victorcabsid
</td>
<td style="text-align:left;">
NULL
</td>
<td style="text-align:left;">
703317
</td>
<td style="text-align:left;">
NA
</td>
</tr>
<tr>
<td style="text-align:left;">
yanderlin
</td>
<td style="text-align:left;">
c("dosel", "suelo")
</td>
<td style="text-align:left;">
9444744
</td>
<td style="text-align:left;">
relación con el hábitat
</td>
</tr>
</tbody>
</table>
 

Desde este punto verás la relación de parcelas asignadas por persona. La función `parcelalea` elige, aleatoriamente, un número de parcelas dentro de los tipos de coberturas asignadas. Cada persona dispondrá de más parcelas para elegir que las que les toca muestrear. Así, si en el terreno se presentaran problemas que impidiesen ejecutar el muestreo en una parcela dada, se podrá elegir otra alternativamente.

### BidelkisCastillo. **Relación con el hábitat**

``` r
x <- 'BidelkisCastillo'
parcelalea(
  estudiante = x,
  tipos = df[df$usuario==x,'tipos'][[1]],
  semilla = df[df$usuario==x,'semilla']
)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
Tipo de cobertura
</th>
<th style="text-align:left;">
Parcelas
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
construido, mobiliario (bordes edificios, acerado, bancos, postes...)
</td>
<td style="text-align:left;">
Elegir al menos 6 de éstas: 3, 8, 53, 78, 95, 105, 106, 152, 169, 178, 187
</td>
</tr>
<tr>
<td style="text-align:left;">
suelo, herbáceas, no edificado ni cubierto
</td>
<td style="text-align:left;">
Elegir al menos 5 de éstas: 16, 27, 37, 39, 40, 122, 159, 160, 182
</td>
</tr>
</tbody>
</table>
 

### dahianagb07. **Nidos**

``` r
x <- 'dahianagb07'
parcelalea(
  estudiante = x,
  tipos = df[df$usuario==x,'tipos'][[1]],
  semilla = df[df$usuario==x,'semilla']
)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
Tipo de cobertura
</th>
<th style="text-align:left;">
Parcelas
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
construido, mobiliario (bordes edificios, acerado, bancos, postes...)
</td>
<td style="text-align:left;">
Elegir al menos 6 de éstas: 1, 53, 78, 89, 104, 110, 127, 130, 146, 147, 163
</td>
</tr>
<tr>
<td style="text-align:left;">
suelo, herbáceas, no edificado ni cubierto
</td>
<td style="text-align:left;">
Elegir al menos 5 de éstas: 10, 25, 27, 30, 38, 42, 66, 70, 143
</td>
</tr>
</tbody>
</table>
 

### emdilone. **Relación con el hábitat**

``` r
x <- 'emdilone'
parcelalea(
  estudiante = x,
  tipos = df[df$usuario==x,'tipos'][[1]],
  semilla = df[df$usuario==x,'semilla']
)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
Tipo de cobertura
</th>
<th style="text-align:left;">
Parcelas
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
suelo, herbáceas, no edificado ni cubierto
</td>
<td style="text-align:left;">
Elegir al menos 6 de éstas: 10, 17, 28, 40, 42, 52, 107, 160, 168, 171, 182
</td>
</tr>
<tr>
<td style="text-align:left;">
dosel
</td>
<td style="text-align:left;">
Elegir al menos 5 de éstas: 24, 32, 50, 88, 97, 98, 134, 155, 174
</td>
</tr>
</tbody>
</table>
 

### enrique193. **Nidos**. Muestreo por conveniencia

``` r
x <- 'enrique193'
enrtiposcob <- c(
  'próximas a sitios de comida',
  'alejadas de sitios de comida'
)
enrparcelas <- c(
  paste('Elegir al menos 6 de éstas:', paste(c(10, 18, 21, 22, 42, 50, 51, 79, 166, 167, 151), collapse = ', ')),
  paste('Elegir al menos 5 de éstas:', paste(c(68, 77, 81, 86, 109, 159, 170), collapse = ', '))
)
kable(
  data.frame(
    `Tipo de cobertura`= enrtiposcob,
    Parcelas = enrparcelas,
    check.names = F)
)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
Tipo de cobertura
</th>
<th style="text-align:left;">
Parcelas
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
próximas a sitios de comida
</td>
<td style="text-align:left;">
Elegir al menos 6 de éstas: 10, 18, 21, 22, 42, 50, 51, 79, 166, 167, 151
</td>
</tr>
<tr>
<td style="text-align:left;">
alejadas de sitios de comida
</td>
<td style="text-align:left;">
Elegir al menos 5 de éstas: 68, 77, 81, 86, 109, 159, 170
</td>
</tr>
</tbody>
</table>
 

### jimenezsosa. **Relación con el hábitat**

``` r
x <- 'jimenezsosa'
parcelalea(
  estudiante = x,
  tipos = df[df$usuario==x,'tipos'][[1]],
  semilla = df[df$usuario==x,'semilla']
)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
Tipo de cobertura
</th>
<th style="text-align:left;">
Parcelas
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
construido, mobiliario (bordes edificios, acerado, bancos, postes...)
</td>
<td style="text-align:left;">
Elegir al menos 6 de éstas: 2, 8, 9, 53, 106, 111, 128, 130, 157, 178, 187
</td>
</tr>
<tr>
<td style="text-align:left;">
dosel
</td>
<td style="text-align:left;">
Elegir al menos 5 de éstas: 31, 33, 43, 88, 97, 112, 114, 164, 174
</td>
</tr>
</tbody>
</table>
 

### Jorge-Mutonen. **Relación con el hábitat**

``` r
x <- 'Jorge-Mutonen'
parcelalea(
  estudiante = x,
  tipos = df[df$usuario==x,'tipos'][[1]],
  semilla = df[df$usuario==x,'semilla']
)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
Tipo de cobertura
</th>
<th style="text-align:left;">
Parcelas
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
suelo, herbáceas, no edificado ni cubierto
</td>
<td style="text-align:left;">
Elegir al menos 6 de éstas: 26, 27, 28, 29, 41, 64, 125, 158, 170, 171, 182
</td>
</tr>
<tr>
<td style="text-align:left;">
dosel
</td>
<td style="text-align:left;">
Elegir al menos 5 de éstas: 44, 49, 50, 54, 88, 98, 135, 174, 175
</td>
</tr>
</tbody>
</table>
 

### Mangoland. **Relación con el hábitat**

``` r
x <- 'Mangoland'
parcelalea(
  estudiante = x,
  tipos = df[df$usuario==x,'tipos'][[1]],
  semilla = df[df$usuario==x,'semilla']
)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
Tipo de cobertura
</th>
<th style="text-align:left;">
Parcelas
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
construido, mobiliario (bordes edificios, acerado, bancos, postes...)
</td>
<td style="text-align:left;">
Elegir al menos 6 de éstas: 3, 8, 85, 104, 106, 128, 140, 148, 156, 178, 183
</td>
</tr>
<tr>
<td style="text-align:left;">
suelo, herbáceas, no edificado ni cubierto
</td>
<td style="text-align:left;">
Elegir al menos 5 de éstas: 16, 17, 18, 29, 40, 66, 86, 107, 122
</td>
</tr>
</tbody>
</table>
 

### maritzafg. **Nidos**

``` r
x <- 'maritzafg'
parcelalea(
  estudiante = x,
  tipos = df[df$usuario==x,'tipos'][[1]],
  semilla = df[df$usuario==x,'semilla']
)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
Tipo de cobertura
</th>
<th style="text-align:left;">
Parcelas
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
construido, mobiliario (bordes edificios, acerado, bancos, postes...)
</td>
<td style="text-align:left;">
Elegir al menos 6 de éstas: 53, 78, 85, 105, 106, 128, 131, 140, 157, 178, 179
</td>
</tr>
<tr>
<td style="text-align:left;">
suelo, herbáceas, no edificado ni cubierto
</td>
<td style="text-align:left;">
Elegir al menos 5 de éstas: 25, 27, 28, 39, 66, 80, 143, 160, 168
</td>
</tr>
</tbody>
</table>
 

### merali-rosario. **Relación con el hábitat**

``` r
x <- 'merali-rosario'
parcelalea(
  estudiante = x,
  tipos = df[df$usuario==x,'tipos'][[1]],
  semilla = df[df$usuario==x,'semilla']
)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
Tipo de cobertura
</th>
<th style="text-align:left;">
Parcelas
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
construido, mobiliario (bordes edificios, acerado, bancos, postes...)
</td>
<td style="text-align:left;">
Elegir al menos 6 de éstas: 2, 9, 78, 79, 89, 104, 129, 147, 152, 157, 179
</td>
</tr>
<tr>
<td style="text-align:left;">
dosel
</td>
<td style="text-align:left;">
Elegir al menos 5 de éstas: 24, 43, 49, 54, 133, 135, 149, 150, 164
</td>
</tr>
</tbody>
</table>
 

### yanderlin. **Relación con el hábitat**

``` r
x <- 'yanderlin'
parcelalea(
  estudiante = x,
  tipos = df[df$usuario==x,'tipos'][[1]],
  semilla = df[df$usuario==x,'semilla']
)
```

<table>
<thead>
<tr>
<th style="text-align:left;">
Tipo de cobertura
</th>
<th style="text-align:left;">
Parcelas
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;">
suelo, herbáceas, no edificado ni cubierto
</td>
<td style="text-align:left;">
Elegir al menos 6 de éstas: 19, 26, 28, 29, 30, 42, 64, 67, 82, 159, 160
</td>
</tr>
<tr>
<td style="text-align:left;">
dosel
</td>
<td style="text-align:left;">
Elegir al menos 5 de éstas: 43, 50, 54, 93, 98, 113, 132, 150, 174
</td>
</tr>
</tbody>
</table>
 

Utiliza las siguientes tareas a modo de listas de "chequeo". Confirma que dispones de lo necesario para iniciar la colecta de especímenes/datos.

Tarea 1. Descarga y prueba tu formulario de campo de ODK
--------------------------------------------------------

**EXTREMADAMENTE IMPORTANTE. Si conservas aún el formulario que descargaste en el aula, bórralo, porque colectarás con uno ligeramente modificado. El servidor no admite remisiones del formulario anterior.**

El servidor dispone de dos formularios disponibles para el trabajo de campo según el enfoque que te corresponda. El formulario del enfoque "relación con el hábitat" está almacenado bajo el nombre **"Hormigas UASD HABITAT"**. El formulario del enfoque "nidos" se denomina **"Hormigas UASD NIDOS"**. Procede de la siguiente manera:

-   Descárgalo. Por razones de seguridad, no puedo compartir los detalles de descarga por esta vía. Crea un *issue* o escribe a mi buzón de correo.

-   Prueba. Rellena un formulario ficticio. Coloca en el campo "Observaciones u otros parámetros" la palabra "PROBANDO".

-   Envíalo. Puedes enviarlo al servidor para comprobar que la comunicación está correcta. Notifícame, mediante un *issue*.

Tarea 3. Ejecuta el siguiente protocolo de recogida de datos según sea tu caso
------------------------------------------------------------------------------

### Relación con el hábitat

Este protocolo se realiza de forma más eficiente por dos personas.

Si trabajas según el enfoque "relación con el hábitat", usarás cebos como método de muestreo por razones logísticas. Inicia el trabajo abriendo tu formulario de campo ODK **"Hormigas UASD HABITAT"**. Rellena todos los datos que puedas al inicio y **GUARDA FRECUENTEMENTE**.

En el terreno, coloca 16 cebos separados dos metros entre sí (3-4 pasos son aproximadamente dos metros), formando un cuadrado (si son dos personas, el trabajo de rellenado de formularios y colocación de cebos se puede hacer en paralelo). Puedes colocar estacas en el terreno para marcar los vértices más importantes del cuadrado, o colocar estacas en todos los cebos.

Todos los cebos de una parcela deben quedar dentro del tipo de cobertura que corresponda al caso. A continuación pongo un ejemplo sobre este particular.

> Ejemplo. BidelkisCastillo debe elegir una parcela, dentro de la cobertura "construido, acerado, etc.". Supongamos que elige la parcela 53, que tiene mayoritariamente dicha "cobertura", pero también tiene algunos (pocos) árboles y edificaciones erguidas (que no es lo mismo que el acerado o el pavimento). En este ejemplo, BidelkisCastillo debe colocar sus cebos en algún lugar con acerado o pavimento dentro de la parcela (preferiblemente, y si se puede, cerca del centro), pero no bajo dosel, ni dentro de edificaciones ni sobre suelo (con o sin herbáceas). Si por alguna razón la cobertura predominante no está presente o no es dominante en la parcela (porque haya cambiado, por error de cartografía, o simplemente porque se hace imposible muestrear), debe elegirse, de la lista de parcelas asignadas, otra que caracterice correctamente a dicha cobertura.

Toma la hora a la que colocaste el primer cebo y espera 30 minutos para iniciar la colecta de hormigas. Cuando se acerque el tiempo de colectar, por ejemplo, a los 25 minutos, una de las personas del equipo puede rellenar en el formulario ODK, los datos sobre "actividad en cebos", en cuatro cebos (de los 16) elegidos al azar (admite que sólo uno de los cuatro caiga en borde).

Pasados 30 minutos, **colecta las hormigas de los 16 cebos de una misma parcela en un único frasco con alcohol** al 70-80% usando el pincel; puedes usar una pinza si tienes la suficiente habilidad para ello. Para colectar, visita los cebos en el mismo orden en el que los colocaste; primero se retiran las hormigas del primer cebo colocado, luego las del segundo, y así sucesivamente hasta llegar al número 16.

Puedes elegir una convención para decidir cuánto colectar en cada cebo: por ejemplo, 5 minutos colectando o hasta que reúnas todos los morfotipos que puedas distinguir a simple vista. Una luz frontal puede servirte en lugares desafiantes, como por ejemplo, en áreas con grama o bajo dosel (poca luz).

Dado que tendrás que muestrear 11 parcelas, al finalizar tu trabajo de campo deberás tener 11 formularios de campo y 11 frascos.

### Nidos

Si trabajas con "nidos", deberás hacer un censo lo más detallado posible de éstos en cada parcela, pero sólo en el tipo predominante de cobertura que corresponda. Pongo un ejemplo: dahianagb07 debe elegir una parcela dentro de la cobertura "suelo, herbáceas...". Supongamos que elige la parcela 10, donde está parte del *play*. dahianagb07 debe censar todos los nidos sobre suelo, pero no sobre pavimento ni acerado, ni dentro de edificaciones. Al igual que en el caso anterior, si por alguna razón la cobertura predominante no está en la parcela, debe elegirse, de la lista de parcelas asignadas, otra que caracterice correctamente a dicha cobertura.

Deberás tomar las coordenadas de cada nido, una mínima información ambiental, y colectarás de cinco a diez obreras en cada uno, preferiblemente de diferentes castas.

Tarea 3. Revisa tu material de campo
------------------------------------

-   Frascos. El número de frascos dependerá mucho del enfoque que hayas elegido.
-   Alcohol etílico al 70-80%.
-   Pincel
-   Cebo si trabajas en el enfoque "relación con el hábitat".
-   Dispositivo Android + ODK Collect + Formularios descargados). Alternativamente, puedes usar formularios en papel

Tarea 4. Sal al "campo"
-----------------------

-   Seguridad

Tarea final. Ajusta tus expectativas
------------------------------------

-   Es probable que, aun teniendo preguntas de investigación debidamente formuladas y un diseño de muestreo acorde a lo que quieres responder, te encuentres al final de los análisis con lo que a veces se denomina resultados negativos. Significa que podrías no encontrar un efecto, un patrón, y lo importante es que debes interpretarlo adecuadamente. Un resultado negativo es también un resultado, porque responde a unas preguntas y probablemente abre otras.

-   No se trata de que hagas un completo estudio sobre hormigas, sino que ensayes técnicas de formulación de preguntas, y de recogida y análisis de datos, pensando en aplicarlo a algo más grande en el futuro. Tu tesis será un buen terreno para poner en práctica dichas técnicas.

-   Finalmente, reconoce la limitación temporal. En 5 meses no podrás ver ni aplicar todas las técnicas biogeográficas que existen en el "mercado". Sin embargo, dispondrás de fuentes bibliográficas, nuevos recursos y nuevas herramientas para localizar soluciones a tus futuros problemas en ecología.

Bibliografía de referencia brevemente comentada
-----------------------------------------------

Referencias
-----------
