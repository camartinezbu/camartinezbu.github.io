---
title: Descubre cómo usar ChatGPT para ciencia de datos
date: 2023-05-18 18:50:00 -0500
categories: [ChatGPT]
tags: [tutorial, trucos, tips, prompts]
image: 
  path: /posts/2023-05-18-descubre-como-usar-chatgpt-para-ciencia-de-datos/hero.jpg
excerpt: Sácale provecho a esta herramienta para potenciar tu aprendizaje o tu flujo de trabajo.
---

Las herramientas basadas en modelos grandes de lenguaje (LLM – *Large Language Models*) han estado en los últimos meses en el foco de la conversación. No sólo por las capacidades que tienen para generar textos de todo tipo de temas, sino también por la explosión en el número de modelos disponibles, tanto de empresas privadas como de iniciativas *open source*.

No es sorpresa entonces que estos modelos pueden convertirse en una adición valiosa a tu caja de herramientas, ayudándote a resolver problemas, obtener ideas y optimizar tu proceso de análisis de datos. A continuación, quiero darte algunos consejos generales sobre cómo usar uno de estos modelos –ChatGPT– y proponerte algunos ejemplos específicos sobre cómo aprovecharlo al máximo en tu flujo de trabajo de ciencia de datos.

## Un aviso preliminar

Aunque ChatGPT puede ser una herramienta útil para obtener respuestas rápidas y explorar ideas, no olvides que no reemplaza las herramientas de búsqueda convencionales. Aunque ChatGPT puede ofrecerte información relevante y, a menudo, orientarte en la dirección correcta, sus respuestas pueden no ser tan precisas o completas como las que puedes obtener de otras fuentes.

A la par de los consejos que te daré más adelante, no dependas exlusivamente de esta herramienta. Por el contrario, complementa su uso con la búsqueda de documentación, foros de discusión como StackOverflow o recursos especializados de ciencia de datos.

Esto te permitirá tener una comprensión más sólida de los conceptos, técnicas y enfoques relacionados con tus proyectos, y a la vez, te da la posibilidad de sacarle aún más provecho a ChatGPT.

Dicho lo anterior, ahora sí vamos a ver los consejos generales para usar esta herramienta.

## Consejos generales

### Sé claro y preciso

Al interactuar con herramientas de chat como ChatGPT, es muy importante que le des instrucciones claras y específicas para obtener los resultados deseados. Para esto ayuda mucho que estés famililarizado con el tema de tu interés, porque conoces los conceptos y las palabras específicas de este tema y así podrás obtener respuestas de mayor utilidad.

> Por ejemplo, en vez de preguntar algo como "¿Cómo puedo mejorar mi modelo de clasificación?", intenta ser más específico: "Dame tres técnicas efectivas para mejorar la precisión de mi modelo de clasificación en Python".
{: .prompt-tip}

### Da contexto sobre lo que estás haciendo

Cuando interactúes con ChatGPT, dale algunas oraciones de contexto sobre lo que vas a hacer. Intenta describir brevemente el problema que estás abordando, los datos que tienes disponibles y cualquier información adicional de contexto que consideres relevante. Esto ayudará a que la respuesta que obtengas esté más alineada con tu entrada.

> Por ejemplo, en vez de escribir "Explícame cómo hacer una regresión logística", intenta "Necesito implementar una regresión logística en Python para resolver un problema de clasificación binaria de un conjunto de datos con la variable objetivo y 5 variables de explicación. Dame el código de Python y explícame cómo funciona".
{: .prompt-tip}

### Describe el formato de salida esperado

Si necesitas un formato particular para tu resultado o que este cumpla con alguna métrica puntual, indícalo en el prompt. Esto puede ser desde un número de palabras o párrafos deseados en la respuesta, hasta una lista explícita de cómo quieres tus resultados.

> Por ejemplo, en vez de preguntar: "¿Cómo lidiar con imágenes de distintos tamaños para un problema de clasificación de imágenes en deep learning?", te sugiero intentar: "¿Cómo puedo lidiar con imágenes de distintos tamaños para un problema de clasificación de imágenes en deep learning? Por favor devuélveme la respuesta en el siguiente formato: - Acercamiento número N, - Título, - Descripción, - Pros, - Contras, - Implementación de código".
{: .prompt-tip}

### Juego de rol: Experto en ciencia de datos

Finalmente, sobre todo cuando quieres obtener explicaciones de algún concepto o descripciones paso paso de algún código, suele servir que le indiques a ChatGPT que asuma el rol de algún experto en un tema específico y hacerle preguntas desde esa perspectiva.

