---
title: Plantillas para proyectos de ciencia de datos
date: 2022-03-20 12:30:00 -0500
categories: [Cookiecutter, Tutorial]
tags: [cookicutter, plantilla, ciencia-datos]
image: 
  path: https://raw.githubusercontent.com/camartinezbu/blog-images/main/posts/2022-03-20-plantillas-para-proyectos-de-ciencia-de-datos/hero.jpeg
excerpt: Te dejo dos plantillas para que puedas trabajar de inmediato.
---

Al iniciar tu proyecto de ciencia de datos seguro te has enfrentado a numerosas decisiones como qué carpetas crear, dónde guardar las fuentes de información, cómo organizar scripts y notebooks, dónde guardar los resultados de tu análisis, etc. 

Hoy te quiero presentar cookiecutter, una herramienta para el manejo de plantillas que te ayudará a organizar tu proyecto de ciencia de datos y a comenzar a trabajar en lo realmente importante en cuestión de minutos.

## Cookiecutter

[Cookiecutter](https://cookiecutter.readthedocs.io/en/latest/README.html) es una herramienta que funciona en la línea de comandos de tu computador que te permite crear proyectos basados en plantillas, llamadas *cookiecutters*.

Esta herramienta es multiplataforma - funciona en Windows, MacOS y Linux - y no tiene una restricción sobre los lenguajes para los que puedes crear plantillas.

> Si bien no es necesario saber python para usar cookiecutter, es algo útil cuando quieras diseñar tu propia plantilla y trabajar con [hooks](https://cookiecutter.readthedocs.io/en/latest/advanced/hooks.html?highlight=hook#writing-hooks).
{: .prompt-info }

## Instalación

Para poder trabajar con Cookicutter debes tener Python instalado en tu computador. Puedes hacerlo directamente desde la [página oficial de Python](https://www.python.org/downloads/) o usando [Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html).

> Si no tienes mucha experiencia trabajando con Python, te sugiero hacerlo con Conda.
{: .prompt-tip }

Luego tienes que hacer un ajuste al *path* de tu computador, procedimiento que va a cambiar dependiendo del sistema operativo que uses. Te recomiendo seguir las instrucciones en la [documentación de Cookicutter](https://cookiecutter.readthedocs.io/en/latest/installation.html).

Una vez hecho lo anterior, en la línea de comandos de tu computador vas a ejecutar alguno de los siguientes comandos, dependiendo la instalación que seguiste:

```bash
# Si no usaste Conda
pip install cookiecutter

# Si usaste Conda
conda install -c conda-forge cookiecutter
```

## Escoge tu plantilla

Ahora que tienes instalado Cookicutter, lo siguiente es escoger una plantilla que quieres replicar en tu computador. Github es un excelente lugar para buscar, dado que hay un gran número de repositorios con plantillas hechas por la comunidad en muchos lenguajes. Sólo pones en el buscador algo como `cookiecutter template` y el lenguaje de tu elección.

Para crear tu primer proyecto con Cookicutter te invito a trabajar con alguna de las dos plantillas que escribí para [R](https://github.com/camartinezbu/cookiecutter-r-project) y [Python](https://github.com/camartinezbu/cookiecutter-python-project). Escoje alguna de ellas y copia la URL del repositorio en el portapapeles.


## Crea tu proyecto

Abre la línea de comandos de tu computador - `Command Shell` en Windows o `Terminal` en MacOS y Linux - y ubícate en la carpeta que quieres crear tu proyecto. Una vez allí, ejecuta el siguiente comando:

```bash
cookiecutter <URL de GIthub>
```

Es decir, si quisiera usar la plantilla que escribí para Python escribiría:

```bash
cookiecutter https://github.com/camartinezbu/cookiecutter-python-project
```

Si Cookiecutter quedó instalado correctamente, la línea de comandos te pedirá algunos datos con los que generar tu proyecto. Para la plantilla de Python estos son:

- `project_name`: El nombre del proyecto que vas a crear.
- `project_slug`: El apodo para tu proyecto. Por defecto incluye una versión en minúsculas separada por guión bajo del nombre del proyecto, pero lo puedes reemplazar por un apodo de tu elección.
- `project_author_name`: Tu nombre o el de la organización que elabora el proyecto.
- `project_description`: Una descripción breve del objetivo del proyecto.
- `project_open_source_license`: La elección de licencia que vas a usar en el proyecto. La plantilla incluye las opciones de no usar licencia, la licencia MIT y la licencia BSD-3-Clause.
- `python_version`: La versión de python para usar en el proyecto. Por defecto se incluye la versión 3.10.
- `project_packages`: Una selección de algunos paquetes básicos de ciencia de datos. Si seleccionas `Basic` se incluirán numpy, pandas, matplotlib y seaborn entre otros. Si seleccionas `Machine Learning` se tendrán en cuenta además scikit-learn y tensorflow.

![Screenshot Terminal](/posts/2022-03-20-plantillas-para-proyectos-de-ciencia-de-datos/Terminal-screenshot.jpg)
*Figura 1. Captura de pantalla de la Terminal al crear tu proyecto.*

Al completarse la operación, tendrás una plantilla de proyecto con carpetas para los datos y su documentación (`data` y `references`), extracción, transformación y carga (`etl`), análisis (`analysis`), visualización (`viz`) y reportes (`reports`).

Adicionalmente, la plantilla generará automáticamente un archivo `README.md` con la información que diligenciaste en la terminal, otro con la licencia escogida (`LICENSE`) e iniciará un repositorio de git.

![Screenshot Finder](/posts/2022-03-20-plantillas-para-proyectos-de-ciencia-de-datos/Finder-screenshot.jpg)
*Figura 2. Captura de pantalla de la carpeta creada con la plantilla.*

## Conclusión

¡Y listo! Ya puedes empezar a trabajar en tu proyecto de ciencia de datos. Te invito a buscar más plantillas en Github y experimentar con ellas para encontrar las que más se sjusten a tu flujo de trabajo. En la siguiente entrada del blog te enseñaré cómo crear tus propias plantillas y personalizar sus funcionalidades.

Si te pareció útil te invito a compartir esta entrada en tus redes sociales y a dejar tus comentarios al final de la página.
