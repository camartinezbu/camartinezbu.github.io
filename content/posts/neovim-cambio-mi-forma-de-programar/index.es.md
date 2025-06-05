---
title: Neovim cambió mi forma de programar
date: 2025-06-05 11:20:00 -0500
categories: [reflexiones]
tags: [paquetes, programación, editor, neovim]
excerpt: Dejé incluso a RStudio atrás.
---

Durante varios años he usado RStudio para programar en R y cuadernos de Jupyter para programar en Python. Tanto para tareas de trabajo como para la clase de Métodos Computacionales para Políticas Públicas, estas herramientas funcionaron  bien. Pero algunos meses atrás descubrí un lado de Youtube en el que varios creadores de contenido se la pasaban hablando de un editor llamado Neovim y quedé impresionado con la velocidad con la que podían usarlo para moverse en el código.

Hoy escribo código tecleando mil veces `jkkjjkkl`, modifico plugins en `lua` para hacer exactamente lo que quiero que hagan, y ahora, comparto lo chévere que es aprender sobre él.

Neovim es un editor de texto que se ejecuta en la terminal del computador. La aplicación sucesora del editor de texo vim, que a su vez es una modificación de vi, cuyo origen se remota a mediados de los 70s. Es muy famoso por el meme de que, como principiante, uno nunca sabe cómo salir del editor. (Pista, es con la combinación de teclas `:q`).

El editor tiene varios modos, uno en el que uno se puede mover en el texto (`NORMAL`), uno en el que se modifica el contenido del texto (`INSERT`), otro en el que uno puede seleccionar visualmente partes del texto (`VISUAL`), entre otros. La idea no es que esto se vuelva un tutorial de Neovim; para eso hay muchos recursos y tutoriales en internet. Pero vale la pena la introducción para contextualizar lo que viene: la forma en la que he cambiado mi forma de programar.

Sí, las vim motions tienen una curva de aprendizaje brutal. Los primeros días no me hallaba y era mucho más lento a la hora de escribir código. Varias veces cerré la terminal y me devolví a RStudio a recuperar la velocidad perdida. De hecho, durante un tiempo ni siquiera usé Neovim como tal. El mismo RStudio tiene una opción para poder moverse con los comandos básicos de vim y fue una manera menos traumática de familiarizarme con los comandos para moverme en el código. Hay más aplicaciones con esta opción de las que uno se imagina.

Pero el punto es que si bien al inicio uno tiene que pensar muy conscientemente sobre las teclas que está oprimiendo para hacer cualquier cosa, la memoria muscular se va adaptando y la mente deja de pensar en esos movimientos mecánicos. Uno termina iterando más rápido, temiéndole menos a hacer _refactors_ del código, y gastando menos tiempo luchando con el editor.

Superado ese primer obstáculo, volví a Neovim con más convicción. De a pocos fui entendiendo cómo se configuraba el editor con archivos de texto plano escritos en un lenguaje que se llama `lua`. Si uno ha escrito python, es relativamente fácil entender qué es lo que está pasando. No pretendo ser el más conocerdor del lenguaje, pero me defiendo.

Algo muy chévere de esto es que uno no es tan consciente de que cuando usa herramientas como RStudio o Jupyter, uno está anclado a muchas decisiones que tomaron otros sobre cómo funciona el entorno de trabajo. Hay muchas que son buenas y que son intuitivas, pero hay muchas otras que uno termina sin usar nunca. Con un poco de curiosidad y siguiendo tutoriales de personas como ThePrimeagen o TJ DeVries, uno puede quedar con un buen entendimiento de la herramienta y con una configuración básica para trabajar.

Eso si, la inversión inicial en tiempo es considerable. Al principio pasé horas cacharreando con la configuración y causando mil errores en el editor para entender qué estaba pasando. Pero el resultado fue algo bien chévere: ahora entiendo (en alto nivel) qué es eso de los Language Server Protocol –LSP–, cómo configurar un _linter_ fuera del contexto protegido de un IDE, aprendí sin querer de varios comandos de la terminal para varias cosas como `stow`, que me permite replicar mi configuración en más computadores fácilmente o `tmux`, que deja configurar sesiones de la terminal y básicamente reemplaza los proyectos de RStudio, entre otras. Cosas que trascienden la edición de código y que le enseñan a uno a pensar sobre herramientas, sobre el diagnósitco de problemas complejos, sobre buenas prácticas a la hora de trabajar.

Yo siento que quienes terminamos en análisis de datos, viniendo de áreas cuyo núcleo no era la programación, tenemos una falencia al no salir de RStudio o los cuadernos de Jupyter. Nos aislamos del funcionamiento del resto de la máquina e ignoramos muchas cosas que nos pueden servir para facilitarnos la vida. En mi caso, Neovim terminó siendo la excusa para salir de a pocos de ese contexto.

Acá el llamado a la acción no es "Desinstala el editor, dedíca horas a configurar una aplicación enredada en la terminal y pierde productividad por dos semanas". Mas bien: "Revisa si la herramienta que usas tiene vim-motions y prueba". Esas horas dedicadas a la configuración vienen más tarde, cuando uno se da cuenta que ser competente en Neovim no solo es útil sino divertido.
