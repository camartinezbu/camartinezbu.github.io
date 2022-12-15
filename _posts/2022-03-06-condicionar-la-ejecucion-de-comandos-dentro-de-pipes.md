---
title: Condicionar la ejecución de comandos dentro de pipes
date: 2022-03-06 10:30:00 -0500
categories: R
tags: [r, pipe, condicional, turorial]
image: 
  path: https://raw.githubusercontent.com/camartinezbu/blog-images/main/posts/2022-03-06-condicionar-la-ejecucion-de-comandos-dentro-de-pipes/hero.png
excerpt: Cuando termines de leer vas a querer reescribir tu código
---

Buena parte del tiempo que pasamos trabajando con el `tidyverse` lo hacemos usando el operador pipe (`%>%`). Este operador nos permite fácilmente incluir la salida de una función como argumento de la siguiente, y de esta manera encadenar comandos para realizar modificaciones complejas a una base de datos con una gran legibilidad.

Sin embargo, ¿qué sucede si quiero supeditar la ejecución de alguno de los comandos del pipe a una condición? Esto es lo que les voy a mostrar en el blog de hoy.

## Un repaso rápido de los pipes

El pipe viene del paquete `magrittr`, que se carga automáticamente cuando llamamos al `tidyverse` en nuestro código. Si ya has trabajado antes en análisis de datos con R, seguro el siguiente código es familiar:

```r
tabla_resumen <- data %>%
  group_by(columna1) %>%
  summarise(total = sum(columna2)) %>%
  arrange(desc(total)) %>%
  head(10)
```

Para un dataframe arbitrario, es claro ver que estoy agrupando por una columna llamada `columna1`, sumando para cada valor de esa columna los valores de `columna2` y asignándolo a la columna `total`, ordenandola de mayor a menor y asignando las primeras 10 filas a un dataframe llamado `tabla_resumen`. Si no usaramos el pipe y creáramos objetos intermedios para cada operación mencionada, tendríamos que hacer algo del estilo:

```r
datos_agrupados <- group_by(data, columna1)

datos_resumidos <- summarise(datos_agrupados, total = sum(columna2))

datos_ordenados <- arrange(datos_resumidos, desc(total))

tabla_resumen <- head(datos_ordenados, 10)
```

Como puedes ver, la implementación a través del pipe es más ordenada y fácil de leer, y nos ahorra la necesidad de crear y almacenar objetos en memoria que no vamos a necesitar.

## ¿Cómo entran los condicionales?

Trabajar con las estructuras de control de `if` es relativamente sencillo en R base, usando la sintaxis:

```r
if (condicion) {
  #comandos
}
```

Adicionalmente, el uso de condiciones dentro de los pipes no es ajeno para el `tidyverse`. Funciones como `case_when()` permiten aplicar una serie de condiciones de manera abreviada para tareas como modificar una columna de la base de datos. Por ejemplo, si quieremos separar los valores de una columna con base en los cuartiles, podemos hacer algo del estilo:

```r
datos %>%
  mutate(segmento = case_when(
    columna1 <= quantile(columna1, .25) ~ 'segmento1',
    columna2 > quantile(columna1, .25) & columna2 <= quantile(columna1, .5) ~ 'segmento2',
    columna2 > quantile(columna1, .5) & columna2 <= quantile(columna1, .75) ~ 'segmento3',
    TRUE ~ 'segmento4'
  ))
```

Ahora bien, qué pasa si queremos aplicar el condicional sobre la *ejecución misma* de alguno de los comandos dentro de un pipe. Lo primero que se viene a la cabeza es simplemente incluir la estructura de un `if` dentro del pipe ¿verdad? Sigamos el ejemplo de la base de datos arbitraria de antes. Supongamos que en vez de agrupar siempre, queremos definir una condición para decirle al pipe cuándo si y cuando no ejecutar esa línea de código. La idea de aplicar el `if` directamente sería algo como:

```r
tabla_resumen <- data %>%
  if (condicion == TRUE) {
    group_by(columna1)
  } %>%
  summarise(total = sum(columna2)) %>%
  arrange(desc(total)) %>%
  head(10)
```

Sin embargo, el paquete `magrittr` no sabe qué hacer con esta manera de escribir la instrucción y devolverá un error. En ese sentido, vamos a hacer tres cambios para que funcione y sea fácil de leer dentro del pipe.

### if...else de una línea

Lo primero es ajustar la manera como está escrito el `if...else`. Si bien la manera en que usualmente se encuentra su sintaxis tiene varias líneas, también podemos escribirla en una sola línea. Los dos comandos siguientes funcionan exactamente igual:

```r
# Varias líneas
if (condicion == TRUE) {
    print('La condición se cumple')
  } 

# Una línea
if(condicion == TRUE) print('La condición se cumple')
```

