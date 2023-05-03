---
title: Aprende a manejar rutas de archivo con pathlib
date: 2023-05-03 13:43:00 -0500
categories: [Python]
tags: [tutorial, carpetas, archivos]
image: 
  path: /posts/2023-05-03-aprende-a-manejar-rutas-de-archivo-con-pathlib/hero.jpg
excerpt: Crea archivos y navega entre carpetas en Python.
---

Si ya has trabajando en scripts o cuadernos de jupyter para algún proyecto de ciencia de datos, probablemente hayas experimentado problemas para manejar las rutas de archivos. En este post te quiero explicar el funcionamiento de `pathlib`, una librería que te facilitará la interacción con archivos en tu computador y que seguro te será útil en tus proyectos de programación.

## Pathlib

[`pathlib`](https://docs.python.org/3/library/pathlib.html) es una librería de Python que te proporciona una forma amigable y consistente para trabajar con rutas de archivo en tu computador. Esto lo hace a través de una aproximación orientada a objetos, por lo que si ya tienes familiaridad con este paradigma, no tendrás muchos problemas en incorporarla en tu flujo normal de trabajo.

## ¿Cómo funciona?

Las rutas de archivo en `pathlib` se representan a través de un objeto de clase `Path`. Este objeto funciona como un string común y corriente, pero con métodos que lo hacen más útil para trabajar con carpetas y archivos en tu computador.

Para crear un objeto de clase `Path`, tienes que pasar un string con la ruta de archivo a la función del mismo nombre así:

```python
from pathlib import Path

mi_ruta = Path("carpeta/archivo.txt")
```

Esto creara un objeto llamado `mi_ruta` que representa un archivo llamado `archivo.txt` al interior de la carpeta con un nombre muy creativo: `carpeta`.

> En Windows, la sintaxis de las rutas de archivo usa barras invertidas (`\`) en lugar de las barras diagonales (`/`). Esto puede causar algunos problemas si Python piensa que estás tratando de "escapar" el siguiente caracter y no queriendo usar la barra invertida como tal. 
> 
> No obstante, `pathlib` se encarga automáticamente de estos errores, entonces no te tienes que preocupar por hacer ajustes manuales al string de la ruta de archivo.
{: .prompt-tip }

## Enlazar rutas de archivo

También puedes crear rutas de archivo más complejhas al concatenar varias partes usando el operador `/`, tal como lo harías en una ruta de archivos ordinaria en MacOS o Linux. Por ejemplo, puedes hacer algo de este estilo:

```python
carpeta_madre = Path("Documentos")
carpeta_hija = carpeta_madre / "Proyectos" / "Proyecto_1"
archivo_final = carpeta_hija / "documento.docx"
```

En el ejemplo, `carpeta_madre` es una ruta que representa la carpeta `Documentos`. Luego la variable `carpeta_hija` crea un objeto tipo `Path` que representa una carpeta `Proyecto_1` que se encuentra al interior de `Proyectos`, a su vez dentro de `Documentos`. Finalmente, `archivo_final` describe una ruta con un archivo `documento.docx` al interior de la carpeta `Proyecto_1`.

## Atributos de los objetos `Path`

Los objetos de clase `Path` tienen muchos atributos que te pueden ser de utilidad. Acá te dejo algunos de los más usados:

| Atributos | Descripción |
|---------|-------------|
| `.name` | El último componente de la ruta |
| `.parent` | El directorio padre de la ruta |
| `.stem` | El último componente de la ruta sin la extensión |
| `.suffix` | La extensión de la última componente de la ruta |
| `.drive` | La letra de la unidad del disco en sistemas Windows (p.e. `C:\`) |
| `.root` | La parte inicial de la ruta que denota la raíz del sistema de archivos (`/` en Linux o `C:\` en Windows) |

## Funciones útiles

`pathlib` también contiene varias functiones que puedes usar para interactuar más directamente con archivos y directorios. En esta tabla te resumo algunos de ellos.

| Funciones | Descripción |
|---------|-------------|
| `mkdir()` | Crea un nuevo directorio en la ruta especificada |
| `exists()` | Verifica si la ruta especificada existe |
| `is_dir()` | Verifica si la ruta especificada es un directorio o carpeta |
| `is_file()` | Verifica si la ruta especificada es un archivo |
| `glob()` | Devuelve un iterador que produce todas las rutas que coinciden con un patrón glob |
| `rglob()` | Devuelve un iterador que produce todas las rutas que coinciden con un patrón glob de manera recursiva |

> Los patrones glob son cadenas de texto que se usan para buscar archivos y directorios en la shell de Unix. Puedes encontrar más información sobre ellos [acá](https://www.malikbrowne.com/blog/a-beginners-guide-glob-patterns/).
{: .prompt-info }

Ahora que conoces los conceptos básicos, veamos un ejemplo práctico de cómo podrías usar `pathlib` en Python.

## Ejemplo práctico

Supongamos que tienes un archivo llamado `datos.csv` en una carpeta llamada `Proyecto` en el escritorio de tu computador. Podrías ir manualmente a la ubicación del archivo con tu explorador de archivos y copiar la ruta. O también podrías hacerlo con lo que acabas de aprender sobre `pathlib`.

Primero, crea la ruta completa de de este archivo. En este ejemplo voy a usar la función `Path.home()`, que devuelve un objeto `Path` que representa el directorio de inicio del usuario actual. En Windows esto debería ser algo como `C:\Users\usuario`, en Linux `/home/usuario` y en MacOS `/Users/usuario`. Arrancar en esta ruta puede ayudar a ubicar más fácilmente los archivos o carpetas.

```python
from pathlib import Path

ruta_completa = Path.home() / "Escritorio" / "proyecto" / "datos.csv"
```

Ahora que tienes la ruta completa de tu archivo, puedes abrirlo y trabajar con él desde otras librerías de acuerdo con tu necesidad. `pathlib` funciona con varias librerías populares de Python como [`pandas`](https://pandas.pydata.org), [`scikit-learn`](https://scikit-learn.org/stable/) o [`torch`](https://pytorch.org).

También puedes aprovechar la funcionalidad de `Pathlib` de una forma más programática. Por ejemplo, podrías usar código para verificar si un archivo o carpeta existen antes de trabajar con ellos:

```python
from pathlib import Path

ruta_completa = Path.home() / "Escritorio" / "proyecto" / "datos.csv"

if ruta_completa.exists():
    with open(ruta_completa, "r") as datos:
        # Ejecuta algún cálculo con los datos
        print("Archivo leido con éxito.")
else:
    print("El archivo no existe.")
```

En este caso, `ruta_completa.exists()` revisa si el archivo `datos.csv` existe en el lugar que le indicamos. Si el archivo existe, podemos abrirlo y hacer alguna operación dentro el bloque de código `with`. Si no, se imprime el mensaje `"El archivo no existe."` en la consola.

## Conclusión

Como puedes ver, `pathlib` es una librería muy útil para trabajar con archivos y directorios en Python. Los objetos de clase `Path` tienen varios atributos y funciones que te van a facilitar la vida a la hora de crear, eliminar y renombrar archivos y carpetas. Así que es un activo muy útil para tu caja de herramientas de ciencia de datos.

¿Qué otros usos se te ocurren para trabajar con `pathlib`? 

Si te pareció útil este artículo, te invito a compartirlo con más personas. ¡Éxitos en tu aprendizaje!
