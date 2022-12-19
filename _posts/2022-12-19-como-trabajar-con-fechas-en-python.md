---
title: ¿Cómo trabajar con fechas en Python?
date: 2022-12-19 10:20:00 -0500
categories: [Python]
tags: [fechas, datetimes, lubridate, r]
image: 
  path: /posts/2022-12-19-como-trabajar-con-fechas-en-python/hero.png
excerpt: Aprende a usar datetime.
---

Saber trabajar correctamente con fechas es una herramienta fundamental para cualquier científico de datos, ya que es muy común encontrarse con columnas de este tipo en los *datasets*. Sin embargo, como lo explico en [esta publicación](https://www.camartinezbu.com/posts/3-razones-que-dificultan-el-trabajo-con-fechas/), no es algo trivial dadas algunas complejidades propias de las fechas.

Al final de este post, sabrás como leer, manipular y hacer aritmética con fechas usando [datetime](https://docs.python.org/3/library/datetime.html). Si te interesa aprender a trabajar con fechas en R, te invito a leer [esta publicación](https://www.camartinezbu.com/posts/como-trabajar-con-fechas-en-r/).

## datetime

`datetime` es un módulo que viene en la instalación base de Python y que contiene todas las herramientas necesitarás para trabajar con fechas y horas.

Antes de entrar en las tareas que puedes hacer con este módulo, veamos algunos de los objetos con los que vas a trabajar. Estos dependerán de qué tanta información contiene tu campo de fecha.

### Tipos de objetos

Si únicamente tienes información sobre el año, mes y día, vas a utilizar el tipo `datetime.date`. Veamos un ejemplo sencillo creando una fecha con la función `date()`, que recibe como argumentos el año, el mes y el día:

```python
from datetime import date

mi_fecha = date(2022,12,10)
# datetime.date(2022, 12, 10)

type(mi_fecha)
# datetime.date
```

Si además tienes información del momento preciso –horas, minutos y segundos–, vas a trabajar con `datetime.datetime`. Podemos crear un objeto de este tipo con la función `datetime()`, que recibe como argumentos el año, mes, día, hora, minuto y segundo:

```python
from datetime import datetime

mi_fecha_hora = datetime(2022,12,10,19,30,0)
# datetime.datetime(1022, 12, 10, 19, 30)

type(mi_fecha_hora)
# datetime.datetime
```

> Más adelante verás otro tipo de objeto de `datetime`: `timedelta`, que se encarga de las diferencias entre dos fechas.
{: .prompt-info }

### Lectura de fechas

En la vida real, te vas a encontrar con fechas que están en formato `str`. Veamos cómo las puedes leer usando el módulo `datetime`. Empecemos con el caso en el que tienes una fecha escrita en formato [ISO 8601](https://es.wikipedia.org/wiki/ISO_8601): `"2022-12-10"`. 

> En el estándar [ISO 8601](https://es.wikipedia.org/wiki/ISO_8601) se escriben las fechas de la mayor unidad a la menor unidad, separando año, día y mes con un guión (`-`); y hora, mes y año con dos puntos (`:`). Esto evita la ambigüedad a la hora de interpretar a qué unidad pertenece cada número.
{: .prompt-info }

Para leerla como `datetime.date`, puedes ejecutar el siguiente código:

```python
from datetime import date

mi_fecha = date.fromisoformat("2022-12-10")
# datetime.date(2022, 12, 10)
```

> Desde la versión de Python 3.11 en adelante, puedes aplicar esta función incluso cuando no se separan las fechas con guión. Por ejemplo:
>
> ```python
> from datetime import date
>
> mi_fecha1 = date.fromisoformat("2022/12/10")
> # datetime.date(2022, 12, 10)
> 
> mi_fecha2 = date.fromisoformat("20221210")
> # datetime.date(2022, 12, 10)
> 
> mi_fecha3 = date.fromisoformat("2022:12:10")
> # datetime.date(2022, 12, 10)
> ```
{: .prompt-tip }

Esto mismo aplica para los `datetime`. Lo único que tenemos que cambiar es el tipo de objeto desde el que corremos el método:

```python
from datetime import datetime

mi_fecha_hora = datetime.fromisoformat("2022-12-10 19:30:00")
# datetime.datetime(2022, 12, 10, 19, 30)
```

¿Qué pasa cuando el `str` de la fecha no está en formato ISO 8601? Aquí deberás usar el método `strptime()`, que recibe dos argumentos: el texto que contiene la fecha y un texto especial que le dice al método cómo leer esa fecha. 

Este texto especial funciona con caracteres que describen cada elemento de la fecha, los más comunes se describen en la siguiente tabla:

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

Entonces, supón que tienes un `str` de texto así: `"Created on Dec 10/2022 at 19:30"`. Con los caracteres de la tabla anterior, puedes leerlo así:

```python
from datetime import datetime

mi_fecha_texto = datetime.strptime("Created on Dec 10/2022 at 19:30",
                                   "Created on %b %d/%Y at %H:%M")
# datetime.datetime(2022, 12, 10, 19, 30)
```

Ahora bien, esto no funciona para nombres de meses que estén escritos en otro idioma distinto a inglés. Para ello, debes configurar el idioma de tu ambiente ejecutando la función `locale.set_locale()`, con el segundo argumento ajustado al código del idioma y el país. Para español puedes usar `"es_ES"`:

```python
import locale

# Cambiar el idioma a español
locale.setlocale(locale.LC_TIME, "es_ES")

# Leer la fecha
mi_fecha_texto_es = datetime.strptime("Creado en Dic 10/2022 a las 19:30",
                                   "Creado en %b %d/%Y a las %H:%M")
# datetime.datetime(2022, 12, 10, 19, 30)
```

### Unidades de tiempo

Leer las fechas es apenas el principio. Usualmente tienes que extraer elementos puntuales de las fechas para hacer comparaciones o filtrar bases de datos. Esto lo puedes hacer usando distintos atributos y métodos que tienen los objetos `date` y `datetime`.

Para los objetos `date`, puedes extraer el año, el mes y el día con los atributos `year`, `month` y `date`, respectivamente. Además, puedes extraer el día de la semana con el método `isoweekday()`:

```python
mi_fecha = date(2022,12,10)
# datetime.date(2022, 12, 10)

mi_fecha.year
# 2022

mi_fecha.month
# 12

mi_fecha.day
# 10

mi_fecha.isoweekday()
# 6
```

Para el caso de objetos tipo `datetime`, también puedes acceder a la hora, minuto, segundo con los atributos `hour`, `minute` y `second`:

```python
mi_fecha_hora = datetime(2022,12,10,19,30,0)
# datetime.datetime(2022, 12, 10, 19, 30)

mi_fecha_hora.hour
# 19

mi_fecha_hora.minute
# 30

mi_fecha_hora.second
# 0
```

> Las funciones que incluye el módulo `datetime` son relativamente limitadas. También puedes consultar los objetos de clase `datetime64` y `timedelta64` de [Numpy](https://numpy.org/doc/stable/reference/arrays.datetime.html) para conocer lo que pueden hacer. Adicionalmente, en los próximos días voy a subir un tutorial para el manejo de fechas con `pandas`.
{: .prompt-info }

### Zonas horarias

Es muy importante que sepas lidiar con zonas horarias, especialmente porque te puedes encontrar con datasets cuyo alcance vaya más allá de la jurisdicción de un país o se desarrollen en una zona geográfica muy amplia. Para las operaciones que verás más abajo, necesitas instalar la librería [`pytz`](https://pypi.org/project/pytz/), ya que esta contiene información útil sobre zonas horarias y algunos métodos útiles para modificar los objetos `datetime`.

- Definir la zona horaria de la fecha: Si estás partiendo de un `datetime` que no tiene información de zona horaria –`datetime.tzinfo` es `None`–, necesitas crear un objeto `pytz.timezone` con un `str` que indique la zona horaria de tu interés. Posteriormente, ejecutas el método `localize()`, usando como argumento el `datetime` que quieres convertir:

```python
import pytz

mi_fecha_hora = datetime(2022,12,10,19,30,0)
# datetime.datetime(1022, 12, 10, 19, 30)

# Hora de Bogotá
mi_fecha_hora_bog = pytz.timezone("America/Bogota").localize(mi_fecha_hora)
# datetime.datetime(2022, 12, 10, 19, 30, tzinfo=<DstTzInfo 'America/Bogota' -05-1 day, 19:00:00 STD>)

# Hora de Madrid
mi_fecha_hora_mad = pytz.timezone("Europe/Madrid").localize(mi_fecha_hora)
# datetime.datetime(2022, 12, 10, 19, 30, tzinfo=<DstTzInfo 'Europe/Madrid' CET+1:00:00 STD>)

# Hora de Tokyo
mi_fecha_hora_mad = pytz.timezone("Asia/Tokyo").localize(mi_fecha_hora)
#datetime.datetime(2022, 12, 10, 19, 30, tzinfo=<DstTzInfo 'Asia/Tokyo' JST+9:00:00 STD>)
```

> Es muy importante que guardes este resultado en un variable –o sobreescribas la existente–. De lo contrario, no se guardarán los cambios a la zona horaria.
{: .prompt-warning }

- Cambiar una zona horaria a otra: Para cambiar tu hora a una hora local de otra ubicación, puedes usar el método `astimezone()` del objeto `datetime`, usando como argumento la zona horaria de `pytz`:

```python
import pytz

# Hora de Bogotá en hora local de Madrid
mi_fecha_hora_mad2 = mi_fecha_hora_bog.astimezone(pytz.timezone("Europa/Madrid"))
# datetime.datetime(2022, 12, 11, 1, 30, tzinfo=<DstTzInfo 'Europe/Madrid' CET+1:00:00 STD>)

# Hora de Bogotá en hora local de Tokyo
mi_fecha_hora_tok2 = mi_fecha_hora_bog.astimezone(pytz.timezone("Europa/Madrid"))
# datetime.datetime(2022, 12, 11, 9, 30, tzinfo=<DstTzInfo 'Asia/Tokyo' JST+9:00:00 STD>)
```

- Forzar una zona horaria sin cambiar la hora original: Si tu `datetime` ya tiene una zona horaria asociada, lo primero que tienes que hacer es eliminar el atributo `tzinfo`, usando el método `replace()`, para luego seguir los pasos para un `datetime` sin zona horaria:

```python
import pytz

mi_fecha_hora_bog
# datetime.datetime(2022, 12, 10, 19, 30, tzinfo=<DstTzInfo 'America/Bogota' -05-1 day, 19:00:00 STD>)

# Extraer la fecha y hora sin la zona horaria
mi_fecha_hora_sin_tz = mi_fecha_hora_bog.replace(tzinfo=None)
# datetime.datetime(2022, 12, 10, 19, 30)

# Forzar la hora a hora de Madrid
mi_fecha_hora_mad3 = pytz.timezone("Europe/Madrid").localize(mi_fecha_hora_sin_tz)
# datetime.datetime(2022, 12, 10, 19, 30, tzinfo=<DstTzInfo 'Europe/Madrid' CET+1:00:00 STD>)

# Forzar la hora a hora de Tokyo
mi_fecha_hora_tok3 = pytz.timezone("Asia/Tokyo").localize(mi_fecha_hora_sin_tz)
#datetime.datetime(2022, 12, 10, 19, 30, tzinfo=<DstTzInfo 'Asia/Tokyo' JST+9:00:00 STD>)
```

### Timedeltas

Los objetos de tipo `timedelta` representan una diferencia entre dos fechas o tiempos, en unidades fijas de días, segundos y microsegundos. Esto significa que el `timedelta` no tiene en cuenta características del uso ordinario de las fechas como horarios de verano, el número de días de los meses o años bisiestos.

```python
from datetime import timedelta

# Crear un timedelta
mi_delta = timedelta(days = 30)
# datetime.timedelta(days=30)

# Aplicar un timedelta a una fecha
mi_fecha_hora = datetime(2022,12,10,19,30,0)
# datetime.datetime(2022, 12, 10, 19, 30)

mi_fecha_hora + mi_delta
# datetime.datetime(2023, 1, 9, 19, 30)
```

A la función también se le pueden pasar argumentos como milisegundos, minutos, horas y semanas, pero estos siempre se van a convertir en términos de días, segundos y microsegundos.

## Conclusiones

¡Felicidades, ya sabes usar `datetime`! Con las herramientas que aprendiste en este blog podrás desarrollar un gran número de tareas relacionadas con fechas. Como recurso adicional, te dejo este post donde aprenderás a [trabajar con fechas en R](https://www.camartinezbu.com/posts/como-trabajar-con-fechas-en-r/).

Te invito a dejar un comentario abajo si tienes algún truco sobre el manejo de `datetime` y, si te pareció útil este post, a compartirlo en tus redes sociales.
