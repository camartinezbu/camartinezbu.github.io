---
title: Args y Kwargs en Python
date: 2022-09-20 10:15:00 -0500
categories: [Python]
tags: [python, args, kwargs, concepto]
image: 
  path: https://raw.githubusercontent.com/camartinezbu/blog-images/main/posts/2022-09-20-args-y-kwargs-en-python/hero.jpg

excerpt: Veamos cómo pasar un número arbitrario de argumentos a una función.
---

Uno de los primeros temas que probablemente abordaste al aprender a programar fue la creación de tus propias funciones. Estas ayudan a ordenar las instrucciones que plasmas en el código, a ahorrar espacio al no tener que repetir los mismos comandos varias veces, a disminuir la probabilidad de errores de tipeo porque sólo tienes que hacer la corrección en un lugar, etc. Sin embargo, puede que no hayas trabajado con los argumentos `*args` y `**kwargs`, que te van a ayudar cuando no sabes a ciencia cierta cuál será el número de argumentos de tu función.

## Un repaso

Antes que nada, recordemos rápidamente cómo se crean funciones en Python. En su versión más básica, la definición de una función consiste en 3 elementos: la palabra clave `def`, seguida del nombre que le vas a asignar a la función; una lista de parámetros separados con coma dentro de un paréntesis, a su vez seguidos por el caracter de dos puntos, y el bloque de la función, que va a contener las instrucciones que quieres que realice. De manera esquemática, la creación de una función se ve algo como:

```python
def mi_funcion(lista_de_argumentos):
  # Bloque de código que describe su funcionamiento.
```

A modo de ejemplo, si quisieras escribir una función que tome un *input* numérico y le sume 5, escribirías algo de la forma:

```python
def sumar_cinco(mi_numero):
  salida = mi_numero + 5
  return salida
```

Nota que en el ejemplo anterior añadí la palabra clave `return`, porque quiero que la función *devuelva* el resultado a mi entorno de trabajo y poder trabajar con él.

