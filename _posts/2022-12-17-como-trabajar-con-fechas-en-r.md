---
title: ¿Cómo trabajar con fechas en R?
date: 2022-12-17 10:10:00 -0500
categories: [R]
tags: [fechas, datetimes, lubridate, r]
image: 
  path: /posts/2022-12-17-como-trabajar-con-fechas-en-r/hero.png
excerpt: Aprende a usar lubridate.
---

Saber trabajar correctamente con fechas es una herramienta fundamental para cualquier científico de datos, ya que es muy común encontrarse con columnas de este tipo en los *datasets*. Sin embargo, como lo explico en [esta publicación](https://www.camartinezbu.com/posts/3-razones-que-dificultan-el-trabajo-con-fechas/), no es algo trivial dadas algunas complejidades propias de las fechas.

Al final de este post, sabrás como leer, manipular y hacer aritmética con fechas usando el paquete [lubridate](https://lubridate.tidyverse.org/index.html). Si te interesa aprender a trabajar con fechas en Python, te invito s leer los posts correspondientes a [`datetime`](https://www.camartinezbu.com/posts/como-trabajar-con-fechas-en-python/) y [`pandas`](https://www.camartinezbu.com/posts/como-trabajar-con-fechas-en-pandas/).

## Tipos de datos primitivos en R

Antes de empezar, es útil hacer un repaso de los tipos de datos que utiliza R base para trabajar con fechas. Lubridate crea un tipo de objeto nuevo, sino que te ofrece un conjunto de funciones para trabajar con ellos.

Dependiendo de qué tanta información contiene tu campo de fecha, vas a trabajar con distintos tipos de objetos en R. Cuando sólo se tiene año, mes y día, R trabaja con el tipo de dato `Date`. Internamente, este consiste en un conteo de los días que han pasado desde el primero de enero de 1970.

Puedes leer una fecha desde un `character` usando la función `as.Date()`. A modo de ejemplo:

```R
mi_fecha <- as.Date("2022-12-10")
```

Ahora bien, si además tienes información del momento preciso –horas, minutos y segundos–, R va a trabajar con el tipo de dato `POSIXct`. Este consiste en un conteo de segundos desde el 1 de enero de 1970 a media noche.

> R no cuenta con una clase nativa que guarde únicamente horas, minutos y segundos. El paquete [hms](https://www.rdocumentation.org/packages/hms/versions/1.1.2) incluye la clase `hms` que habilita esta funcionalidad.
{: .prompt-tip }

Para leer una fecha de este estilo desde un `character`, puedes usar la función `as.POSIXct()` así:

```R
mi_fecha_hora <- as.POSIXct("2022-12-10 19:30:00")
```

> El formato en el que están escritas la fechas anteriores se conoce como [ISO 8601](https://es.wikipedia.org/wiki/ISO_8601). En este estándar se escriben las fechas de la mayor unidad a la menor unidad: Año antes que mes, mes antes que día, y así sucesivamente. De esta manera, se elimina la ambigüedad a la hora de interpretar a qué unidad pertenece cada número.
{: .prompt-info }

## Lubridate

Lubridate es un paquete que te ofrece funciones que aplican sobre fechas en formato `Date` y `POSIXct`, que te facilitarán mucho tu trabajo. Adicionalmente, este paquete funciona muy bien con otros paquetes del [tidyverse](https://www.tidyverse.org), por lo que lo puedes incorporar sin mayores problemas en tu código existente. Veamos algunas tareas que puedes hacer con `lubridate`.

### Lectura de fechas

El primer paso es leer un `character` que contiene una fecha en tu dataset. Si bien podrías hacerlo como lo mostré en un ejemplo anterior con las funciones `as.Date()` y `as.POSIXct()`, no siempre tendrás la fortuna de que las fechas estén en el formato ISO 8601. 

Por esta razón, lubridate te ofrece una familia de funciones que puedes usar para leer configuraciones comunes. Estas se basan en una letra para denotar cada unidad: "**y**"" para año, "**m**" para mes, "**d**" para día, "**h**" para hora, "**m**" para minuto y "**s**" para segundo.

> Es cierto que la "**m**" se repite en esta asignación. Sin embargo, esto no es un problema porque las funciones de lubridate separan el componente de fecha y el de hora con un guión bajo (\_), como lo verás más adelante.

Con esta lógica, se construyen funciones como `ymd()`, que te permite leer un `character` en el que el orden es Año-Mes-Día, sin importar el caracter que las separe. También funciones como `mdy()`, que espera un orden Mes-Día-Año, `dym()`, que espera Día-Año-Mes, y demás combinaciones de esas letras.

A modo de ejemplo, al ejecutar las siguientes líneas de código tendrás exactamente el mismo objeto `Date`:

```R
library(lubridate)

# Estas líneas devuelven la misma fecha: 
mi_fecha1 <- ymd("2022-12-10")
mi_fecha2 <- mdy("12/10/2022")
mi_fecha3 <- dym("10:2022:12")
#> "2022-12-10"
```

Lo mismo sucede si añadimos datos de hora. Con las mismas letras se construyen funciones como `ymd_hms()`, que puede crear un objeto `POSIXct` a partir de un texto en orden Año-Mes-Día-Hora-Minuto-Segundo. Ya te podrás imaginar qué hacen funciones como `mdy_hm()` o `dmy_h()`.

Veamos qué resulta de ejecutar las siguientes funciones:

```r
library(lubridate)

# Año-Mes-Día Hora-Minuto-Segundo
mi_fecha_hora1 <- ymd_hms("2022-12-10 19:30:00")
#> "2022-12-10 19:30:00 UTC"

# Mes-Día-Año Hora-Minuto
mi_fecha_hora2 <- mdy_hm("12/10/2022_19_30")
#> "2022-12-10 19:30:00 UTC"

# Día-Mes-Año Hora
mi_fecha_hora3 <- dmy_h("10:12:2022 19")
#> "2022-12-10 19:00:00 UTC"
```

> También puedes cargar una fecha que aparezca como una combinación entre año y trimestre (p.e. `"2022:4"`) usando la función `yq()`.
{: .prompt-tip }

### Unidades de tiempo

¡Perfecto! Ya sabes cargar una fecha con lubridate. Ahora vas a aprender a extraer los elementos individuales de las fechas. Puedes acceder al año, mes, día, hora, minuto, segundo y más unidades de tiempo de tu objeto tipo `Date` o `POSIXct`, con las siguientes funciones:

```r
mi_fecha_hora <- ymd_hms("2022-12-10 19:30:00")

# Año
year(mi_fecha_hora) 
#> 2022

# Mes
month(mi_fecha_hora) 
#> 12

# Día
day(mi_fecha_hora) 
#> 10

# Hora
hour(mi_fecha_hora) 
#> 19

# Minuto
minute(mi_fecha_hora)
#> 30 

# Segundo
second(mi_fecha_hora) 
#> 0

# Trimeste
quarter(mi_fecha_hora) 
#> 4

# Semestre
semester(mi_fecha_hora) 
#> 2

# Día de la semana
wday(mi_fecha_hora, week_start = 1) 
#> 6 
```

> En la función `wday()` es necesario poner el argumento `week_start = 1` para que el lunes sea el primer día de la semana –y no el domingo, que es la función por defecto–. Esto hace que el valor que devuelve la función sea 6, dado que la fecha del ejemplo es un sábado. Si no lo haces, la función te devuelve el 7, ya que es el séptimo día de la semana si comienzas a contar desde el domingo.
{: .prompt-tip }

Hasta el momento, todos los campos de fecha que hemos revisado se devuelven como un `integer`. Sin embargo, también puedes hacer que la función `month()` devuelva el nombre del mes o `wday()` el nombre del día. Hay dos argumentos que intervienen en esta operación: el primero es `label`, que le indica a `lubridate` que debe imprimir un nombre y no un número, y el segundo es `abbr`, que define si quieres una representación larga o corta del nombre. Veamos.

```r
mi_fecha_hora <- ymd_hms("2022-12-10 19:30:00")

# Nombre del mes sin abreviar
month(mi_fecha_hora, label = TRUE, abbr = FALSE)
#> diciembre

# Nombre del mes sin abreviar
month(mi_fecha_hora, label = TRUE, abbr = TRUE)
#> dic
```

> Dependiendo de la configuración de idioma de tu computador, así como el sistema operativo que estés usando, puede que la función te devuelva el nombre del mes o del día en otro idioma. Para forzar un idioma de salida de la función, debes usar el argumento `locale` con un código de idioma y país. Para español puedes usar `"es_ES"`.
{: .prompt-tip }

### Zonas horarias

Otro aspecto que debes saber manejar son las zonas horarias. Si estás trabajando con un set de datos que escapa a la jurisdicción de un país o que se desarrolla en una zona geográfica muy amplia, puedes encontrarte con fechas que están en distintos husos horarios. Hay 3 operaciones que vas a aprender:

- Definir la zona horaria de la fecha apenas la lees: Para hacerlo, simplemente debes agregar a cualquiera de las funciones vistas con anterioridad el argumento `tz`, con el código de la zona horaria de tu interés. Puedes consultar una lista de zonas horarias en este [enlace](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

```r
# Hora de Bogotá
mi_fecha_hora_bog <- ymd_hms("2022-12-10 19:30:00", tz = "America/Bogota")
#> "2022-12-10 19:30:00 -05"

# Hora de Madrid
mi_fecha_hora_mad <- ymd_hms("2022-12-10 19:30:00", tz = "Europe/Madrid")
#> "2022-12-10 19:30:00 CET"

# Hora de Tokyo
mi_fecha_hora_tok <- ymd_hms("2022-12-10 19:30:00", tz = "Asia/Tokyo")
#> "2022-12-10 19:30:00 JST"
```

> Por defecto, la zona horaria que se incluye en las fechas es `UTC`, el[Tiempo Universal Coordinado](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), que para efectos prácticos corresponde al tiempo del Meridiano de Greenwich.
{: .prompt-info }

- Cambiar una fecha de una zona horaria a otra: La función `with_tz()` permite cambiar una fecha de una zona horaria a otra en horario local. Esta función tiene dos argumentos principales: la fecha que queremos convertir y la zona horaria objetivo.

```r
# Hora de Bogotá
mi_fecha_hora_bog <- ymd_hms("2022-12-10 19:30:00", tz = "America/Bogota")
#> "2022-12-10 19:30:00 -05"

# Hora de Bogotá en hora local de Madrid
mi_fecha_hora_mad2 <- with_tz(mi_fecha_hora_bog, tz = "Europe/Madrid")
#> "2022-12-11 01:30:00 CET"

# Hora de Bogotá en hora local de Tokyo
mi_fecha_hora_tok2 <- with_tz(mi_fecha_hora_bog, tz = "Asia/Tokyo")
#> "2022-12-11 09:30:00 JST"
```

- Forzar una zona horaria sin cambiar la hora original: La función `force_tz()` permite cambiar la zona horaria de una fecha sin hacer la transformación anterior. Es decir, conservando intacta la hora.

```r
# Hora de Bogotá
mi_fecha_hora_bog <- ymd_hms("2022-12-10 19:30:00", tz = "America/Bogota")
#> "2022-12-10 19:30:00 -05"

# Forzar la hora de Bogotá a hora de Madrid
mi_fecha_hora_mad3 <- force_tz(mi_fecha_hora_bog, tz = "Europe/Madrid")
#> "2022-12-10 19:30:00 CET"

# Forzar la hora de Bogotá a hora de Tokyo
mi_fecha_hora_tok3 <- force_tz(mi_fecha_hora_bog, tz = "Asia/Tokyo")
#> "2022-12-10 19:30:00 JST"
```

### Periodos

Los periodos corresponden a los espacios de tiempo en unidades que usamos en el día a día, como años, meses, semanas o días. Es decir que incorporan características como horarios de verano, número de días de los meses o años bisiestos. Estos objetos te pueden servir para ubicar fechas que ocurren con cierta periodicidad específica en *términos humanos*.

La siguiente tabla resume las funciones que puedes usar para crear periodos. No incluyo una columna de unidad porque, por construcción, los periodos no tienen siempre la misma duración en segundos.

| Periodo       | Función de `lubridate` |
| ------------- | ---------------------- |
| Años          | years()                |
| Meses         | months()               |
| Semanas       | weeks()                |
| Días          | days()                 |
| Horas         | hours()                |
| Minutos       | minutes()              |
| Segundos      | seconds()              |
| Milisegundos  | milliseconds()         |
| Microsegundos | microseconds()         |

Puedes obtener el mismo día del próximo año de la siguiente manera:

```r
mi_fecha_hora <- ymd_hms("2022-12-10 19:30:00")
#> "2022-12-10 19:30:00 UTC"

# Misma fecha en el próximo año
mi_fecha_hora + years(1)
#> "2023-12-10 19:30:00 UTC" 
```

### Duraciones

Por el contrario, las duraciones corresponden a espacios de tiempo exactos, medidos en segundos. A las duraciones no les interesan las reglas de los calendarios –horarios de verano, número de días de los meses, años bisiestos–, por lo que pueden ser muy útiles cuando quieres automatizar procesos en tus scripts o guardar información sobre experimentos.

La siguiente tabla resume las unidades de tiempo que puedes manejar y a cuántos segundos equivalen:

| Duración      | # segundos | Función de `lubridate` |
| ------------- | ---------- | ---------------------- |
| Años          | 31536000   | dyears()               |
| Meses         | 2629800    | dmonths()              |
| Semanas       | 604800     | dweeks()               |
| Días          | 86400      | ddays()                |
| Horas         | 3600       | dhours()               |
| Minutos       | 60         | dminutes()             |
| Segundos      | 1          | dseconds()             |
| Milisegundos  | 0.001      | dmilliseconds()        |
| Microsegundos | 0.000001   | dmicroseconds()        |

A modo de ejemplo, para obtener una fecha más una duración de un año puedes escribir lo siguiente:

```r
mi_fecha_hora <- ymd_hms("2022-12-10 19:30:00")
#> "2022-12-10 19:30:00 UTC"

# Fecha + Duración de 1 año (31536000 segundos)
mi_fecha_hora + dyears(1)
#> "2023-12-11 01:30:00 UTC" 
```

### Intervalos

Los intervalos representan la diferencia de tiempo entre dos fechas y se pueden dividir entre periodos y duraciones para obtener su longitud en estas medidas. Estos se pueden calcular de dos maneras: llamando la función `interval()` con las dos fechas o escribiendo los caracteres `%--%` entre las dos fechas.

Por ejemplo, usando los casos anteriores podemos escribir algo como:

```r
mi_fecha_inicio <- ymd_hms("2022-12-10 19:30:00")
#> "2022-12-10 19:30:00 UTC"

mi_fecha_final <- ymd_hms("2023-02-10 19:30:00")
#> "2023-02-10 19:30:00 UTC"

# Intervalo calculado con la función interval()
mi_intervalo <- interval(mi_fecha_inicio, mi_fecha_final)
#> 2022-12-10 19:30:00 UTC--2023-02-10 19:30:00 UTC

# Intervalo calculado con %--%
mi_intervalo2 <- mi_fecha_inicio %--% mi_fecha_final
#> 2022-12-10 19:30:00 UTC--2023-02-10 19:30:00 UTC

# Periodos del intervalo
mi_intervalo / months(1)
#> 2

# Duraciones del intervalo
mi_intervalo / dmonths(1)
#> 2.036961
```

## Integración con el Tidyverse

Como lo mencioné anteriormente, `lubridate` está diseñado desde el comienzo para trabajar muy bien con las funciones del `tidyverse`. Esto significa que puedes ejecutar todas las funciones que viste con anterioridad con funciones como `group_by()`, `mutate()`, `filter()`, `sort()`, `summarise()`, etc.

Por ejemplo si tienes una columna con fechas `POSIXct` que registre algún evento, podrías agrupar por el año y el trimestre con un código así:

```R
dataframe |>
	group_by(anio = year(fecha), trimestre = quarter(fecha)) |>
	summarise(promedio = mean(variable_interes))
```

¡Inténtalo! Verás que es muy fácil extrapolar todos los conceptos que vimos a tu flujo de trabajo ordinario.

## Conclusión

¡Felicidades, ya sabes usar `lubridate`! Con las herramientas que aprendiste en este blog podrás desarrollar un gran número de tareas relacionadas con fechas. Como recursos adicionales, te dejo este *[cheat sheet](https://rawgit.com/rstudio/cheatsheets/main/lubridate.pdf)* elaborado por Posit, que te ayudará a recordar las funciones de este paquete cuando las necesites. Cuando se publiquen, pondré los enlaces de los posts sobre cómo trabajar con fechas en `datetime` y `pandas` en Python.

Te invito a dejar un comentario abajo si tienes algún truco sobre el manejo de `lubridate` y, si te pareció útil este post, a compartirlo en tus redes sociales.