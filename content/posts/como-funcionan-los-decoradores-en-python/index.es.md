---
title: ¿Cómo funcionan los decoradores en Python?
date: 2023-04-07 13:19:00 -0500
categories: [Python]
tags: [decoradores, tutorial, concepto]
image: 
  path: /posts/2023-04-07-como-funcionan-los-decoradores-en-python/hero.jpg
excerpt: Lleva tus funciones a un nuevo nivel.
---

Los decoradores en Python son una característica poderosa y útil que te permiten modificar o envolver el comportamiento de una función sin modificar su código original. Sin embargo, si vienes de otros lenguajes o eres principiante en programación, puede que su funcionamiento sea un poco enredado.

En este artículo quiero explicarte cómo funcionan y darte algunos ejemplos básicos para que puedas involucrarlos en tu flujo de trabajo ordinario en Python.

## Los decoradores

En Python, un decorador es una función que **toma otra función como argumento y devuelve una nueva función** que generalmente agrega alguna funcionalidad al comportamiento original. Los decoradores se aplican utilizando el símbolo @ seguido del nombre de la función decoradora justo encima de la función que deseas decorar.

Cuando llamas a la función decorada, en realidad estás llamando a la función devuelta por el decorador, que puede realizar alguna tarea adicional antes o después de la ejecución de la función original.

## La estructura general

Para implementar un decorador en Python, puedes seguir una estructura básica como la que se presenta a continuación:

```python
def decorador(func):
  # Definir una nueva función que "envuelve" la función original
  def wrapper(*args, **kwargs):
    # Haz algo antes de llamar la función original
    resultado = func(*args, **kwargs)
    # Haz algo después de llamar la función original
    return result
  # Devuelve la nueva función
  return wrapper
```

Veamos paso a paso cómo funciona el código detrás del decorador. Primero, creas una función llamada `decorador` que toma como argumento a otra función llamada `func`. Al interior de `decorador`, creas otra función que se llama `wrapper`, que como su nombre lo indica, tiene como objetivo "envolver" la función `func`.

Hay que notar dos cosas sobre esta función. Por un lado, que los argumentos de la función `wrapper` son `*args` y `**kwargs`. Esto es porque no sabes los argumentos específicos de todas las funciones a las que le puedes aplicar el decorador. `*args` y `**kwargs` garantizan que el resultado del decorador va a usar esos mismos argumentos.

Por otro lado, que dentro de la función `wrapper` se definen las acciones que vas a implementar antes o después de la función original. En el ejemplo anterior, aparecen como dos mensajes en pseudocódigo antes y después de `resultado`. ¡Este es lugar donde los decoradores hacen su magia!

Como puedes ver, el decorador devuelve la función `wrapper`. Es decir, la función original (`func`) que incluye los cambios dictados por el decorador.

Finalmente, para aplicar el decorador a otra función, simplemente debes escribir un arroba (`@`) seguido del nombre del decorador. Inmediatamente debajo, debes crear a la función a la que le quieres aplicar el decorador. Algo así:

```python
@decorador
def mi_funcion():
  # Tu función especifica
  return # El resultado que quieres devolver con tu función
```

## Medir el tiempo de ejecución

Uno de los ejemplos clásicos sobre los decoradores es la creación de uno que ejecute una función particular y te permita medir cuánto se demora en ejecutarse. 

Veamos cómo funcionaría siguiendo la estructura anterior. El pseudocódigo asociado se vería algo así:

```python
# Crear la función decoradora, que tome como argumento otra función

  # Definir la función que envuelve (wrapper) la función original
    
    # Calcular el tiempo de inicio de ejecución

    # Ejecutar la función original

    # Calcular el tiempo del fin de ejecucuón

    # Imprimir el tiempo de ejecución en consola

    # Devolver el resultado (en este caso la función original)
  
  # Devolver la función wrapper
```

Como puedes ver, dado que lo que queremos hacer es calcular el tiempo de ejecución, vamos a guardar dos momentos en el tiempo e imprimiremos la diferencia entre el tiempo de inicio y el de finalización.

> No en todos los casos necesitas hacer algo antes y después de la función original. Debes ajustar el decorador de acuerdo con tu uso específico.

Si esta estructura está clara, pasemos a cómo se vería incluyendo el código de Python:

```python
import time

# Crear la función decoradora, que tome como argumento otra función
def tiempo_ejecucion(func):

  # Definir la función que envuelve (wrapper) la función original
  def wrapper(*args, **kwargs):
    # Calcular el tiempo de inicio de ejecución
    tiempo_inicio = time.time()
    # Ejecutar la función original
    resultado = func(*args, **kwargs)
    # Calcular el tiempo del fin de ejecucuón
    tiempo_fin = time.time()
    # Imprimir el tiempo de ejecución en consola
    print(f'{func.__name__} se demoró {tiempo_fin - tiempo_inicio:.2f} segundos en su ejecución.')
    # Devolver el resultado (en este caso la función original)
    return resultado

  # Devolver la función wrapper
  return wrapper
```

Varias precisiones en el código anterior.

- Es necesario cargar el módulo `time` para poder calcular el momento en que arranca y termina la ejecución de tu función.
- Como lo vimos anteriormente, en la definición de `wrapper`, así como en la llamada de `func` dentro de su alcance, usamos los argumentos `*args` y `**kwargs`. No olvides incluirlos para que el decorador funcione correctamente.
- En el mensaje que se imprime en consola se está usando un [f-string](https://realpython.com/python-f-strings/), una manera más concisa de incluir los valores de variables en strings. Como puedes ver, el mensaje incluye el nonbre de la función a la que le aplicas el decorador, así como el tiempo de ejecución en segundos, redondeado a 2 dígitos.

## El decorador en acción

Supón que quieres calcular cuánto se demora tu código en ejecutar una función que crea una *list comprehension* que calcula los cuadrados de los números del 0 al n, donde n es un argumento que le pasas a la función. Es decir, algo como esto:

```python
def calcular_cuadrados(numero):
  return [i**2 for i in range(numero + 1)]

calcular_cuadrados(9)
# La salida en consola sería:
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

> Como te puedes imaginar, en este ejemplo sencillo el tiempo de ejcución va a redondearse a 0 segundos. No obstante, el decorador funcionará igual para funciones que se demoran más en ejecutarse.

Si quieres aplicar el decorador que creamos anteriormente, tienes que llamarlo con el caracter `@` antes de la definición de la función. Algo así:

```python
@tiempo_ejecucion
def calcular_cuadrados(numero):
  return [i**2 for i in range(numero + 1)]

calcular_cuadrados(9)
# La salida en consola sería:
# calcular_cuadrados se demoró 0.00 segundos en su ejecución.
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

## Conclusión

Espero que con este post te quede un poco más claro cómo es el funcionamiento de los decoradores en Python. Para que lo puedas aplicar, te dejo la tarea de pensar en qué luegares de tu flujo de trabajo podrías utilizar los decoradores.

Piensa especialmente en tareas que tienes que hacer de manera reiterativa para múltiples funciones. Puede ser el cálculo de alguna estadística de resumen para una base de datos o la aplicación de algún tema o título en tus gráficos.

En los próximos días te compartiré varios ejemplos que pueden serte de utilidad, con la explicación paso a paso de cómo los construí.

Espero que te haya gustado este artículo. Si te pareció útil, te invito a compartirlo con más peronas. ¡Éxitos en tu aprendizaje!
