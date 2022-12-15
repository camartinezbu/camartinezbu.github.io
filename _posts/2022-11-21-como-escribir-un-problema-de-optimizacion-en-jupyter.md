---
title: ¿Cómo escribir un problema de optimización en Jupyter?
date: 2022-11-21 15:15:00 -0500
categories: Jupyter
tags: [jupyter, ecuaciones, mathjax, latex, markdown, tutorial]
image: 
  path: https://raw.githubusercontent.com/camartinezbu/blog-images/main/posts/2022-11-21-como-escribir-un-problema-de-optimizacion-en-jupyter/hero.png
excerpt: Aprende sobre el funcionamiento de MathJax.
math: true
---

Hace varias semanas compartí una entrada en el blog en la que hablaba sobre cómo [escribir ecuaciones en Jupyter con LaTeX](https://www.camartinezbu.com/posts/la-guia-definitiva-para-escribir-ecuaciones-en-jupyter/), en la que expliqué cómo escribir fracciones, superíndices, sumas, integrales y otras expresiones comunes. En esta ocasión, te quiero explicar algunas funciones avanzadas de MathJax usando el problema clásico de maximización de utilidad de economía.

> Si tienes dudas sobre el ejemplo que voy a expicar a continuación, te invito a revisar [el post en el que explico elementos básicos de MathJax](https://www.camartinezbu.com/posts/la-guia-definitiva-para-escribir-ecuaciones-en-jupyter/), la implementación de LaTeX que funciona en Jupyter.
{: .prompt-tip }

## Maximización de la Utilidad

En los cursos de economía, econometría o machine learning es normal encontrarnos con el planteamiento de problemas de optimización, en los que se pretende maximizar o minimizar una función objetivo –por ejemplo una función de utilidad o una función de costos– usando algún argumento de esa función y sujeto a alguna condición.

En este ejemplo vas a aprender a escribir la maximización de una función de utilidad Cobb-Douglas sujeta a una restricción presupuestaria, muy común en los cursos introductorios de microeconomía:

$$\begin{align}&\underset{x_1,x_2}{\operatorname{max}}\;U(x_1,x_2)=x_1^{\alpha}x_2^{1-\alpha}\\&\mathrm{s.a.}\;p_1x_1+p_2x_2=w\end{align}$$

Antes de seguir, quiero que notes varias cosas de esta expresión:
- La palabra $\operatorname{max}$ y las letras $\mathrm{s.a.}$ tienen un estilo sin cursiva, a diferencia del resto de letras de la ecuación.
- Los argumentos $x_1,x_2$ aparecen justo debajo de $\operatorname{max}$ y con un tamaño de fuente un poco más pequeño.
- La restricción presupuestaria ($p_1x_1+p_2x_2=w$) está alineada al operador $\operatorname{max}$ y no a la función de utlidad $U$.

Veamos cómo implementar cada uno de estos comportamientos.

## Estilos de fuente

Por defecto, los caracteres que aparecen en las ecuaciones de MathJax aparecen en cursiva. Sin embargo, no en todos los casos vas a querer trabajar de esta manera, como en el caso del ejemplo del problema de maximización de utilidad.

En estos casos, es necesario usar la familia de funciones `\math_` incluidas en MathJax, que se resume en la siguiente tabla:

| Estilo | Expresión | Salida |
| ---- | ---- | ---- |
| Cursiva | `ABCDEFabcdef` o `\mathit{ABCDEFabcdef}` | $ABCDEFabcdef$ |
| Negrilla | `\mathbf{ABCDEFabcdef}` | $\mathbf{ABCDEFabcdef}$ |
| Redonda con serifas | `\mathrm{ABCDEFabcdef}` | $\mathrm{ABCDEFabcdef}$ |
| Redonda sin serifas | `\mathsf{ABCDEFabcdef}` | $\mathsf{ABCDEFabcdef}$ |
| Máquina de escribir | `\mathtt{ABCDEFabcdef}` | $\mathtt{ABCDEFabcdef}$ |
| Caligrafía | `\mathcal{ABCDEFabcdef}` | $\mathcal{ABCDEFabcdef}$ |
| Script | `\mathscr{ABCDEFabcdef}` | $\mathscr{ABCDEFabcdef}$ |
| Fraktur | `\mathfrak{ABCDEFabcdef}` | $\mathfrak{ABCDEFabcdef}$ |
| Negrilla de tablero | `\mathbb{ABCDEFabcdef}` | $\mathbb{ABCDEFabcdef}$ |

> Para caracteres que se esceriben con una expresión –como `\alpha` – hay que usar la función `\boldsymbol{}` para ponerlas en negrilla. Por ejemplo, `\boldsymbol{\alpha}` devuelve $\boldsymbol{\alpha}$.
{: .prompt-tip }

Ahora bien, no siempre usar `\mathrm{}` es la mejor opción para quitar la fuente cursiva a las ecuaciones. En particular, en lo que se refiere a operadores o nombres de funciones, es una buena idea usar la expresión `\operatorname{}`, dado que incluye espacios antes y después del operador entre [otros comportamientos específicos](https://tex.stackexchange.com/questions/48459/whats-the-difference-between-mathrm-and-operatorname). Mientras que `a\mathrm{operador}b` devuelve $a\mathrm{operador}b$, `a\operatorname{operador}b` devuelve $a\operatorname{operador}b$.

## Underset y Overset

MathJax incluye varios operadores para lidiar con elementos ubicados encima o debajo de la línea principal, tal como en el ejemplo de maximización de la utilidad.

Similar a las fracciones, para usar estos operadores tenemos que usar la palabra clave `\underset` (u `\overset`) seguido de dos pares de corchetes. El primero denota el elemento que va debajo (o encima) y el segundo el elemento principal. Es decir:

| Expresión | Salida |
| ---- | ---- |
| `\underset{b}{A}` | $\underset{b}{A}$ |
| `\overset{b}{A}` | $\overset{b}{A}$ |

## Alineación

Para la alineación horizontal de expresiones, tal como lo que sucede con la restricción presupuestaria, es necesario usar las expresiones `\begin{align}...\end{align}`, en conjunto con los caracteres `&` y `\\`. Veamos cómo funciona.

Para empezar, sólo lo que va dentro de `\begin{align}` y `\end{align}` va a seguir el comportamiento de alineación horizontal. Esto es útil porque como te vas a dar cuenta las expresiones en MathJax se pueden volver complejas con facilidad, por lo que es útil limitar los comportamientos especiales únicamente al lugar en el que los necesitamos.

El caracter `&` va a indicar los lugares de la expresión a donde se van a anclar cada una de las líneas para alinearse. Por su parte, `\\` va a marcar el final de cada línea (algo parecido a oprimir Enter en tu teclado en Word).

Sopongamos que tenemos la siguiente expresión, escrita en MathJax como `w + x + y + z`:

$$w + x + y + z$$

Al encerrarla así `\begin{align} w + x + y + z \end{align}`, no vas a ver ningún comportamiento especial, porque no le hemos indicado hasta dónde van las lineas y dónde tienen que alinease.

$$\begin{align} w + x + y + z \end{align}$$

Sin embargo, si incluimos `\\`, podemos separar las líneas de distintas formas:

- `\begin{align} w \\+ x + y + z \end{align}` devuelve:

$$\begin{align} w \\+ x + y + z \end{align}$$

- `\begin{align} w + x\\ + y + z \end{align}` devuelve:

$$\begin{align} w + x \\ + y + z \end{align}$$

- `\begin{align} w + x + y +\\ z \end{align}` devuelve

$$\begin{align} w + x + y + \\z \end{align}$$

Nota que por defecto las líneas se acomodan de tal forma que las líneas quedan con una justificación a la derecha. Puedes cambiar este comportamiento con el caracter `&`. 

Tomemos el último ejemplo. Ouedes acomodar el lugar en el que aparece $+z$ de la siguiente manera:

- `\begin{align} &w + x + y +\\ &z \end{align}` devuelve

$$\begin{align} &w + x + y + \\&z \end{align}$$

- `\begin{align} w + &x + y +\\ &z \end{align}` devuelve

$$\begin{align} w + &x + y + \\&z \end{align}$$

- `\begin{align} w + x + &y +\\ &z \end{align}` devuelve

$$\begin{align} w + x + &y + \\&z \end{align}$$

## Juntemos todo

Pongamos en práctica todo lo anterior. ¿Recuerdas los tres puntos que resalté al inicio del post? Vamos a implementarlos uno por uno. Comencemos por una expresión que no tenga el formato deseado. Algo como 

`max\;x_1,x_2\;U(x_1,x_2)=x_1^{\alpha}x_2^{1-\alpha}\;s.a.\;p_1x_1+p_2x_2=w`

$$max\;x_1,x_2\;U(x_1,x_2)=x_1^{\alpha}x_2^{1-\alpha}\;s.a.\;p_1x_1+p_2x_2=w$$

> Puedes ver que usé `\;` varias veces en la expresión. Esta combinación de caracteres inserta un espacio en blanco entre las letras, que incluí para facilitar la legibilidad. Intenta escribirlo tú mismo sin estos caracteres para ver qué pasa.
{: .prompt-info }

Lo primero es quitarle el formato en cursiva a $max$ y $s.a.$. Para el primero voy a usar la función `\operatorname` y para el segundo `\mathrm` así: 

`\operatorname{max}\;x_1,x_2\;U(x_1,x_2)=x_1^{\alpha}x_2^{1-\alpha}\;\mathrm{s.a.}\;p_1x_1+p_2x_2=w`.

$$\operatorname{max}\;x_1,x_2\;U(x_1,x_2)=x_1^{\alpha}x_2^{1-\alpha}\;\mathrm{s.a.}\;p_1x_1+p_2x_2=w$$

Lo siguiente es hacer que $x_1,x_2$ aparezcan debajo de $\operatorname{max}$. Para ello, voy a usar `\underset` de la siguiente manera: 

`\underset{x_1,x_2}{\operatorname{max}}\;U(x_1,x_2)=x_1^{\alpha}x_2^{1-\alpha}\;\mathrm{s.a.}\;p_1x_1+p_2x_2=w`

$$\underset{x_1,x_2}{\operatorname{max}}\;U(x_1,x_2)=x_1^{\alpha}x_2^{1-\alpha}\;\mathrm{s.a.}\;p_1x_1+p_2x_2=w$$

Ahora vamos a dejar la función objetivo y la restricción en líneas distintas, con `\begin{align}...\end{align}` y `\\` así:

`\begin{align}\underset{x_1,x_2}{\operatorname{max}}\;U(x_1,x_2)=x_1^{\alpha}x_2^{1-\alpha}\;\\\mathrm{s.a.}\;p_1x_1+p_2x_2=w\end{align}`

$$\begin{align}\underset{x_1,x_2}{\operatorname{max}}\;U(x_1,x_2)=x_1^{\alpha}x_2^{1-\alpha}\;\\\mathrm{s.a.}\;p_1x_1+p_2x_2=w\end{align}$$

Finalmente, vamos a alinear la restricción al operador $\operatorname{max}$ con el caracter `&`:

`$$\begin{align}&\underset{x_1,x_2}{\operatorname{max}}\;U(x_1,x_2)=x_1^{\alpha}x_2^{1-\alpha}\;\\&\mathrm{s.a.}\;p_1x_1+p_2x_2=w\end{align}$$`

$$\begin{align}&\underset{x_1,x_2}{\operatorname{max}}\;U(x_1,x_2)=x_1^{\alpha}x_2^{1-\alpha}\;\\&\mathrm{s.a.}\;p_1x_1+p_2x_2=w\end{align}$$

## Conclusión

Aprendiste a cambiar el estilo de fuente, a poner elementos sonre otros y a alinear las líneas de tus expresiones con MathJax. Te invito a que experimentes con nuevas ecuaciones y veas qué tipo de comportamientos complejos logras escribir.

Si te pareció útil, déjame un comentario abajo o comparte este post en tus redes. ¡Éxitos en tu aprendizaje!