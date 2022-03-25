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

Hoy te quiero presentar Cookiecutter, una herramienta para el manejo de plantillas que te ayudará a organizar tu proyecto de ciencia de datos y a comenzar a trabajar en lo realmente importante en cuestión de minutos.

## Cookiecutter

[Cookiecutter](https://cookiecutter.readthedocs.io/en/latest/README.html) es una herramienta que funciona en la línea de comandos de tu computador y que te permite crear proyectos basados en plantillas, llamadas *cookiecutters*.

Esta herramienta es multiplataforma - funciona en Windows, MacOS y Linux - y no tiene una restricción sobre los lenguajes para los que puedes crear plantillas.

> Si bien no es necesario saber Python para usar cookiecutter, es algo útil cuando quieras diseñar tu propia plantilla y trabajar con [hooks](https://cookiecutter.readthedocs.io/en/latest/advanced/hooks.html?highlight=hook#writing-hooks).
{: .prompt-info }

## Instalación

Para poder trabajar con Cookicutter debes tener Python instalado en tu computador. Puedes hacerlo directamente desde su [página oficial](https://www.python.org/downloads/) o usando [Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html).

> Si no tienes mucha experiencia trabajando con Python, te sugiero hacerlo con Conda.
{: .prompt-tip }

Luego tienes que hacer un ajuste al *path* de tu computador, procedimiento que va a cambiar dependiendo del sistema operativo que uses. Te recomiendo seguir las instrucciones en la [documentación de Cookiecutter](https://cookiecutter.readthedocs.io/en/latest/installation.html).

Una vez hecho lo anterior, dependiendo de la instalación que seguiste, en la línea de comandos de tu computador vas a ejecutar:

```bash
# Si no usaste Conda
pip install cookiecutter

# Si usaste Conda
conda install -c conda-forge cookiecutter
```

## Escoge tu plantilla

Ahora que tienes instalado Cookiecutter, lo siguiente es escoger una plantilla que quieres replicar en tu computador. Github es un excelente lugar para buscar, dado que hay un gran número de repositorios con plantillas hechas por la comunidad en muchos lenguajes. Sólo pones en el buscador algo como `cookiecutter template` y el lenguaje de tu elección.

Para crear tu primer proyecto con Cookiecutter te invito a trabajar con alguna de las dos plantillas que escribí para [R](https://github.com/camartinezbu/cookiecutter-r-project) y [Python](https://github.com/camartinezbu/cookiecutter-python-project). Escoje alguna de ellas y copia la URL del repositorio en el portapapeles.

## Crea tu proyecto

Abre la línea de comandos de tu computador - `Command Shell` en Windows o `Terminal` en MacOS y Linux - y ve a la carpeta en la que quieres crear tu proyecto. Una vez allí, ejecuta el siguiente comando:

```bash
cookiecutter <URL de Github>
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
- `project_packages`: Una selección de algunos paquetes básicos de ciencia de datos. Si seleccionas `Basic` se incluirán numpy, pandas, matplotlib y seaborn entre otros. Si seleccionas `Scikit-learn` o `Tensorflow` se instalará además el paquete corresponidnete.

> La plantilla de R te pedirá los mismos datos, excepto `python_version` y `project_packages`.
{: .prompt-info }

Luego de introducir los datos anteriores, tu terminal debería verse similar a esta:

![Screenshot Terminal](/posts/2022-03-20-plantillas-para-proyectos-de-ciencia-de-datos/Terminal-screenshot.jpg)
*Figura 1. Captura de pantalla de la Terminal al crear tu proyecto.*

Como resultado, tendrás una plantilla de proyecto con carpetas para:

- Datos y su documentación: `data` y `references`
- Cuadernos de Jupyter: `notebooks`
- Reportes: `reports`
- Salidas gráficas: `figures`
- El código para el proyecto, cargado como un paquete de python: `src`

Adicionalmente, la plantilla generará automáticamente:

- un archivo `README.md` con la información que diligenciaste en la terminal
- un archivo con la licencia escogida: `LICENSE`
- un archivo `environment.yml` para crear un ambiente de conda
- un repositorio de git

![Screenshot Finder](/posts/2022-03-20-plantillas-para-proyectos-de-ciencia-de-datos/Finder-screenshot.jpg)
*Figura 2. Captura de pantalla de la carpeta creada con la plantilla.*

¡Y listo! Ya puedes empezar a trabajar en tu proyecto de ciencia de datos.
## Conclusión

Con esta explicación breve ya debes poder usar Cookiecutter y crear una carpeta de proyecto en tu computador con base en una plantilla. Te invito a buscar más plantillas en Github y experimentar con ellas para encontrar la que más se ajuste a tu flujo de trabajo. En la siguiente entrada del blog te enseñaré cómo crear tus propias plantillas y personalizar sus funcionalidades.

Si te pareció útil te invito a compartir este artículo en tus redes sociales y a dejar tus comentarios al final de la página.