> Lo anterior se refiere únicamente a las funciones nombradas. En Python existen las [funciones lambda](https://www.w3schools.com/python/python_lambda.asp), que son funciones anónimas y que generalmente se reservan para operaciones simples porque sólo pueden tener una expresión. Si quisieras escribir una función lambda para el ejemplo anterior, sería algo como:
> ```python
> lambda x: x+5
> ```
{: .prompt-info }

Para añadir un número determinado de argumentos a la función, basta con separar los argumentos adicionales con coma. Por ejemplo, si quisieramos una función que multiplicara cuatro números que damos como *input*, podríamos hacer algo como:

```python
def multiplicacion_de_cuatro(arg1, arg2, arg3, arg4):
  return arg1 * arg2 * arg3 * arg4
```

No obstante, esta función serviría únicamente para las multiplicaciones en las que conoces de antemano que vas a tener exactamente 4 factores. En caso de no tenerlos, Python te devolvería un error al momento de llamar la función. En escenarios como estos, `*args` y `**kwargs` muestran su utilidad.

## *args

El argumento `*args` sirve para referirse a una lista de longitud variable de argumentos en la definición de una función. Es decir, no importa si son 2, 5, 10 o 100 argumentos; la función podrá lidiar con esa lista sin que sea necesario definir explícitamente el número de argumentos al momento de su creación.

Sigamos con el ejemplo de la multiplicación. Ya viste en el caso en que definíamos una multiplicación con un número determinado de argumentos. Para crear una función productoria, tenemos que emplear una lista variable de argumentos con `*args`. Una opción sería escribir algo así:

```python
def productoria(*args):
  salida = 1
  for arg in args:
    salida *= arg
  return salida
```

> Por si no lo habías visto, el operador `*=` que uso en el bloque de la función sirve para abreviar la operación de asignación y de multiplicación. Pude haber escrito esa misma línea como: `salida = arg * salida`.
{: .prompt-tip }

Quiero que veas dos cosas. Primero, que si bien incluimos la mención a `*args` con el asterisco al interior de los paréntesis, cuando nos referimos a esta lista arbitraria en el bloque de la función no tenemos que hacerlo; podemos escribir únicamente `args`.

> Esto es porque cuando se usa un asterisco antes de un elemento iterable –como una lista–, este se convierte en su propio operador: el *unpacking operator* u operador de desempacado. El comportamiento de este operador da para un post aparte, pero si quieres leer más al respecto puedes revisar este [artículo](https://geekflare.com/python-unpacking-operators/)
{: .prompt-info }

Segundo, que trabajar con una lista variable de argumentos implica cambiar la forma en la que construyes tu función. Ya no puedes definir directamente la multiplicación, justamente porque no conoces de antemano el número de factores. En este caso, tienes que determinar un punto de partida en 1 e iterar a lo largo de los elementos de la lista de longitud variable de argumentos. Si bien este puede no ser el ejemplo más complejo, sirve para ilustrar este cambio de lógica.

## **kwargs

La lógica del argumento `**kwargs` es muy parecida, sólo que cambia un poco la sintaxis. En vez de definir la lista como antes, ahora se va a definir a través de parejas  *key = value*. De ahí el `kw` de `**kwargs`; es por que son argumentos que van con su palabra clave o *keyword*.

En estricto sentido, mientras lo que va en el lugar de `*args` se comportaba como una lista o una tupla de Python, por lo que se puede iterar de la manera anterior; lo que va en lugar de `**kwargs` se comporta como un diccionario. Es decir, que podemos acceder a los elementos usando los métodos `.items()`, `.keys()` o `.values()`.

Veamos un ejemplo. Supongamos que queremos una función que imprima una serie de atributos para una persona: nombre, apellido y edad. Si sabemos que es exactamente ese número de elementos, podemos escribir algo como:

```python
def imprimir_caracteristicas(nombre, apellido, mascota):
  print("Nombre:", nombre)
  print("Apellido:", apellido)
  print("Mascota:", mascota)

imprimir_caracteristicas("Camilo", "Martinez", "Gato")
```

Ahora bien, si tenemos un número variable de atributos nos enfrentamos con el mismo problema de antes. No los podemos definir explícitamente porque no sabemos cuántos son. Aquí podemos usar `**kwargs`.

```python
def imprimir_caracteristicas_v2(**kwargs):
  for (key, value) in kwargs.items():
    print(f'{key}: {value}')

imprimir_caracteristicas_v2(nombre = "Camilo", apellido = "Martínez", mascota = "Gato", nacionalidad = "Colombia", color_favorito = "Azul")
```

## Todo junto

Por supuesto, todo lo que has visto se puede usar a la vez, incluso con los argumentos tradicionales sin el operador `*`. Veamos un ejemplo más complejo.

Supongamos que quieres crear una función que me imprima a la consola la descripción de un curso en un colegio específico. Esa función tendrá como *inputs* el nombre del Colegio, el nombre del curso, los alumnos que están matriculados en ese curso y las clases que tienen que ver en el día. Vamos a construirla por partes.

Primero, creemos una función simple que imprima en consola el nombre del colegio y el nombre del curso:

```python
def descripcion_curso(nombre_colegio, nombre_curso):
  print(f'Este es el resumen del curso {nombre_curso} del {nombre_colegio}.')

descripcion_curso("Colegio Pepito Pérez",
                  "4B")
```

Ahora continuemos escribiendo una lista de longitud arbitraria de estudiantes que están matriculados en ese curso usando `*args`:

```python
def descripcion_curso(nombre_colegio, nombre_curso, *args):
  print(f'Este es el resumen del curso {nombre_curso} del {nombre_colegio}.')
  print(f'Estas son las personas matriculadas: {", ".join(args)}')

descripcion_curso("Colegio Pepito Pérez",
                  "4B",
                  "Ana", "Juan", "Maria", "Carlos")
```

Finalmente, incluyamos las clases que tienen que ver en el día usando `**kwargs`:

```python
def descripcion_curso(nombre_colegio, nombre_curso, *args, **kwargs):
  print(f'Este es el resumen del curso {nombre_curso} del {nombre_colegio}.')
  print(f'Estas son las personas matriculadas: {", ".join(args)}')
  print('Estas son las clases del día:')
  for (key, value) in kwargs.items():
    print(f'{key}: {value}')


descripcion_curso("Colegio Pepito Perez",
                  "4B",
                  "Ana", "Juan", "Carlos", "Maria",
                  mañana = "Biología", tarde = "Matemáticas")
```

> No necesariamente te vas a encontrar con un ejemplo de este estilo en la vida real. Sin embargo, cuando estés utilizando esos tres tipos de argumentos es muy importante preservar el orden: Primero argumentos posicionales, luego `*args` y luego `**kwargs`. 
> 
> Adicionalmente, para este ejemplo específico, no puedes llamar el nombre de los argumentos posicionales –p.e. escribiendo `descripcion_curso(nombre_colegio = "Colegio Pepito Pérez")`– porque te generará un error al tener argumentos posicionales después de argumentos con palabra clave. Puedes leer más al respecto [acá](https://docs.python.org/3/glossary.html#term-parameter).
{: .prompt-warning }

## Conclusión

Espero que te hayan quedado más claros los conceptos de `*args` y `**kwargs`. Te dejo la tarea de experimentar y crear funciones más complejas que hagan uso de estas listas de parámetros de tamaño variable. Si te gustó, te invito a dejarme un comentario y a compartir este artículo en tus redes sociales.
