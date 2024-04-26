---
title: ¿Cómo trabajar con fechas en Pandas?
date: 2022-12-21 16:50:00 -0500
categories: [Python]
tags: [fechas, datetimes, lubridate, r, pandas]
image: 
  path: /posts/2022-12-21-como-trabajar-con-fechas-en-pandas/hero.png
excerpt: Aprende a usar las funcionalidades de pandas.
---

El trabajo con fechas tiene [numerosas complejidades](https://www.camartinezbu.com/posts/3-razones-que-dificultan-el-trabajo-con-fechas/), entre las que se cuentan el formato de escritura de las fechas, las zonas horarias y las diferentes formas de hablar de duraciones de tiempo.

En este post quiero explicarte cómo leer, manipular y hacer aritmética con fechas usando [pandas](https://pandas.pydata.org/docs/getting_started/index.html). Si buscas un tutorial para el módulo `datetime`, dirígete a [este enlace](https://www.camartinezbu.com/posts/como-trabajar-con-fechas-en-python/). Si te interesa aprender a trabajar con fechas en R, te invito a leer [esta publicación](https://www.camartinezbu.com/posts/como-trabajar-con-fechas-en-r/).

## pandas

Pandas es una de las librerías más usadas para trabajar con datos tabulares en Python. Esta incluye un gran número de funciones para leer, limpiar, modificar, hacer análisis descriptivo y exportar tablas; entre ellas, algunas diseñadas para optimizar tu trabajo con fechas y horas. 

Veamos los tipos de objetos relacionados con fechas con los que te vas a encontrar en `pandas`.

### Tipos de objetos

Pandas te permite capturar 4 conceptos relacionados con el tiempo:

- Momentos puntuales.
- Periodos: Una duración de tiempo que consiste en un punto de partida y una frecuencia.
- Diferencias de tiempo absolutas: que no tienen en cuenta la aritmética de los calendarios. Por ejemplo, años bisiestos.
- Diferencias de tiempo relativas: que si tienen en cuenta la aritmética de los calendarios.

Cada uno de estos conceptos está asociado a una función específica que crea objetos de distinta clase, dependiendo de si el argumento que se le pasa es un escalar, un array o un objeto de Python (`Series` o `DataFrames`). La siguiente tabla describe estas relaciones:

| Concepto              | Función asociada | Clase escalar | Clase Array      | Clase Pandas                            |
| --------------------- | ---------------- | ------------- | ---------------- | --------------------------------------- |
| Momento puntual       | `to_datetime()`  | `Timestamp`   | `DatetimeIndex`  | `datetime64[ns]` o `datetime64[ns, tz]` |
| Periodos              | `Period()`       | `Period`      | `PeriodIndex`    | `period[freq]`                          |
| Diferencias absolutas | `to_timedelta()` | `Timedelta`   | `TimedeltaIndex` | `timedelta64[ns]`                       |
| Diferencias relativas | `DateOffset()`   | `DateOffset`  | `None`           | `None`                                  |

### Lectura de fechas

Por lo pronto, veamos cómo funciona la lectura de fechas desde strings para los momentos puntuales, es decir, los objetos que están asociados con la función `to_datetime()`. En el siguiente bloque de código voy a crear un string, una lista y una serie de `pandas` con texto en formato [ISO 8601](https://es.wikipedia.org/wiki/ISO_8601):

```python
import pandas as pd

mi_string = "2022-11-06"
mi_array = ["2022-11-06", "2022-12-06", "2023-01-06"]
mi_serie = pd.Series(["2022-11-06", "2022-12-06", "2023-01-06"])
```

> El formato [ISO 8601](https://es.wikipedia.org/wiki/ISO_8601) es un estándar internacional para escribir fechas y horas, que elimina ambigüedades entre las distintas maneras de escribir fechas. En general, este estándar consiste en escribir primero las unidades más grandes (p.e. años) y luego las unidades más pequeñas (p.e. meses o días). Los componentes de la fecha se separan con un guión (`-`) y los componentes de la hora con dos puntos (`:`).

Los siguientes son los resultados de aplicar la función `to_datetime()` a los 3 tipos de objetos. Como te darás cuenta, estos corresponden a la tabla que mencioné con anterioridad:

```python
# String
pd.to_datetime(mi_string)
# Timestamp('2022-11-06 00:00:00')

# Array
pd.todatetime(mi_array)
# DatetimeIndex(['2022-11-06', '2022-12-06', '2023-01-06'], dtype='datetime64[ns]', freq=None)

# Serie
pd.to_datetime(mi_serie)
# 0   2022-11-06
# 1   2022-12-06
# 2   2023-01-06
# dtype: datetime64[ns]
```

Ahora bien, ¿qué sucede cuando el formato de los strings en los que tienes tus fechas no coincide con el estándar ISO 8601? En estos casos, debes proveer la estructura de la fecha en el argumento `format`. Esta estructura se escribe con un string especial compuesto por caracteres que representan cada uno de los elementos. Estos caracteres son los mismos que funcionan con el paquete `datetime` de Python base:

| Medida de tiempo                | Ejemplo     | Caracter asociado |
| ------------------------------- | ----------- | ----------------- |
| Año con siglo                   | `"1987"`    | `"%Y"`            |
| Año sin siglo                   | `"87"`      | `"%y"`            |
| Mes en número                   | `"02"`      | `"%m"`            |
| Mes en palabra larga            | `"Febrero"` | `"%B"`            |
| Mes en palabra corta            | `"Feb"`     | `"%b"`            |
| Día del mes                     | `"04"`      | `"%d"`            |
| Hora (formato 24 horas)         | `"19"`      | `"%H"`            |
| Hora (formato 12 horas)         | `"07"`      | `"%h"`            |
| Identificador de mañana o tarde | `"PM"`      | `"%p"`            |
| Minuto                          | `"30"`      | `"%M"`            |
| Segundo                         | `"00"`      | `"%S"`                  |

Supón que tienes un string en tu `DataFrame` que contiene una columna con las fechas en las que algún registro se actualizó, con el siguiente formato `"Updated on 09 Dec 2022 at 13:30:05"`. Al ejecutar la función `to_datetime()` sin ningún argumento adicional, Python te devolverá un error porque `pandas` no sabe cómo interpretar ese string:

```python
pd.to_datetime("Updated on 09 December 2022 at 13:30:05")
# ParserError: Unknown string format: Updated on 09 Dec 2022 at 13:30:05
```

Ahora intentemos con el argumento `format`. En este caso, puedes ver que el string contiene el número del día (`%d`), el nombre largo del mes (`%bB`), el año incluyendo los dígitos del siglo (`%Y`), la hora en formato 24 horas (`%H`), los minutos (`%M`) y los segundos (`%S`). Con esta información puedes construir el string que iría en `format` así:

```python
pd.to_datetime("Updated on 09 December 2022 at 13:30:05", format = "Updated on %d %B %Y at %H:%M:%S")
# Timestamp('2022-12-09 13:30:05')
```

Sin embargo, el código anterior sólo funciona para fechas que estén escritas en inglés, dado que contiene el nombre del mes en ese idioma. Si quisieras leer directamente el string `"Actualizado el 09 de Diciembre de 2022 a las 13:30:05"`, `pandas` te devolvería un error porque no reconoce `Diciembre` como el nombre de un mes:

```python
pd.to_datetime("Actualizado el 09 de Diciembre de 2022 a las 13:30:05", 
			   format = "Actualizado el %d de %B de %Y a las %H:%M:%S")
# ValueError: time data 'Actualizado el 09 de Diciembre de 2022 a las 13:30:05' does not match format 'Actualizado el %d de %B de %Y a las %H:%M:%S' (match)
```

Para corregir este error, tienes que definir el idioma en el que va a correr tu ambiente de Python usando la función `setlocale` del módulo `locale`. Como queremos que Python lea los nombres de los meses en español tenemos que usar el locale `"es_ES"` así:

```python
import locale

# Cambiar el idioma a español
locale.setlocale(locale.LC_TIME, "es_ES")

# Leer la fecha
pd.to_datetime("Actualizado el 09 de Diciembre de 2022 a las 13:30:05", 
			   format = "Actualizado el %d de %B de %Y a las %H:%M:%S")
# Timestamp('2022-12-09 13:30:05')
```

### Unidades de tiempo

Puedes acceder a una gran cantidad de elementos de los objetos `Timestamp` y `DatetimeIndex` llamando los atributos que se describen en la siguiente tabla. Si quieres más información, puedes ver la lista completa de atributos en el siguiente [enlace](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#time-date-components).

| Atributo      | Descripción                                  |
| ------------- | -------------------------------------------- |
| `year`        | Año                                          |
| `month`       | Mes                                          |
| `day`         | Día del mes                                  |
| `hour`        | Hora                                         |
| `minute`      | Minuto                                       |
| `Second`      | Segundo                                      |
| `date`        | Fecha sin información de zona horaria        |
| `time`        | Hora sin información de zona horaria         |
| `timetz`      | Hora incluyendo información de zona horaria  |
| `dayofyear`   | Día del año                                  |
| `dayofweek`   | Día de la semana (0 es lunes y 6 es domingo) |
| `daysinmonth` | Número de días del mes actual                |

Veamos un ejemplo. Supón que tienes un objeto `Timestamp` como el que se presenta a continuación. Puedes extraer los elementos fácilmente así:

```python
mi_ejemplo = pd.to_datetime('2022-12-09 14:20:35')

# Imprimir los componentes de la fecha
print("Año:", mi_ejemplo.year)
print("Mes:", mi_ejemplo.month)
print("Día:", mi_ejemplo.day)
print("Hora:", mi_ejemplo.hour)
print("Minuto:", mi_ejemplo.minute)
print("Segundo:", mi_ejemplo.second)
print("Día del año:", mi_ejemplo.dayofyear)

# Año: 2022
# Mes: 12
# Día: 9
# Hora: 14
# Minuto: 20
# Segundo: 35
# Día del año: 343
```

Te habrás dado cuenta que en los objetos que mencioné hace falta la Serie de `datetime64[ns]`. Esto es porque la forma de acceder a los componentes de la fecha es ligeramente diferente. En vez de llamar directamente el nombre del atributo como en la tabla, lo que tienes que hacer es poner primero el atributo `dt`. Es decir, si quieres extraer la hora de una Serie de fechas, deberías escribir algo como:

```python
mi_serie = pd.to_datetime(pd.Series(['2022-12-09 14:20:35',
									'2022-12-15 15:30:14',
									'2023-01-04 09:55:22']))

# Extraer la hora
print(mi_serie.dt.hour)
# 0    14
# 1    15
# 2    9
# dtype: int64
```

### Zonas horarias

Detrás de cámaras, `pandas` usa las librerías `pytz` y `dateutil` para manejar zonas horarias. Por defecto, los objetos que creas no tienen información de zona horaria. Pero esto se puede modificar fácilmente con las mismas lógicas de `pytz`. Veamos las operaciones más comunes que tendrás que realizar:

- Definir la zona horaria de la fecha: Si comienzas con un objeto que no tiene información de zona horaria, puedes definirla con el con el método `tz_localize()`.

```python
mi_fecha_hora = pd.to_datetime('2022-12-09 14:20:35')
# Timestamp('2022-12-09 14:20:35')

# Definir la zona horaria de Bogotá
mi_fecha_hora.tz_localize("America/Bogota")
# Timestamp('2022-12-09 14:20:35-0500', tz='America/Bogota')
```

> Es muy importante que guardes este resultado en un variable –o sobreescribas la existente–. De lo contrario, no se guardarán los cambios a la zona horaria.

- Cambiar de una zona horaria a otra: Para esto, puedes usar el método `tz_convert()`, escribiendo como argumento la zona horaria que quieres como resultado.

```python
# Definir un objeto en hora de Bogotá
mi_fecha_hora_bog = pd.to_datetime('2022-12-09 14:20:35').tz_localize("America/Bogota")
# Timestamp('2022-12-09 14:20:35-0500', tz='America/Bogota')

# Transformarlo a hora de Madrid
mi_fecha_hora_mad = mi_fecha_hora_bog.tz_convert("Europe/Madrid")
# Timestamp('2022-12-09 20:20:35+0100', tz='Europe/Madrid')

# Transformarlo a hora de Tokyo
mi_fecha_hora_tok = mi_fecha_hora_bog.tz_convert("Asia/Tokyo")
# Timestamp('2022-12-10 04:20:35+0900', tz='Asia/Tokyo')
```

- Forzar una zona horaria sin cambiar la hora original: Primero tienes que eliminar la información de zona horaria de tu objeto de interés usando `tz_localize(None)` y luego, incluyes la zona horaria que desees usando de nuevo `tz_localize()`.

```python
# Definir un objeto en hora de Bogotá
mi_fecha_hora_bog = pd.to_datetime('2022-12-09 14:20:35').tz_localize("America/Bogota")
# Timestamp('2022-12-09 14:20:35-0500', tz='America/Bogota')

# Forzar el objeto a hora de Madrid
mi_fecha_hora_mad2 = mi_fecha_hora_bog.tz_localize(None).tz_localize("Europe/Madrid")
# Timestamp('2022-12-09 14:20:35+0100', tz='Europe/Madrid')

# Forzar el objeto a la hora de Tokyo
mi_fecha_hora_tok2 = mi_fecha_hora_bog.tz_localize(None).tz_localize("Asia/Tokyo")
# Timestamp('2022-12-09 14:20:35+0900', tz='Asia/Tokyo')
```

### Periodos

Es momento de hablar de los tipos de objetos que quedaron pendientes. Los periodos representan duraciones de tiempo con una fecha específica de inicio. Esto se traduce en una diferencia sutil frente a los objetos tipo `Timestamp`: mientras que los `Timestamp` representan un momento específico en el tiempo, los objetos tipo `Period` describen una ventana de tiempo que comienza en un momento específico. En otras palabras, en vez de referirnos a un punto arbitrario en el tiempo como el 10 de diciembre de 2022, hablamos del día, semana o mes que comienzan a partir del 10 de diciembre de 2022. 

De manera similar a los momentos puntuales, podemos crear objetos distintos dependiendo de si partimos de un escalar, un vector o un objeto de `pandas`. Dado su propósito, la función `Period` contiene un argumento llamado `freq`, que define el tamaño de la ventana de tiempo que queremos definir. Algunas de las periodicidades más comunes son: 

| Caracter | Descripción    |
| -------- | -------------- |
| `D`      | Día calendario |
| `B`      | Día hábil      |
| `W`      | Semana         |
| `M`      | Mes            |
| `Q`      | Trimestre      |
| `Y`      | Año            |
| `H`      | Hora           |
| `T`      | Minuto         |
| `S`      | Segundo               |

> Puedes consultar el resto de duraciones en el [siguiente link](https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html#timeseries-offset-aliases)

A modo de ejemplo, puedes crear objetos de la familia de `Period` así:

```python
import pandas as pd

mi_string = "2022-11-06"
mi_array = ["2022-11-06", "2022-12-06", "2023-01-06"]
mi_serie = pd.Series(["2022-11-06", "2022-12-06", "2023-01-06"])

# Period con duración de 2 días.
pd.Period(mi_string, freq="2D")
# Period('2022-11-06', '2D')

# PeriodIndex con duración de 1 semana.
pd.PeriodIndex(mi_array, freq="W")
# PeriodIndex(['2022-10-31/2022-11-06', '2022-12-05/2022-12-11',
#             '2023-01-02/2023-01-08'],
#            dtype='period[W-SUN]', freq='W-SUN')

# period[freq] con duración de 1 semana
pd.to_datetime(mi_serie).dt.to_period("W")
# 0    2022-10-31/2022-11-06
# 1    2022-12-05/2022-12-11
# 2    2023-01-02/2023-01-08
dtype: period[W-SUN]
```

> Nota que para crear una serie de objetos tipo `period[freq]` necesitas partir de un array que ya sea un `Timestamp` o un `DatetimeIndex`. Por tal motivo, antes de usar el método `to_period()`, fue necesario convertir la serie con la función `pd.to_datetime()`.

### Timedeltas

Los objetos tipo `Timedelta` funcionan muy similar a los `timedelta` del módulo `datetime`. En resumen, describen un intervalo de tiempo exacto, medido en unidades como días, horas, minutos, segundos etc. Puedes crear estos objetos con la función `Timedelta()` y ajustar su duración con parámetros comp `days`, `hours`, etc. Aquí puedes ver un ejemplo de cómo usarlos para hacer aritmética con fechas.

```python
# Crear un Timedelta
mi_timedelta = pd.Timedelta(days=30)
# Timedelta('30 days 00:00:00')

# Usar un timedelta para conseguir una nueva fecha
mi_timestamp = pd.to_datetime("2022-12-21")
# Timestamp('2022-12-21 00:00:00')

mi_timestamp + mi_timedelta
# Timestamp('2023-01-20 00:00:00')
```

> Dada su construcción, no puedes usar `Timedelta` para representar duraciones de tiempo variables como las que usamos en nuestro lenguaje cotidiano. Para eso están los objetos tipo`DateOffset`, que verás a continuación.

### DateOffsets

Por su parte, los objetos de tipo `DateOffset` también se usan para describir intervalos de tiempo; pero en vez de hacerlo en términos de unidades exactas de tiempo, lo hacen siguiendo las reglas del calendario. Por ejemplo, un `DateOffset` con una duración de 1 mes, puede tener duraciones reales de 29, 29, 30 o 31 días dependiendo del mes y el año desde el que se calcule.

Como argumentos de la función `DateOffset()` puedes incluir duraciones como años (`years`), meses (`months`), semanas(`weeks`), días(`days`), entre otros. Encontrarás los demás argumentos en [este enlace](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.tseries.offsets.DateOffset.html#pandas.tseries.offsets.DateOffset). A continuación verás un ejemplo de trabajo con objetos `DateOffset`.

```python
# Crear un DateOffset
mi_dateoffset = pd.DateOffset(months=1)

# Crear una fecha de inicio:
mi_timestamp = pd.to_datetime("2023-01-21")
# Timestamp('2023-01-21 00:00:00')

# Calcular la fecha más un mes
mi_timestamp2 = mi_timestamp + mi_dateoffset
# Timestamp('2023-02-21 00:00:00')

# ¿Cuántos días de diferencia tienen las dos fechas?
(mi_timestamp + mi_dateoffset) - mi_timestamp
# Timedelta('31 days 00:00:00')
```

> Como puedes ver, al calcular la diferencia entre dos fechas obtienes un objeto tipo `Timedelta`. Intenta cambiar los argumentos de `DateOffset()` y calcula la diferencia entre las fechas para que veas la variabilidad en su duración.

## Conclusiones

¡Felicidades, ya sabes usar `pandas` para trabajar con fechas! Con las herramientas que aprendiste en este blog podrás desarrollar un gran número de tareas relacionadas con fechas. Como recursos adicionales, te dejo los enlaces a un par de tutoriales que escribí para que aprendas a hacer [tareas similares en R](https://www.camartinezbu.com/posts/como-trabajar-con-fechas-en-r/) y [en Python](https://www.camartinezbu.com/posts/como-trabajar-con-fechas-en-python/).

Te invito a dejar un comentario abajo si tienes algún truco sobre el manejo de `datetime` y, si te pareció útil este post, a compartirlo en tus redes sociales.
