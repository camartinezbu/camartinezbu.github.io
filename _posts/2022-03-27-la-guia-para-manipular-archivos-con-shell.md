---
title: La guía para manipular archivos con shell
date: 2022-03-27 21:10:00 -0500
categories: Terminal
tags: [shell, bash, zsh, tutorial]
image: 
  path: /posts/2022-03-27-la-guia-para-manipular-archivos-con-shell/hero.png
excerpt: Crea y elimina archivos usando la línea de comandos de tu computador.
---

En tu camino de aprendizaje de ciencia de datos, seguro has visto artículos o tutoriales en los que se emplean comandos de texto para navegar a través de las carpetas de tu computador, crear o eliminar archivos, descargar archivos de internet, interactuar con `git`, entre otras.

Al principio estos comandos pueden parecer muy complejos, pero en el post de hoy quiero compartir contigo una guía básica para su utilización y que puedas desbloquear el potencial de tu computador.

## ¿Qué es el *shell*?

Un *shell* o línea de comandos, es el programa en tu computador que te permite comunicarte a través de comandos con el sistema operativo. El shell se puede acceder a través de una interfaz gráfica que se denomina *terminal.*

Dependiendo del sistema operativo vas a encontrar distitos tipos de shell:

- Para Linux y versiones de MacOS anteriores a Catalina, el shell por defecto es Bash.
- Para versiones de MacOS desde Catalina, el shell es Z Shell (o Zsh).
- Para Windows, el shell predeterminado es PowerShell.

> [Windows Susbsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/about) te permite ejecutar de manera nativa comandos de bash en windows. Puedes encontrar más información sobre su instalación [aqui](https://docs.microsoft.com/en-us/windows/wsl/install).
{: .prompt-info}

En este tutorial me voy a enfocar específicamente en algunos comandos básicos de Bash y Zsh, cuyo uso es muy parecido. Todos los comandos que se mencionan abajo se van a ejecutar desde la Terminal, por lo que deberías abrirla antes de empezar.

## Navegar entre carpetas

Al igual que cuando trabajas normalmente en tu computador a través de el Explorador de arvhicos o Finder, en el shell puedes desplazarte entre carpetas, crear carpetas nuevas, crear archivos, entre otras. Vamos a ver los comandos para hacer lo anterior.

Lo primero es preguntarle al sistema operativo en qué carpeta estamos actualmente. Esto lo podemos hacer ejecutando el comando `pwd`, o *present working directory.* la Terminal te va a devolver una ruta de archivo cuando ejecutas este comando, que por defecto será el directorio de Inicio. En mi caso, este es `/Users/camilomartinez`.

Ahora sigue cambiar ese directorio de trabajo. Usando el comando `cd` seguido de una ruta de archivo, podrás moverte de carpeta en carpeta. Por ejemplo, estando en el directorio de inicio, si ejecutas el comando `cd Downloads` vas a pasar a tu carpeta de descargas.

## Una nota sobre las rutas de archivos

El ejemplo anterior es ilustrativo de lo que puedes hacer con los comandos, pero en la mayoría de los casos no te vas a encontrar con turas de archivo tan simples. Por lo anterior, hay algunas cosas clave que tienes que saber antes de continuar.

Lo primero es que las rutas de archivo se definen separando los nombres de la carpeta con la barra o *slash* (`/`). La ruta de archivos que mencioné arriba es entonces la carpeta `camilomartinez` dentro de la carpeta `Users`. Estas rutas pueden ser tan largas o tan cortas como quieras, teniendo múltiples carpetas dentro de carpetas.

Lo segundo es que existen dos símbolos especiales para hacer referencias relativas a carpetas. Cuando estamos escribiendo una ruta de archivo, `.` hace referencia al directorio actual en el que estamos trabajando y `..` al directorio padre.

A modo de ejemplo, supongamos que estoy en la ruta por defecto `/Users/camilomartinez`. Si hay un comando que me pide definir la ruta de archivo - como los que verás más adelante-, `.` hace referencia a la ruta `/Users/camilomartinez` y `..` hará referencia a `/Users`, porque es la carpeta que contiene a `/camilomartinez`.

Por último, arriba mencioné que existe un directorio por defecto. El caracter `~` te permitirá hacer una referencia a ese directorio sin importar donde te encuentres.

Por ejemplo, supón que estás en la ruta `/Users/camilomartinez/Downloads/mi_carpeta` y quieres ir a la carpeta de documentos de tu directorio principal. En vez de escribir varias veces `..` después del comando `cd`, basta con que escribas `cd ~/Documents`.

## Ver el contenido de una carpeta

Ahora que sabes navegar entre las carpetas de tu computador, el paso siguiente es ver el contenido de dichas carpetas. Para ello, ubicandote en la carpeta deseada, vas a ejecutar el comando `ls -lh`. La Terminal te devolverá una lista de carpetas y archivos con sus respectvos nombres y peso.

Notarás que hay algo nuevo en este comando: las dos letras precedidas por un guión. Esto es algo que verás de manera recurrente en los comandos y son opciones adicionales para modificar su comportamiento. Estas se denominan banderas o *flags*.

En este caso, la bandera `l` hace que la salida del comando sea en una lista y la bandera `h` a que los pesos de los archivos sean más fáciles de leer.

> Como te puedes dar cuenta, `-lh` no hace referencia a una sóla bandera sino a dos separadas. Sin embargo, el shell permite abreviar varias banderas usando un sólo guión como te lo mostré anteriormente. En estricto sentido, podrías escribir `ls -l -h` y el comando funcionaría igual.
{: prompt-info}

## Crear carpetas y archivos

Ahora que sabes navegar entre las carpetas de tu computador, el paso siguiente es manipular el contenido de dichas carpetas.

Para crear una nueva carpeta en la ubicación deseada, ejecutas el comando `mkdir` seguido el nombre de la carpeta. Por ejemplo `mkdir mi_carpeta`.

También puedes crear un archivo usando el comando `touch`, seguido del nombre y extensión que quieras. Es decir, si quieres crear un archivo de texto plano, debes ejecutar el comando `touch mi_archivo.txt`.

> Intenta crear una carpeta y un archivo dentro, y luego usar el comando `ls -lh` para ver si tuviste éxito.
{: prompt-tip}

## Borrar carpetas y archivos

Puedes hacer ambas operaciones usando el comando `rm`. Para el caso de los archivos, sólo tienes que ir a la ruta donde está el archivo y ejecutar el comando mencionado, seguido del nombre del archivo. Es decir, si quieres eliminar el archivo que creaste arriba, puedes escribir algo como `rm mi_archivo.txt`.

Para las carpetas debes añadir la bandera `-r`, de lo contrario no la podrás eliminar. A modo de ejemplo, para eliminar la carpeta que creaste arriba tienes que ejecutar `rm -r mi_carpeta`. Ten cuidado con la ejecución de este comando. Si eliminas una carpeta se eliminará todo su contenido. Por esta razón, la Terminal te va a preguntar si de verdad quieres eliminar la carpeta antes de hacerlo.

## Conclusión

¡Espero que te haya gustado este post! Con lo que viste, ya puedes navegar fácilmente en las carpetas de tu computador sin necesidad de usar la interfaz gráfica de tu sistema operativo.

En las siguientes semanas voy a publicar una serie de entradas sobre cómo modificar los permisos de los archivos, crear tus propios scripts y modificar variables de entorno en shell.

Te invito a compartir este post si te pareció útil y que sigas aprendiendo a trabajar con el shell.
