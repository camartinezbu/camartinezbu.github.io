---
title: La guía definitiva para escribir ecuaciones en Jupyter
date: 2022-07-19 16:15:00 -0500
categories: [Jupyter, Tutorial]
tags: [jupyter, ecuaciones, mathjax, latex, markdown]
image: 
  path: https://raw.githubusercontent.com/camartinezbu/blog-images/main/posts/2022-07-11-la-guia-definitiva-para-escribir-ecuaciones-en-jupyter/hero.png
excerpt: Aprende sobre el funcionamiento de MathJax.
math: true
---

Escribir ecuaciones como las que puedes encontrar en un libro de texto o una publicación académica parece un asunto complejo. Sin embargo, con MathJax esta tarea se vuelve muy sencilla y al final de este post podrás crear ecuaciones para tus cuadernos de Jupyter y tus documentos de Markdown.

## ¿Qué es MathJax?

MathJax es un motor para la visualización de ecuaciones en Javascript que funciona con la notación de LaTeX, que funciona en todos los navegadores de internet modenos. De acuerdo con la [documentación de este motor](https://docs.mathjax.org/en/latest/basic/mathjax.html):

> **"Uno sólo debe incluir MathJax y matemáticas en una página web, y MathJax se encarga del resto."**

Este motor se usa tanto en [Jupyter](https://jupyterbook.org/content/math.html) como en el [Markdown usado por Github](https://github.blog/2022-05-19-math-support-in-markdown/), así que sin duda va a ser una herramienta muy útil para que aprendas a usarla.

Antes de entrar en detalle sobre las expresiones que te permitirán escribir símbolos para tus ecuaciones, tienes que saber dos cosas: qué es lo que procesa MathJax en ecuaciones y cómo agrupa los elementos en una expresión que escribas.

## Delimitar una ecuación

Lo primero que hay que indicarle a MathJax es cuáles caracteres corresponden a una ecuación que queremos mostrar en pantalla. Para ello, tenemos dos opciones: ecuaciones en la misma línea o ecuaciones en línea propia.

En el primer caso, vamos a mostrar la ecuación siguiendo el mismo renglón, lo que puede servir para mostrar algún símbolo en específico o una ecuación corta. Esto lo hacemos encerrando los caracteres entre símbolos de pesos `$`. Por ejemplo, podemos escribir la ecuación $x=0$ al digitar `$x=0$`.

En el segundo, mostramos la ecuación en un renglón aparte, lo que es especialmente útil para ecuaciones grandes. Lo único que tenemos que hacer es crear una nueva línea y encerrar los caracteres en doble símbolo de pesos `$$`. Entonces, el resultado de escribir la misma ecuación de arriba digitando `$$x=0$$` es: 

$$x=0$$

> Si no dejas estos caracteres encerrados por doble paréntesis en una nueva línea, no se mostrará la ecuación como arriba.
{: .prompt-info }

## Grupos

Lo otro que es importante aprender es la forma en que MathJax lidia con grupos de caracteres. Dado que necesitamos escribir ecuaciones de manera precisa, este motor agrupa elementos usando los corchetes: `{}`. Veamos un ejemplo.

Supón que quieres escribir una constante `a` elevada a una potencia de 20. Lo primero que se te puede venir a la cabeza es escribir algo como `$a^20$`. No obstante, lo que mostraría MathJax sería esto $a^20$. Esto sucede porque el motor lidia con los caracteres uno por uno.

Para mostrar correctamente la potencia, tienes que agrupar los caracteres del 20 de la siguiente manera `$a^{20}$` para tener lo que esperabas: $a^{20}$.

Por este motivo, de ahora en adelante vas a ver de manera recurrente el uso de los corchetes en muchas fórmulas. Ahora si, veamos cómo construir ecuaciones más complejas.

## Operadores básicos

MathJax cuenta con un gran número de símbolos para denotar operadores. Acá te dejo una lista no exhaustiva de ellos, la mayoría de los cuales se escriben con un `\`, seguido de una secuencia específica de caracteres.

### Aritméticos

| Expresión | Salida |
| ---- | ---- |
| `a + b` | $a + b$ |
| `a - b` | $a-b$ |
| `a \times b` | $a \times b$ |
| `a \pm b` | $a \pm b$ |
| `a ^ b` | $a ^ b$ |
| `a \pm b` | $a \pm b$ |

### Comparación

| Expresión | Salida |
| ---- | ---- |
| `a \lt b` | $a \lt b$ |
| `a \gt b` | $a \gt b$ |
| `a \le b` | $a \le b$ |
| `a \ge b` | $a \ge b$ |
| `a = b` | $a = b$ |
| `a \neq b` | $a \neq b$ |

### Conjuntos

| Expresión | Salida |
| ---- | ---- |
| `A \cup B` | $A \cup B$ |
| `A \cap B` | $A \cap B$ |
| `A \setminus B` | $A \setminus B$ |
| `A \subset B` | $A \subset B$ |
| `A \subseteq B` | $A \subseteq B$ |
| `A \subsetneq B` | $A \subsetneq B$ |
| `A \supset B` | $A \supset B$ |
| `A \supseteq B` | $A \supseteq B$ |
| `A \supsetneq B` | $A \supsetneq B$ |
| `A \in B` | $A \in B$ |
| `A \notin B` | $A \notin B$ |
| `\emptyset` | $\emptyset$ |
| `\varnothing` | $\varnothing$ |

### Lógicos

| Expresión | Salida |
| ---- | ---- |
| `p \land q` | $p \land b$ |
| `p \lor q` | $p \lor b$ |
| `\lnot p` | $\lnot p$ |
| `\foraall b` | $\forall p$ |
| `\exists p` | $\exists p$ |
| `\top` | $\top$ |
| `\bot` | $\bot$ |
| `\vdash` | $\vdash$ |
| `\Vdash` | $\Vdash$ |

## Subindices y Superíndices

Cuando escribes ecuaciones te puedes encontrar con una notación que implique añadir información adicional a una variable, como $a_i$, $b^2$ o $\hat{c}$. Los elementos que van sobre la variable se llaman superíndices y los que van debajo subíndices.

Para escribir un subíndice necesitas poner la variable seguida de un `_` y el caracter o caracteres que hacen parte del subíndice. La expresión $a_i$ es el resultado de `$a_i$`.

Por su parte, para los superíndices, debes escribir la variable principal seguida de `^` y el caracter o caracteres que hacen parte de tu superíndice. Para mostrar $b^2$ escribí `$b^2$`.

> No olvides usar correctamente los corchetes. Si tienes varios caracteres en el mismo subíndice o superíndice, es necesario que los encierres con `{}`.
{: .prompt-tip }

Por último, hay algunas funciones que te permiten escribir caracteres especiales como $\hat{c}$ sobre las variables. En la siguiente tabla encuentras algunos de ellos:

| Expresión | Salida |
| ---- | ---- |
| `\hat{x}` o `\widehat{x}` | $\hat{x}$ |
| `\bar{x}` o `\overline{x}`| $\bar{x}$ |
| `\dot{x}` | $\dot{x}$ |
| `\ddot{x}` | $\ddot{x}$ |
| `\vec{x}` | $\vec{x}$ |
| `\overtightarrow{x}` | $\overrightarrow{xy}$ |
| `\overleftarrow{x}` | $\overleftarrow{xy}$ |
| `\overleftrightarrow{x}` | $\overleftrightarrow{xy}$ |

> La diferencia entre `\hat{x}` y `\widehat{x}` es que el primero sólo funciona para un caracter y el segundo para varios. Esto mismo aplica para `\bar{x}` o `\overline{x}`.
{: .prompt-info }

## Fracciones

Te voy a mostrar dos formas para escribir fracciones de este estilo:

$${a\over b}$$

La primera es usando la forma `\frac{a}{b}`, que te permite poner en corchetes aparte el numerador y el denominador. 

La segunda es incluyendo tanto el numerador como el denominador en el mismo grupo con la estructura `{a\over b}`.

En algunos casos puede ser más fácil trabajar de la sergunda forma, especialmente cuando tienes ecuaciones complejas.

Tema aparte son las fracciones contínuas como esta

$$x = a_0 + \cfrac{b_1}{a_1 + \cfrac{b_2}{a_2 + \cfrac{b_3}{...}}}$$

que tienen su propia función: `\cfrac{}{}`. La ecuación anterior la generé usando:

```
$$x = a_0 + \cfrac{b_1}{a_1 + \cfrac{b_2}{a_2 + \cfrac{b_3}{...}}}$$
```

> Las ecuaciones con fracciones contínuas se pueden volver complejas muy rápido. Ten mucho cuidado con el uso de los corchetes y cómo estás encadenando las expresiones `\cfrac{}{}`.
{: .prompt-tip }

## Paréntesis

La manera de escribir paréntesis va a depender de si lo que quieres hacer es envolver elementos que ocupan una o más líneas.

Para una sola línea, basta con escribir los caracteres normales `()`, `[]` o para el caso de los corchetes `\{` y `\}`. Estos símbolos adicionales son para indicarle a MathJax que queremos mostrar un corchete y no que estamos armando un grupo. Así podemos cosntruir cosas como $(a+b)$.

Si hablamos de varias líneas - por ejemplo para fracciones - tenemos que añadir `\left` y `\right` a esos símbolos. Supón que quieres escribir siguiente fracción entre paréntesis:

$${x+1\over 2}$$

Si escribes únicamente `$({x+1\over 2})$` vas a tener el siguiente resultado.

$$({x+1\over 2})$$

Pero si escribes `$\left({x+1\over 2}\right)$` tendrás:

$$\left({x+1\over 2}\right)$$

Aquí algunos símbolos adicionales que se comportan con la misma lógica de los paréntesis:

| Expresión | Salida |
| ---- | ---- |
| `|x|` | $\|x\|$ |
| `\vert x \vert ` | $\vert x \vert$ |
| `\Vert x \Vert` | $\Vert x \Vert$ |
| `\langle x \rangle` | $\langle x\rangle$ |
| `\lceil x \rceil` | $\lceil x \rceil$ |
| `\lfloor x \rfloor` | $\lfloor x \rfloor$ |

## Funciones nombradas

Si bien podrías pensar que sólo necesitas escribir algo como `$sin(x)$`, usualmente las letras de estas funciones no se escriben en cursiva. Por tal motivo, hay expresiones especiales para `\sin`, `\cos`, `\tan` y otras como `\ln`, `\max` y `\min`. Mira la diferencia entre escribir `$sin(x)$` - $sin(x)$ - y `$\sin(x)$` - $\sin(x)$ -.

## Limites

Algo similar sucede con los límites -tienen la expresión `\lim`-, sólo que en este caso es necesario añadir el elemento que va debajo con `_`. Por ejemplo, si quieres escribir la definición del número de Euler

$$\lim_{x\to\infty} \left(1 + {1\over n}\right)^n$$

deberías escribir algo como:

```
$$\lim_{x\to\infty} \left(1 + {1\over n}\right)^n$$
```

> Como pouedes ver, la flecha hacia la derecha que indica hasta dónde va el límite ($\to$) se escribe `\to` y el símbolo de infinito ($\infty$) con `\infty`.
{: .prompt-info }

## Sumas e Integrales

Estos símbolos usualmente incluyen unos elementos arriba y abajo, por lo que tienes que usar de manera simultánea los operadores de subíndice (`_`) y superíndice (`^`). 

Por ejemplo, escribir `$\sum_{n = 1}^{100} n$` te dará como resultado:

$$\sum_{n = 1}^{100} n$$

En la siguiente tabla hay una lista de símbolos que se comportan de la misma manera.

| Expresión | Salida |
| ---- | ---- |
| `\int ` | $\int$ |
| `\iint ` | $\iint$ |
| `\iint ` | $\iiint$ |
| `\prod ` | $\prod$ |
| `\bigcup` | $\bigcup$ |
| `\bigcap` | $\bigcap$ |

## Conclusión

Ya tienes una guía para empezar a escribir ecuaciones tan complejas como quieras, y así complementar tus documentos de Markdown o tus cuadernos de Jupyter.

Por supuesto, hay muchos más símbolos y ajustes que puedes hacer que no cubrí en este post. Te invito a consultar este [artículo de referencia](https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference/5058#5058) en el que se entra más en detalle sobre MathJax.

Si te pareció útil, déjame un comentario abajo o comparte este post en tus redes. ¡Éxitos en tu aprendizaje!