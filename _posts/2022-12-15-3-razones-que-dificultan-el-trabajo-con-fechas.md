---
title: 3 razones que dificultan el trabajo con fechas
date: 2022-12-15 18:10:00 -0500
categories: [Conceptos]
tags: [fechas, datetimes, lubridate, r, python]
image: 
  path: /posts/2022-12-15-3-razones-que-dificultan-el-trabajo-con-fechas/hero.png
excerpt: Las fechas no son tan sencillas como parecen.
---

Una de las tareas más habituales en el trabajo de un científico de datos tiene que ver con el manejo de fechas. Asumiendo que tus datos ya [están ordenados](https://www.camartinezbu.com/posts/que-significa-que-los-datos-esten-ordenados/), los campos que describen fechas te van a permitir hacer agrupaciones de tus observaciones de acuerdo con periodos de tiempo específicos, encontrar patrones útiles para la toma de decisiones y ayudarte a diseñar visualizaciones de datos llamativas y fáciles de entender.

Sin embargo, el trabajo con fechas no es tan evidente. En este post quiero hablar sobre por qué es complejo trabajar con fechas y al final encontrarás el enlace para aprender a usar `lubridate` en R, así como `datetime` y `pandas en Python.`

## 1. Formato de escritura

Dependiendo del lugar de origen de tu dataset o la organización que lo elabora, te vas a encontrar con que las fechas tienen una gran cantidad de formatos distintos. Para ilustrar este punto, veamos las formas en las que podrías escribir el 5 de octubre de 2022.

Empecemos con representaciones de esa fecha que usan únicamente números. Con esta restricción, podríamos modificar el orden en el que aparecen los elementos de la fecha, así como los caracteres que los separan. Frente al orden, podríamos escribir esta fecha así:

- Año/Mes/Día: `2022/10/05`
- Día/Mes/Año: `05/10/2022`
- Mes/Día/Año: `10/05/2022`

Por otra parte, no es necesario que escribas el año de la fecha con cuatro dígitos. Los siguientes ejemplos usan una notación más corta:

- Día/Mes/Año corto: `05/10/22`
- Mes/Día/Año corto: `10/05/22`

Con respecto a los caracteres, también podrías representar esa fecha sin usar el "/", reemplazándolo por caracteres como:

- `2022-10-05`
- `2022_10_05`
- `2022 10 05`
- `20221005`

Si permitimos que los elementos de la fecha estén escritos como palabras, el universo de posibilidades se amplia aún más. Acá puedes ver algunas formas alternativas en las que podrías escribir la fecha del ejemplo:

- `5 Oct 2022`
- `5 de Octubre de 2022`
- `Octubre 5 2022`

> A este último caso le podemos añadir una complejidad adicional. Dado que las fechas están escritas en lenguaje natural, también te puedes encontrar con fechas en otros idiomas: `5th October 5, 2022`,  `Octobre 5 2022`, u `Outubro 5, 2022`.
{: .prompt-info }

## 2. Zonas horarias

Todo lo anterior asume que las fechas con las que estás trabajando tienen la misma zona horaria, pero este no siempre es el caso. Imagina que tienes un *dataset* con las salidas y llegadas de los vuelos de una aerolínea, expresadas en hora local. En este caso, dos vuelos que tengan `2022/03/01 05:43:00 AM`  en la columna de hora de llegada pueden hacer referencia a dos momentos completamente distintos.

Esto sin contar las modificaciones especiales a la hora, como el [Horario de Verano](https://es.wikipedia.org/wiki/Horario_de_verano) –o *Daylight Savings Time*–, que adelanta la hora del reloj durante los meses más calientes del año y que se utiliza en partes de Estados Unidos, Canadá, varios países de Europa, Chile, Paraguay, entre otros.

![Mapa de Horarios de verano](https://upload.wikimedia.org/wikipedia/commons/thumb/1/16/DST_Countries_Map.png/1280px-DST_Countries_Map.png)
*Mapa de lugares que emplean el horario de verano: en azúl los que lo usan en el verano boreal, en naranja en el verano austral, en gris claro donde ya no se emplea y en gris oscuro donde nunca se empleó. Fuente: Wikipedia.*

## 3. Intervalos de tiempo

Supón que estás creando un evento en tu calendario que debería repetirse en un día específico de cada mes; digamos que el quinto día. Muy probablemente, lo que harías es ir a tu calendario y marcar el 5 de octubre, el 5 de noviembre, el 5 de diciembre y así sucesivamente. Fácil, ¿verdad?

![Ejemplo espacios irregulares de tiempoi](/posts/2022-12-15-3-razones-que-dificultan-el-trabajo-con-fechas/Figura1.png)

Cambiemos un poco la ejemplo: ahora el evento se debería repetir cada 30 días. Siguiendo la instrucción, lo que deberías hacer es navegar en tu calendario contando los días a partir del día inicial y cuando llegues al día 30, marcarías la siguiente ocurrencia del evento. Es un poco más complejo, pero nada que no pudieras hacer a mano.

![Ejemplo espacios regulares de tiempo](/posts/2022-12-15-3-razones-que-dificultan-el-trabajo-con-fechas/Figura2.png)

Sin embargo, lo que estos ejemplos quieren ilustrar es que no hay una única manera de hacer aritmética con fechas y horas. En el primer caso, estamos tomando una interpretación cotidiana de las fechas, que tiene espacios de tiempo irregulares. Siguiendo el ejemplo, algunas veces vamos a tener 30 días entre los eventos y otras 31. En el segundo caso, estamos tomando una interpretación un poco más precisa, pero que no corresponde con nuestro día a día. Muy probablemente, a partir del siguiente mes el número del día no coincida.

## Conclusión

Como te puedes dar cuenta, en contra de lo que podría parecer a simple vista, la representación de una fecha no es algo trivial. Afortunadamente, lenguajes de programación como R y Python contienen paquetes que te permiten lidiar con todos estos aspectos fácilmente.

Si quieres aprender a trabajar con `lubridate` en R, te invito a que leas [este post](https://www.camartinezbu.com/posts/como-trabajar-con-fechas-en-r/) que escribí. También te dejo los posts que escribí sobre [`datetime`](https://www.camartinezbu.com/posts/como-trabajar-con-fechas-en-python/) y [`pandas`](https://www.camartinezbu.com/posts/como-trabajar-con-fechas-en-pandas/).

Si te pareció útil, déjame un comentario abajo o comparte este post en tus redes. ¡Éxitos en tu aprendizaje!
