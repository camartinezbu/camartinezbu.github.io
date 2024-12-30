---
title: Lo que aprendí escribiendo un paquete
date: 2024-12-30 15:50:00 -0500
categories: [reflexiones]
tags: [paquetes, programación]
excerpt: Ahora tengo una nueva apreciación por la programación.
---

Mucho se suele escribir sobre cómo hacer cosas, pero no tanto frente a por qué hacerlo. Yo mismo he escrito varios tutoriales para [crear dashboards con shiny](), [trabajar con fechas en R]() y [python](), [mejorar los gráficos con ggplot2](), entre otros. Pero hoy quiero escribir algo diferente y hacerte una invitación para que escribas tu propio paquete en el lenguaje de programación que uses.

Desde la última vez que escribí por acá he crecido un montón como desarrollador. He aprendido más sobre mis herramientas de trabajo, he experimentado con la configuración de mi entorno de trabajo de programación y me he acercado a otros lenguajes de programación como Go.

Mi esfuerzo más reciente ha sido optimizar –de una vez por todas– la manera en la que trabajo con cifras de crímen en Colombia. Cada vez que tenía que generar reportes con base en estas cifras escribía nuevas funciones o tenía que desempolvar _scripts_ viejos para cargar, procesar y generar visualizaciones de datos.
En vez de facilitarme la vida, estaba haciendo el mismo esfuerzo una y otra vez.

Después de acumular suficiente frustración, decidí de una vez por todas aprender sobre la creación de paquetes en R. El punto de partida fue el libro de [R Packages](ihttps://r-pkgs.org) de Hadley Wickham –por supuesto– y Jennifer Bryan. Es un recurso maravilloso y lo recomiendo un montón por si tienen una frustración como la mía.

Más allá de las respuestas obvias –organizar mejor el código y facilitar los productos que tengo que armar–, me dí cuenta de una dimensión completamente nueva del lenguaje de programación, que creo que va a ayudarme a crear código más legible y que se rompa menos a menudo.

Para el caso puntual de paquetes en R, he tenido que aprender desde trucos para lidiar con avisos y notas como resultado del `R CMD check` por la evaluación no estándar que suele usar el Tidyverse, el uso de [`roxygen`](https://roxygen2.r-lib.org) para documentar funciones y datos, la implementación de _assertions_ con el paquete [`assertthat`](https://github.com/hadley/assertthat), la creación de pruebas unitarias para funciones de la mano de [`testthat`](https://testthat.r-lib.org), entre muchas otras cosas.

La verdad sea dicha, esto se siente como una dimensión completamente nueva de la programación. Una cosa es programar en clave de unir funciones que otras personas crearon para lograr alguna tarea puntual y otra es tener que crear esas funciones en primer lugar. Es muy distinto armar un set de Lego con piezas creadas que diseñar nuevas piezas.

Por ahora, el paquete que estoy creando sigue en desarrollo. Me hace falta un camino largo para asegurarme que tengo las pruebas adecuadas para verificar el funcionamiento del paquete en casos borde. Además, tengo pendiente mejorar la documentación y, quizás, eliminar algunas dependencias que otros paquetes que solo uso muy poco.

Terminar este proyecto me inspira un montón porque al final del recorrido, esto me permitirá implementar tableros de visualización, reportes o aplicaciones de línea de comandos mucho más rápido.

Si estás aprendiendo a programar en R, o en cualquier otro lenguaje, vas a tener un entendimiento más profundo de la herramienta que usan y tendrás una mayor apreciación por el trabajo de las demás personas que escriben los paquetes que ustedes usas en el día a día. En el peor de los casos, tendrás un repositorio privado más en Github. En el mejor, tendrás un proyecto interesante para mostrar o incluso un producto para ofrecer.

Así que anímate a ejecutar por primera vez `usethis::create_package()` o el equivalente en tu lenguaje de preferencia. No te vas a arrepentir.