Nota como el cambio entre los dos es que se ahorra el uso de unos corchetes. Sólo por el hecho de que la función print no está entre los primeros paréntesis, R entiende que esa es el comando a ejecutar en caso que la condición sea cierta y no parte de la condición. 

Lo anterior también aplica si tenemos un `else`:

```r
# Varias líneas
if (condicion == TRUE) {
    print('La condición se cumple')
  } else {
    print('La condición no se cumple')
  }

# Una línea
if(condicion == TRUE) print('La condición se cumple') else print('La condición no se cumple')
```

En estricto sentido, este cambio no va a hacer que el `if...else` funcione diferente dentro del pipe. Sin embargo, en mi opinión escribirlo en una sóla línea ayudará a que se vea mejor en esta serie de comandos y facilitará su lectura.


### Corchetes

Lo siguiente es que `magrittr` necesita saber que ese `if...else` hace parte del pipe y lo puede encadenar a los demás comandos. Para ello, tenemos que encerrarlo en corchetes. Es decir, con los dos cambios vistos hasta el momento implican que la operación de agrupar según una condición se escribiría como:

```r
tabla_resumen <- data %>%
  { if (condicion == TRUE) group_by(columna1) } %>%
  summarise(total = sum(columna2)) %>%
  arrange(desc(total)) %>%
  head(10)
```

No obstante, esto también nos arrojará error. Si bien ya `magrittr` entiende que esto es un comando a encadenar, sigue sin saber *qué* encadenar con los siguientes comandos.

### Ajustes finales

Esto sucede por dos motivos. El primero es que no hemos definido qué sucede en ambos escenarios posibles: cuando la condición es cierta como cuando es falsa. El segundo es que internamente los `pipes` y las funciones del `tidyverse` tienen una manera de indicarle a las funciones dónde entran los resultados de las operaciones anteriores, que tenemos que llamar explícitamente dentro del `if`.

Con respecto al primer motivo, deberíamos tener entonces un código del siguiente estilo:

```r
tabla_resumen <- data %>%
  { if (condicion == TRUE) 'salida1' else 'salida2' } %>%
  summarise(total = sum(columna2)) %>%
  arrange(desc(total)) %>%
  head(10)
```

El segundo motivo merece una explicación un poco más detallada. Al revisar la documentación de las funciones del `tidyverse` se puede ver que tienen un argumento que llamado `.data`. Este le indica a las funciones cuál es el dataframe sobre el que tienen que realizar la operación específica. Cuando no se está usando un pipe, tenemos que llamarlo de manera explícita como el primer argumento. Cuando usamos un pipe, éste se encarga de que el dataframe que venía antes sea ese `.data` y de esta manera encadenar los comandos. Por tal razón, estos dos elementos son exactamente iguales:

```r
# Sin pipe
head(data, 10)

# Con pipe
data %>%
  head(10)
```

Como ahora tenemos dos posibilidades de qué hacer según la condición, para implementar el condicional dentro del pipe tenemos que definir explícitamente el nombre de lo que iría en ese `.data` con un placeholder. De esta manera, para ambas posibilidades del `if...else`, el pipe sabrá que hacer y podremos seguir con la ejecución del comando. Para ello, vamos a usar el caracter de punto `.`.

Uniendo todo lo anterior, tenemos que la solución para implementar el ejemplo anterior sería algo como:

```r
tabla_resumen <- data %>%
  { if (condicion == TRUE) group_by(., columna1) else . } %>%
  summarise(total = sum(columna2)) %>%
  arrange(desc(total)) %>%
  head(10)
```

Lo que quiere decir esta nueva sintaxis es que si la condición es cierta, vamos a tomar el dataframe `data` -referenciada dentro de la función como `.` y lo vamos a agrupar de acuerdo con la `columna1`; y si la condición es falsa, vamos a devolver el mismo dataframe anterior, es decir `data`. Culquiera que sea la salida de este `if...else`, será un dataframe que se podrá inttroducir en la siguiente línea y ejecutar el resto de código con normalidad.

## Extra

Como ejercicio adicional, se podría incluir esta operación al interior de una función e incluir la condición como un argumento de dicha función:

```r
crear_tabla_resumen <- function(dataframe, agrupar = TRUE) {
  output <- dataframe %>%
    { if (agrupar == TRUE) group_by(., columna1) else . } %>%
    summarise(total = sum(columna2)) %>%
    arrange(desc(total)) %>%
    head(10)

  return output
}

# Agrupando por la columna 1
tabla_resumen1 <- crear_tabla_resumen(data, agrupar = TRUE)

# Sin agrupar por la columna 1
tabla_resumen2 <- crear_tabla_resumen(data, agrupar = TRUE)
```

Espero que te haya servido este tutorial y que puedas utilizar condicionales dentro de los pipes en R. Puedes leer más sobre su funcionamiento en este [capítulo](https://r4ds.had.co.nz/pipes.html) del líbro de R para Ciencia de Datos de Hadley Wickham.