> Por ejemplo, en vez de preguntarle: "¿Qué buenas prácticas existen para el análisis exploratorio de datos?", intenta con "Eres un experto en análisis exploratorio de datos y necesito recomendaciones sobre las mejores prácticas y las visualizaciones más efectivas para identificar correlaciones en un conjunto de datos. ¿Qué me sugieres?".
{: .prompt-tip}

## Usos en Ciencia de Datos

Ahora vamos a explorar algunos casos de uso específicos de ChatGPT para tareas de ciencia de datos. El objetivo de esta sección es que te inspires para escribir prompts que te ayuden en tu día a día.

### Ejemplo 1.: Explicación de expresiones regulares

Las expresiones regulares son herramientas muy poderosas a la hora de trabajar con textos, pero a simple vista, pueden ser complejas de leer. Especialmente si estás lidiando con expresiones muy largas o complejas.

Podrías intentar darle un ejemplo de expresión regular y hacerle preguntas a ChatGPT como:

- "¿Podrías explicar cómo funciona esta expresión regular?"
- "¿Hay alguna forma más eficiante de escribir esta expresión regular para obtener el mismo resultado?"
- "¿Cuáles son los posibles casos de uso donde esta expresión regular podría fallar?"

### Ejemplo 2: Ayuda con pandas y manipulación de DataFrames

Si estás trabajando con esta librería de Python y tienes dudas sobre cómo realizar una operación específica en un DataFrame, puedes usar ChatGPT para resolverlas. Recuerda los consejos que te di al inicio del post y dale contexto sobre la estructura de tu DataFrame y describe la tarea que quieres reallizar.

Puedes preguntarle cosas como:

- "¿Puedes sugerirme cómo aplicar una función específica a una columna en mi DataFrame de Pandas?"
- "¿Cuál es la mejor forma de filtrar los datos en función de una condición en una columna específica de mi DataFrame?"
- "¿Puedes darme un ejemlo de cómo hacer un análisis exploratorio básico de mi DataFrame utilizando las funciones de pandas?"

### Ejemplo 3: Optimización de hiperparámetros

Al entrenar modelos de Machine Learning, es fundamental encontrar los hiperparámetros óptimos para mejorar el rendimiento de tus modelos. Puedes utilizar CHatGPT para obtener sugerencias y consejos sobre cómo ajustar los hiperparámetros de un modelo en el que estés trabajando. Sin embargo, no olvides consultar directamente la documentación y otros recursos.

Para esta tarea, podrías preguntarle cosas como:

- "¿Podrías indicarme qué hiperparámetros puedo ajustar para este modelo de clasificación?"
- "¿Qué técnicas hay para evaluar diferentes combinaciones de hiperparámetros y cómo las implemento en código?"
- "¿Hay alguna forma de automatizar la búsqueda de hiperparámetros óptimos en Python?"

### Ejemplo 4: Extracción de datos mediante Web Scrapping

Puedes utilizar ChatGPT para obtener ayuda con la implementación de Web Scraping o resolver dudas específicas que tengas con esta tarea.

Algunas preguntas que podrías hacer son:

- "¿Podrías sugerirme algunas librerías de Python para realizar Web Scraping?"
- "¿Cómo puedo extraer datos de una tabla específica en una página web usando Web Scraping?"
- "¿Qué son los navegadores fantasma en el contexto de Web Scraping?"

### Ejemplo 5: Procesamiento de Lenguaje Natural (NLP)

Si has trabajado con texto sabes que hay una gran cantidad de librerías que te permiten cumplir tareas de procesamiento de lenguaje natural. En esta aplicación, ChatGPT también puede ser útil para obtener consejos o para prototipar rápidamente.

Puedes intentar preguntarle:

- "¿Existen librerías pre-entrenadas que pueden ayudame a clasificar automáticamente el sentimiento de un texto?"
- "¿Cuáles son las distintas aproximaciones paras tokenizar un texto en procesamiento de lenguaje natural? ¿sus pros y sus contras?"
- "Supón que tengo un DataFrame con oraciones en español. ¿Cómo se vería el código para hacer detección de entidades nombradas?"

## Conclusión

Como pudiste ver a lo largo de este post, escribir los prompts para modelos de lenguaje que interactúan a través de chat –como ChatGPT– es todo un arte. Si aplicas estos trucos, puedes mejorar la forma en la que estos modelos te ayudan a resolver problemas, prototipar ideas y optimizar tu flujo de trabajo. Sin embargo, no olvides complementar su uso con otras fuentes de información y motores de búsqueda para obtener respuestas más precisas.

Espero que estos tips te sirvan para sacar el máximo provecho de ChatGPT en el campo de la ciencia de datos. Te invito a compartir estos tips con más personas y a experimentar con distintas formas de escribir tus prompts. ¡Éxitos en tu aprendizaje!
  