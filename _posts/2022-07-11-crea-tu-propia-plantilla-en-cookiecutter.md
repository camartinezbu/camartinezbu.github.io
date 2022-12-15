---
title: Crea tu propia plantilla en Cookiecutter
date: 2022-07-11 14:00:00 -0500
categories: Cookiecutter
tags: [cookiecutter, plantilla, proyecto, tutorial]
image: 
  path: /posts/2022-07-11-crea-tu-propia-plantilla-en-cookiecutter/hero.jpg
excerpt: Personaliza tus proyectos de ciencia de datos.
---

En una entrada de blog anterior te compartí [dos plantillas para proyectos de ciencia de datos](https://www.camartinezbu.com/posts/plantillas-para-proyectos-de-ciencia-de-datos/) que hice en Cookiecutter. En esta ocasión quiero enseñarte cómo puedes crear tus propias plantillas y ajustarlas a tu flujo de trabajo específico.

Antes de empezar, debes tener Python y Cookiecutter instalado tu computador. Para ver cómo se hace puedes ir a la sección correspondiente del [post original](https://www.camartinezbu.com/posts/plantillas-para-proyectos-de-ciencia-de-datos/#instalación) o consultar la [documentación de Cookiecutter](https://cookiecutter.readthedocs.io/en/latest/installation.html).

## Sintaxis

Lo primero que debes saber es que Cookiecutter usa una sintaxis especial que viene de Jinja, el motor de plantillas en el que está escrito. Dado que el código de la plantilla tiene que convivir con código en otros lenguajes de programación, es muy importante poder diferenciar lo que está escrito en Jinja de lo que no.

Por tal razón, las lineas con las que vas a personalizar tu plantilla están encerradas en corchetes (`{}`). En adelante me voy a referir a las líneas de Jinja como bloques.

Dependiendo los caracteres que delimiten esos bloques, vas a lograr distintas funciones:

- Para **imprimir el contenido en la salida de la plantilla**, vas a encerrar el código en doble corchete: {% raw %}`{{...}}`{% endraw %}. Por ejemplo, si quieres llamar la variable autor de la plantilla, tienes que escribir algo como {% raw %}`{{ cookiecutter.autor }}`{% endraw %}.

- Para **usar estructuras de control** como if o for, encierras el código en corchetes y signos de porcentaje: {% raw %}`{%...%}`{% endraw %}. Dependiendo de la estructura de control, también vas a tener que cerrar con un {% raw %}`{% endif %}`{% endraw %} o {% raw %}`{% endfor %}`{% endraw %}. Por ejemplo:

   {% raw %}
  ```jinja
  {% if cookiecutter.condicion %}
  ¡Muestra esto!
  {% endif %}
  ```
   {% endraw %}
  

- Para **incluir un comentario**, encierras el contenido en corchetes y numerales: {% raw %}`{#...#}`{% endraw %}: {% raw %}`{# Esto es un comentario #}`{% endraw %}.

## Creación de la plantilla

Teniendo lo anterior, vas a crear una nueva carpeta, que en adelante llamaré la carpeta madre, y dentro de ella una con este nombre: {% raw %}`{{ cookiecutter.project_slug }}`{% endraw %}. Ahora que entiendes la sintaxis de Jinja, ya puedes intuir que esta última se va a llamar según el contenido de la variable `project_slug`. Más adelante veremos la configuración de esta y otras variables.

Ahora, dentro de esa carpeta {% raw %}`{{ cookiecutter.project_slug }}`{% endraw %} que creaste vas a organizar la estructura de tu espacio de trabajo: carpetas y subcarpetas, `.gitignore`, un `README.md`, cuadernos de jupyter, etc. A partir de ésta es que se generará tu proyecto.

Al interior de los archivos que crees vas a poder trabajar con los bloques de Jinja que viste antes. Por ejemplo, para que el `README.md` de tu proyecto tenga un nombre y una descripción personalizada, puedes escribir en ese archivo algo como:

{% raw %}
```md
# {{ cookiecutter.project_title }}

Autor: {{ cookiecutter.project_author_name }}

{{ cookiecutter.project_description }}
```
{% endraw %}

## Configuración de variables

Naturalmente, para que la plantilla funcione todas las variables que llamas tienen que existir. Para ello, en la carpeta {% raw %}`{{ cookiecutter.project_slug }}`{% endraw %} tienes que crear un archivo llamado `cookiecutter.json`. Usando líneas como las que están a continuación vas a enumerar las variables para tu proyecto, que a su vez serán las que la consola te va a preguntar cuando generes un proyecto a partir de la plantilla.

{% raw %}
```json
{
	"project_title": "Mi plantilla",
	"project_slug": "{{ cookiecutter.project_title.lower().replace(' ', '_').replace('-', '_') }}",
	"project_description": "Acá va la descripción.",
	"project_author_name": "Tu nombre"
}
```
{% endraw %}

En esta configuración, tenemos una variable para el título del proyecto, una variable para un apodo del proyecto que se genera automáticamente con un par de funciones de Python, una descripción para el proyecto y el nombre de quien lo creó. Si no añades información en la consola, éste tomará los valores por defecto que aparecen arriba.

Con lo anterior, puedes configurar comportamientos tan complejos como quieras. Por ejemplo, podrías crear una variable que pregunte si quieres incluir ciertas librerías de Python en tu proyecto e incluir unos bloques {% raw %}`{% if ... %}` `{% endif %}`{% endraw %} en un archivo `environment.yml` para luego instalarlas con Conda.

El límite es tu creatividad y lo granular que quieras llegar a ser con el comportamiento de la plantilla.

## Hooks

Finalmente, Cookiecutter también incluye una funcionalidad para ejecutar código antes y después de generar el proyecto con base en la plantilla. A esto se le llama _Hooks_.

En síntesis, son líneas de código de Python que incluyes en scripts llamados `pre_gen_project.py` o `post_gen_project.py` - dependiendo el comportamiento que quieras que hagan - y que se alojan en una carpeta llamada `hooks` en la carpeta madre.

> ¡Ojo! Estos scripts NO van en la carpeta {% raw %}`{{ cookiecutter.project_slug }}`{% endraw %}.
{: .prompt-warning }

Usando hooks, puedes hacer cosas como [generar automáticamente un repositorio de Git](https://stackoverflow.com/questions/38556622/create-a-git-versioned-project-with-cookiecutter):

```python
import subprocess

subprocess.call(['git', 'init'])
subprocess.call(['git', 'add', '*'])
subprocess.call(['git', 'commit', '-m', 'Initial commit'])
```

## Conclusión

Luego de configurar tu plantilla, puedes conservar la carpeta madre en tu máquina local o subirla a un repositorio de github para llamarla de manera remota. Si la plantilla quedó creada correctamente, vas a poder crear un proyecto con base en ella corriendo el siguiente comando en consola:

```bash
# Si está en tu máquina local
cookiecutter <Ruta de Archivo>

# Si está en Gitub
cookiecutter <URL del repositorio>
```

Espero que te animes a crear tus propias plantillas y te deseo buena suerte experimentando con esta herramienta. Si te pareció de utilidad esta entrada no olvides compartir o dejarme un comentario.
