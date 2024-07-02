---
title: Creando gráficas con calidad para publicación con ggplot2
teaching: 60
exercises: 20
source: Rmd
---

::::::::::::::::::::::::::::::::::::::: objectives

- Ser capaz de utilizar ggplot2 para generar gráficos con calidad para publicación.
- Entender la gramática básica de los gráficos, incluyendo estética y capas geométricas, agregando estadísticas, transformando las escalas y los colores, o dividiendo por grupos.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- ¿Cómo puedo crear gráficos con calidad para publicación en R?

::::::::::::::::::::::::::::::::::::::::::::::::::



Graficar nuestros datos es una de las mejores maneras
para explorarlos y para observar las relaciones que existen entre las variables.

En R existen tres sistemas principales encargados de hacer gráficos,
el sistema de ploteo [base], el paquete [lattice]
y el paquete [ggplot2].

Hoy aprenderemos acerca de **`ggplot2`**, que permite de manera sencilla crear gráficos
complejos a partir de un conjunto de datos. Este paquete provee una estructura para que podamos especificar
qué variables graficar, cómo deben ser presentadas y otras propiedades visuales generales
([consulta la guía rápida de funciones](https://raw.githubusercontent.com/rstudio/cheatsheets/master/translations/spanish/data-visualization_es.pdf)).

De esta manera, sólo necesitamos realizar cambios mínimos si los datos sufren alguna modificación
o si decidimos pasar, por ejemplo, de un gráfico de barras a un diagrama de dispersión.
Esto ayuda a crear gráficos con calidad para publicación con pocos ajustes adicionales.

**`ggplot2`** se basa en la *gramática de gráficos*, una idea que plantea que
cualquier gráfico puede expresarse a partir de la combinación de estos componentes:
un conjunto de **datos**, un **sistema de coordenadas** y un conjunto de **geoms** (que determinan la representación visual de los datos).

La clave para entender **`ggplot2`** es pensar en una figura como un conjunto de capas.
Esta idea podría resultarte familiar si has usado un programa de edición de imágenes como
Photoshop, Illustrator o Inkscape. Esto quiere decir que los gráficos de **`ggplot2`**
se construyen paso a paso agregando nuevos elementos o capas,
resultando en una gran flexibilidad para la personalización de los mismos.

Para construir un ggplot, emplearemos esta plantilla básica que puede usarse para distintos tipos de
gráficos:

```
ggplot(data = <DATOS>, mapping = aes(<MAPEOS>)) +  <GEOM_FUNCIÓN>()
```

- Usa la función `ggplot()` para vincular el gráfico a un conjunto de datos específico a través
  del argumento `data`. A las funciones de **`ggplot2`** les gusta trabajar con datos en *formato largo*, es decir, una columna para cada variable y una fila para cada observación. Tener datos bien estructurados te ahorrará mucho tiempo a la hora de realizar gráficos con **`ggplot2`**. Además, todos los argumentos
  que le pasemos a la función `ggplot()` serán considerados como opciones *globales* en nuestro gráfico,
  lo cual significa que estas opciones son válidas para todas sus capas:




``` r
library("ggplot2")
ggplot(data = gapminder)
```

- Define un mapeo (usando la función `aes()` dentro de `ggplot()`) para seleccionar las variables a graficar y especificar cómo deben ser presentadas, por ejemplo, en los ejes x/y o como característicales tales como tamaño, forma, color, etc. `aes()` le dice a `ggplot` cómo se relaciona cada una de las variables en los **datos** con las propiedades
  **aesthetic** (estéticas) de la figura. En este caso, le decimos a `ggplot` que queremos graficar la columna "gdpPercap" del **data frame** gapminder en el eje X, y la columna
  "lifeExp" en el eje Y. Nota que no necesitamos indicar explícitamente estas columnas en la función `aes`
  (e.g. `x = gapminder[, "gdpPercap"]`), ¡esto es debido a que `ggplot` es suficientemente listo para
  buscar esa columna en los **datos**!:


``` r
ggplot(data = gapminder, mapping = aes(x = gdpPercap, y = lifeExp))
```

- Agrega *geoms*, que indican cómo se representan de los datos en el gráfico (puntos, líneas, barras). **`ggplot2`** ofrece muchos geoms diferentes, algunos comúnmente utilizados son:
  
  ```
  * `geom_point()` para diagramas de dispersión, gráficos de puntos, etc.
  * `geom_boxplot()` para diagramas de caja y bigotes.
  * `geom_line()` para líneas de tendencias, series de tiempo, etc.  
  ```
  
  Para agregar un geom, usa el operador `+`. ¡Podemos agregar varios geoms unidos a través de múltiples operadores `+`! Observa que en la figura anterior, llamar a la función **`ggplot`** no fue suficiente para obtener un gráfico, debemos agregar un geom. Podemos usar **`geom_point`** para representar visualmente la relación entre **x** y **y** como un gráfico de dispersión de puntos (**scatterplot**).


``` r
ggplot(data = gapminder, mapping = aes(x = gdpPercap, y = lifeExp)) +
  geom_point()
```

<img src="fig/08-plot-ggplot2-rendered-build-ggplot-3-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 1

Modifica el ejemplo anterior de manera que en la figura se muestre
como la esperanza de vida ha cambiado a través del tiempo:


``` r
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp)) + geom_point()
```

Pista: El **dataset** gapminder tiene una columna llamada "year", la cual
debe aparecer en el eje X.

:::::::::::::::  solution

## Solución al desafío 1

Modifica el ejemplo de manera que en la figura se muestre
como la esperanza de vida ha cambiado a través del tiempo:

Esta es una posible solución:


``` r
ggplot(data = gapminder, aes(x = year, y = lifeExp)) + geom_point()
```

<img src="fig/08-plot-ggplot2-rendered-ch1-sol-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 2

En los ejemplos y desafíos anteriores hemos usado la función `aes` para decirle al `geom_point`
cuáles serán las posiciones **x** y **y** para cada punto. Otra propiedad **aesthetic** que podemos modificar
es el *color*. Modifica el código del desafío anterior para **colorear** los puntos de acuerdo a la columna
"continent". ¿Qué tendencias observas en los datos? ¿Son lo que esperabas?

:::::::::::::::  solution

## Solución al desafío 2

En los ejemplos y desafios anteriores hemos usado la función `aes` para decirle al `geom_point`
cuáles serán las posiciones **x** y **y** para cada punto. Otra propiedad **aesthetic** que podemos modificar
es el *color*. Modifica el código del desafío anterior para **colorear** los puntos de acuerdo a la columna
"continent". ¿Qué tendencias observas en los datos? ¿Son lo que esperabas?


``` r
ggplot(data = gapminder, aes(x = year, y = lifeExp, color=continent)) +
  geom_point()
```

<img src="fig/08-plot-ggplot2-rendered-ch2-sol-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Capas

Un gráfico de dispersión probablemente no es la mejor manera de visualizar el cambio a través del tiempo.
En vez de eso, vamos a decirle a `ggplot` que queremos visualizar los datos como un diagrama de línea (line plot):


``` r
ggplot(data = gapminder, aes(x=year, y=lifeExp, by=country, color=continent)) +
  geom_line()
```

<img src="fig/08-plot-ggplot2-rendered-lifeExp-line-1.png" style="display: block; margin: auto;" />

En vez de agregar una capa `geom_point`, hemos agregado una capa `geom_line`.
Además, hemos agregado el argumento **aesthetic** **by**, el cual le dice a `ggplot` que
debe dibujar una línea para cada país.

Pero, ¿qué pasa si queremos visualizar ambos, puntos y líneas en la misma gráfica?
Simplemente tenemos que agregar otra capa al gráfico:


``` r
ggplot(data = gapminder, aes(x=year, y=lifeExp, by=country, color=continent)) +
  geom_line() + geom_point()
```

<img src="fig/08-plot-ggplot2-rendered-lifeExp-line-point-1.png" style="display: block; margin: auto;" />

Es importante notar que cada capa se dibuja encima de la capa anterior. En este ejemplo,
los puntos se han dibujado *sobre* las líneas. A continuación observamos una demostración:


``` r
ggplot(data = gapminder, aes(x=year, y=lifeExp, by=country)) +
  geom_line(aes(color=continent)) + geom_point()
```

<img src="fig/08-plot-ggplot2-rendered-lifeExp-layer-example-1-1.png" style="display: block; margin: auto;" />

En este ejemplo, el mapeo **aesthetic** de **color** se ha movido de las opciones globales de la gráfica en
`ggplot` a la capa `geom_line` y, por lo tanto, ya no es válido para los puntos.
Ahora podemos ver claramente que los puntos se dibujan sobre las líneas.

:::::::::::::::::::::::::::::::::::::::::  callout

## Sugerencia: Asignando un **aesthetic** a un valor en vez de a un mapeo

Hasta ahora, hemos visto cómo usar un **aesthetic** (como **color**) como un *mapeo* entre una variable de los datos
y su representación visual. Por ejemplo, cuando usamos `geom_line(aes(color=continent))`, ggplot le asignará
un color diferente a cada continente.
Pero, ¿qué tal si queremos cambiar el color de todas las líneas a azul? Podrías pensar que
`geom_line(aes(color="blue"))` debería funcionar, pero no es así. Dado que no queremos crear un mapeo hacia una variable
específica, simplemente debemos cambiar la especificación de color afuera de la función `aes()`, de esta manera:
`geom_line(color="blue")`.


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 3

Intercambia el orden de las capas de los puntos y líneas del ejemplo anterior. ¿Qué sucede?

:::::::::::::::  solution

## Solución a desafío 3

Intercambia el orden de las capas de los puntos y líneas del ejemplo anterior. ¿Qué sucede?


``` r
ggplot(data = gapminder, aes(x=year, y=lifeExp, by=country)) +
 geom_point() + geom_line(aes(color=continent))
```

<img src="fig/08-plot-ggplot2-rendered-ch3-sol-1.png" style="display: block; margin: auto;" />

¡Las líneas ahora están dibujadas sobre los puntos!

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Transformaciones y estadísticas

`ggplot` también facilita sobreponer modelos estadísticos a los datos.
Para demostrarlo regresaremos a nuestro primer ejemplo:


``` r
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp, color=continent)) +
  geom_point()
```

<img src="fig/08-plot-ggplot2-rendered-lifeExp-vs-gdpPercap-scatter3-1.png" style="display: block; margin: auto;" />

En este momento es difícil ver las relaciones entre los puntos debido a algunos
valores altamente atípicos de la variable GDP per capita. Podemos cambiar la escala de unidades del eje X
usando las funciones de **escala**. Estas funciones controlan la relación entre los valores
de los datos y los valores visuales de un **aesthetic**. También podemos modificar la transparencia
de los puntos, usando la función *alpha*, la cual es especialmente útil cuando tienes una
gran cantidad de datos fuertemente conglomerados.


``` r
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp)) +
  geom_point(alpha = 0.5) + scale_x_log10()
```

<img src="fig/08-plot-ggplot2-rendered-axis-scale-1.png" style="display: block; margin: auto;" />

La función `log10` aplica una transformación sobre los valores de la columna "gdpPercap"
antes de presentarlos en el gráfico, de manera que cada múltiplo de 10 ahora
corresponde a un incremento de 1 en la escala transformada, e.g. un GDP per capita de 1,000
se convierte en un 3 en el eje Y, un valor de 10,000 corresponde a un valor de 4 en el eje Y, y así sucesivamente.
Esto facilita visualizar la dispersión de los datos sobre el eje X.

:::::::::::::::::::::::::::::::::::::::::  callout

## Sugerencia Recordatorio: Asignando un **aesthetic** a un valor en vez de a un mapeo

Nota que usamos `geom_point(alpha = 0.5)`. Como menciona la sugerencia anterior,
cambiar una especificación afuera de la función `aes()` causará que este valor sea usado
para todos los puntos, que es exactamente lo que queremos en este caso.  Sin embargo, como cualquier otra
especificación **aesthetic**, *alpha* puede ser mapeado hacia una variable de los datos. Por ejemplo,
podemos asignar una transparencia diferente a cada continente usando `geom_point(aes(alpha = continent))`.


::::::::::::::::::::::::::::::::::::::::::::::::::

Podemos ajustar una relación simple a los datos agregando otra capa,
`geom_smooth`:


``` r
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp)) +
  geom_point() + scale_x_log10() + geom_smooth(method="lm")
```

``` output
`geom_smooth()` using formula = 'y ~ x'
```

<img src="fig/08-plot-ggplot2-rendered-lm-fit-1.png" style="display: block; margin: auto;" />

Podemos hacer la línea más gruesa *configurando* el argumento **aesthetic** **tamaño** en la capa `geom_smooth`:


``` r
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp)) +
  geom_point() + scale_x_log10() + geom_smooth(method="lm", size=1.5)
```

``` warning
Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
ℹ Please use `linewidth` instead.
This warning is displayed once every 8 hours.
Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
generated.
```

``` output
`geom_smooth()` using formula = 'y ~ x'
```

<img src="fig/08-plot-ggplot2-rendered-lm-fit2-1.png" style="display: block; margin: auto;" />

Existen dos formas en las que un *aesthetic* puede ser especificado. Aquí *configuramos* el
**aesthetic** **tamaño** pasándolo como un argumento a `geom_smooth`. Previamente en la lección
habíamos usado la función `aes` para definir un *mapeo* entre alguna variable de los datos y su representación visual.

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 4a

Modifica el color y el tamaño de los puntos en la capa puntos en el ejemplo anterior

Pista: No uses la función `aes`.

:::::::::::::::  solution

## Solución al desafío 4a

Modifica el color y el tamaño de los puntos en la capa puntos en el ejemplo anterior

Pista: No uses la función `aes`.


``` r
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp)) +
 geom_point(size=3, color="orange") + scale_x_log10() +
 geom_smooth(method="lm", size=1.5)
```

``` output
`geom_smooth()` using formula = 'y ~ x'
```

<img src="fig/08-plot-ggplot2-rendered-ch4a-sol-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 4b

Modifica tu solución al Desafío 4a de manera que ahora los puntos
tengan una forma diferente y estén coloreados de acuerdo al continente
incluyendo líneas de tendencia.

Pista: El argumento color puede ser usado dentro de **aesthetic**

:::::::::::::::  solution

## Solución al desafío 4b

Modifica tu solución al Desafío 4a de manera que ahora los puntos
tengan una forma diferente y estén coloreados de acuerdo al continente
incluyendo líneas de tendencia.

Pista: El argumento color puede ser usado dentro de **aesthetic**


``` r
ggplot(data = gapminder, aes(x = gdpPercap, y = lifeExp, color = continent)) +
geom_point(size=3, shape=17) + scale_x_log10() +
geom_smooth(method="lm", size=1.5)
```

``` output
`geom_smooth()` using formula = 'y ~ x'
```

<img src="fig/08-plot-ggplot2-rendered-ch4b-sol-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Figuras en múltiples paneles

Anteriormente visualizamos el cambio en la esperanza de vida a lo largo del tiempo para cada uno de los países
en un solo gráfico. Como una alternativa, podemos dividir este gráfico en múltiples paneles al agregar una capa **facet**,
enfocándonos únicamente en aquellos países con nombres que empiezan con la letra "A" o "Z".

:::::::::::::::::::::::::::::::::::::::::  callout

## Pista

Empezamos por subdividir los datos. Usamos la función `substr` para extraer una parte de una secuencia de caracteres;
extrayendo las letras que ocurran de la posición `start` hasta `stop`, inclusive, del vector `gapminder$country`.
El operador `%in%` nos permite hacer múltiples comparaciones y evita que tengamos que escribir
una condición muy larga para subdividir los datos (en este caso, `starts.with %in% c("A", "Z")`
es equivalente a `starts.with == "A" | starts.with == "Z"`)


::::::::::::::::::::::::::::::::::::::::::::::::::


``` r
starts.with <- substr(gapminder$country, start = 1, stop = 1)
az.countries <- gapminder[starts.with %in% c("A", "Z"), ]
ggplot(data = az.countries, aes(x = year, y = lifeExp, color=continent)) +
  geom_line() + facet_wrap( ~ country)
```

<img src="fig/08-plot-ggplot2-rendered-facet-1.png" style="display: block; margin: auto;" />

La capa `facet_wrap` toma una "fórmula" como argumento, lo cual se indica por el símbolo `~`.
Esto le dice a R que debe dibujar un panel para cada valor único de la columna "country"
del **dataset** gapminder.

## Modificando texto

Para limpiar esta figura y que quede lista para publicar necesitamos cambiar algunos elementos de texto.
El eje X está demasiado saturado, y el nombre del eje Y debería ser "Esperanza de vida",
en vez del nombre que aparece para esa columna en el **data frame**.

Podemos hacer todo lo anterior agregando un par de capas. La capa **theme** controla el texto de los ejes,
y el tamaño del texto. Las etiquetas de los ejes, el título del gráfico y el título para todas las
leyendas pueden ser configurados utilizando la función `labs`. Los títulos de las leyendas son configurados
haciendo referencia a los mismos nombres que utilizamos en la especificación `aes`. Entonces, en el siguiente ejemplo
el título de la leyenda de los colores de las líneas se define utilizando `color = "Continent"`, mientras que el título
de la leyenda del color del relleno se definiría utilizando `fill = "MyTitle"`.


``` r
ggplot(data = az.countries, aes(x = year, y = lifeExp, color=continent)) +
  geom_line() + facet_wrap( ~ country) +
    labs(
        x = "Year",              # título del eje x
        y = "Life expectancy",   # título del eje y
        title = "Figure 1",   # título principal de la figura
        color = "Continent"   # título de la leyenda
    ) +
  theme(axis.text.x=element_blank(), axis.ticks.x=element_blank())
```

<img src="fig/08-plot-ggplot2-rendered-theme-1.png" style="display: block; margin: auto;" />

## Exportando una gráfica

La función `ggsave()` permite exportar una gráfica creada con ggplot. Puedes especificar la dimensión y la resolución de
tu gráfica  ajustando los argumentos apropiados (`width`, `height` y `dpi`) para crear gráficas de gran calidad para publicar.
Para poder guardar la gráfica de arriba, primero tenemos que asignarla a una variable `lifeExp_plot`, y luego decirle a
`ggsave` que guarde la gráfica en formato `png` a un directorio llamado `results`. (Asegúrate de que tienes una carpeta
llamada `results/` en tu directorio de trabajo.)


``` r
lifeExp_plot <- ggplot(data = az.countries, aes(x = year, y = lifeExp, color=continent)) +
  geom_line() + facet_wrap( ~ country) +
  labs(
        x = "Year",              # título del eje x
        y = "Life expectancy",   # título del eje y
        title = "Figure 1",   # título principal de la figura
        color = "Continent"   # título de la leyenda
  ) +
  theme(axis.text.x=element_blank(), axis.ticks.x=element_blank())
ggsave(filename = "results/lifeExp.png", plot = lifeExp_plot, width = 12, height = 10, dpi = 300, units = "cm")
```

``` error
Error in `ggsave()`:
! Cannot find directory 'results'.
ℹ Please supply an existing directory or use `create.dir = TRUE`.
```

Hay dos cosas agradables acerca de `ggsave`. Primero, usa la última gráfica por default, así que si omites el argumento `plot`,
`ggsave` automáticamente guardará la última gráfica que creaste con `ggplot`.
En segundo lugar, `ggsave` trata de determinar el formato en que quieres guardar la gráfica a partir de la extensión provista
en el nombre del archivo (por ejemplo `.png` or `.pdf`). Si es necesario, puedes especificar el formato explícitamente
en el argumento `device`.

Esta lección es una prueba de lo que puedes hacer utilizando `ggplot2`. RStudio proporciona
una [hoja de ayuda][cheat] realmente útil de las diferentes capas disponibles,
una documentación más extensa se encuentra disponible en el [sitio web de ggplot2][ggplot-doc].
Finalmente, si no tienen idea de cómo cambiar algún detalle, una búsqueda rápida en Google
te llevarán a Stack Overflow donde encuentras preguntas y respuestas ¡con código reusable que puedes modificar!

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 5

Crea una gráfica de densidad de GDP per cápita, en la que el color del relleno cambie por continente.

Avanzado:

- Transforma el eje X para visualizar mejor la dispersión de los datos.
- Agrega una capa **facet** para generar gráficos de densidad con un panel para cada año.

:::::::::::::::  solution

## Solución al desafío 5

Crea una gráfica de densidad de GDP per cápita, en la que el color del relleno cambie por continente.

Avanzado:

- Transforma el eje X para visulaizar mejor la dispersión de los datos.
- Agrega una capa **facet** para generar gráficos de densidad con un panel para cada año.


``` r
ggplot(data = gapminder, aes(x = gdpPercap, fill=continent)) +
 geom_density(alpha=0.6) + facet_wrap( ~ year) + scale_x_log10()
```

<img src="fig/08-plot-ggplot2-rendered-ch5-sol-1.png" style="display: block; margin: auto;" />

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



[base]: https://www.statmethods.net/graphs/
[lattice]: https://www.statmethods.net/advgraphs/trellis.html
[ggplot2]: https://www.statmethods.net/advgraphs/ggplot2.html
[cheat]: https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf
[ggplot-doc]: https://docs.ggplot2.org/current/


:::::::::::::::::::::::::::::::::::::::: keypoints

- Usar `ggplot2` para crear gráficos.
- Piensa cada gráfico como capas: estética, geometría, estadisticas, transformaciones de escala, y agrupamiento.

::::::::::::::::::::::::::::::::::::::::::::::::::


