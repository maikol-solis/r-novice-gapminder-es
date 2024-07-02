---
title: Estructuras de datos
teaching: 40
exercises: 15
source: Rmd
---

::::::::::::::::::::::::::::::::::::::: objectives

- Conocer los distintos tipos de datos.
- Comenzar a explorar los *data frames* y entender cómo se relacionan con **vectors**, **factors** y **lists**.
- Ser capaz de preguntar sobre el tipo, clase y estructura de un objeto en R.
- Conocer y entender qué es coerción y cuáles son los distintos tipos de coerciones.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- ¿Cómo puedo leer datos en R?
- ¿Cuáles son los tipos de datos básicos en R?
- ¿Cómo represento la información categórica en R?

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::  checklist

## Palabras clave

Comando : Significado

*data set* : conjunto de datos

`c` : combinar

`dim` : dimensión

`nrow` : número de filas

`ncol`: número de columnas


::::::::::::::::::::::::::::::::::::::::::::::::::

Una de las características más poderosas de R es su habilidad para manejar datos tabulares -
como los que puedes tener en una planilla de cálculo o un archivo CSV.
Comencemos creando un **dataset** llamado `gatos` que se vea de la siguiente forma:


``` r
color,peso,le_gusta_cuerda
mixto,2.1,1
negro,5.0,0
atigrado,3.2,1
```

Podemos usar la función `data.frame` para crearlo.


``` r
gatos <- data.frame(color = c("mixto", "negro", "atigrado"),
                      peso = c(2.1, 5.0, 3.2),
                      le_gusta_cuerda = c(1, 0, 1))                    
gatos                   
```

``` output
     color peso le_gusta_cuerda
1    mixto  2.1               1
2    negro  5.0               0
3 atigrado  3.2               1
```



:::::::::::::::::::::::::::::::::::::::::  callout

## Consejo: Edición de archivos de texto en R

Crea el archivo `data/gatos-data.csv` en RStudio usando el ítem del Menú  **Files -> New folder**.
Ahid debes crear el directorio data para poder guardar dentro el archivo gatos-data.csv.
Ahora sí usa `write.csv(gatos, "data/gatos-data.csv", row.names=FALSE)` para crear el archivo


::::::::::::::::::::::::::::::::::::::::::::::::::

Podemos leer el archivo en R con el siguiente comando:


``` r
gatos <- read.csv(file = "data/gatos-data.csv", stringsAsFactors = TRUE)
gatos
```

``` output
     color peso le_gusta_cuerda
1    mixto  2.1               1
2    negro  5.0               0
3 atigrado  3.2               1
```

La función `read.table` se usa para leer datos tabulares almacenados en un archivo de
texto donde las columnas de datos están separadas por caracteres de puntuación, por ejemplo
archivos CSV (csv = valores separados por comas). Las tabulaciones y las comas son los
caracteres de puntuación más utilizados para separar o delimitar datos en archivos csv.
Para mayor comodidad, R proporciona otras 2 versiones de "read.table". Estos son: `read.csv`
para archivos donde los datos están separados por comas y `read.delim` para archivos
donde los datos están separados por tabulaciones. De estas tres funciones, `read.csv` es
el más utilizado. Si fuera necesario, es posible anular o modificar el delimitador predeterminado
para `read.csv` y ` read.delim`.

Podemos empezar a explorar el **dataset** inmediatamente proyectando las columnas usando el operador `$`:


``` r
gatos$peso
```

``` output
[1] 2.1 5.0 3.2
```

``` r
gatos$color
```

``` output
[1] mixto    negro    atigrado
Levels: atigrado mixto negro
```

Podemos hacer otras operaciones sobre las columnas. Por ejemplo, podemos aumentar el peso de todos los gatos con:


``` r
gatos$peso + 2
```

``` output
[1] 4.1 7.0 5.2
```

Podemos imprimir los resultados en una oración


``` r
paste("El color del gato es", gatos$color)
```

``` output
[1] "El color del gato es mixto"    "El color del gato es negro"   
[3] "El color del gato es atigrado"
```

Pero qué pasa con:


``` r
gatos$peso + gatos$color
```

``` warning
Warning in Ops.factor(gatos$peso, gatos$color): '+' not meaningful for factors
```

``` output
[1] NA NA NA
```

Si adivinaste que el último comando iba a resultar en un error porque `2.1` más
`"negro"` no tiene sentido, estás en lo cierto - y ya tienes alguna intuición sobre un concepto
importante en programación que se llama *tipos de datos*.

No importa cuán complicado sea nuestro análisis, todos los datos en R se interpretan con uno de estos
tipos de datos básicos. Este rigor tiene algunas consecuencias importantes.

Hay 5 tipos de datos principales: `double`, `integer`, `complex`, `logical` and `character`.

Podemos preguntar cuál es la estructura de datos si usamos la función `class`:


``` r
class(gatos$color)
```

``` output
[1] "factor"
```

``` r
class(gatos$peso)
```

``` output
[1] "numeric"
```

También podemos ver que `gatos` es un **data.frame** si usamos la función `class`:


``` r
class(gatos)
```

``` output
[1] "data.frame"
```

## Vectores y Coerción de Tipos

Para entender mejor este comportamiento, veamos otra de las estructuras de datos en R: el **vector**.

Un vector en R es esencialmente una lista ordenada de cosas, con la condición especial
de que *todos los elementos en un vector tienen que ser del mismo tipo de datos básico*.
Si no eliges un tipo de datos, por defecto R elige el tipo de datos **logical**.
También puedes declarar un vector vacío de cualquier tipo que quieras.

Una indicación del número de elementos en el vector - específicamente los índices
del vector, en este caso `[1:3]` y unos pocos ejemplos
de los elementos del vector - en este caso **strings** vacíos.

Podemos ver que `gatos$peso` es un vector usando la funcion `str`.


``` r
str(gatos$peso)
```

``` output
 num [1:3] 2.1 5 3.2
```

Las columnas de datos que cargamos en **data.frames** de R son todas vectores
y este es el motivo por el cual R requiere
que todas las columnas sean del mismo tipo de datos básico.

::::::::::::::::::::::::::::::::::::::  discussion

## Discusión 1

¿Por qué R es tan obstinado acerca de lo que ponemos en nuestras columnas de datos?
¿Cómo nos ayuda esto?

:::::::::::::::  solution

## Discusión 1

Al mantener todos los elementos de una columna del mismo tipo, podemos hacer
suposiciones simples sobre nuestros datos; si puedes interpretar un elemento
en una columna como un número, entonces puedes interpretar *todos* los elementos como números,
y por tanto no hace falta comprobarlo cada vez.
Esta consistencia es lo que se suele mencionar como *datos limpios*;
a la larga, la consistencia estricta hace nuestras vidas más fáciles cuando usamos R.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

También puedes crear vectores con contenido explícito con la función **combine** o `c()`:


``` r
mi_vector <- c(2,6,3)
mi_vector
```

``` output
[1] 2 6 3
```

``` r
str(mi_vector)
```

``` output
 num [1:3] 2 6 3
```

Dado lo que aprendimos hasta ahora, ¿qué crees que hace el siguiente código?


``` r
otro_vector <- c(2,6,'3')
```

Esto se denomina *coerción de tipos de datos* y es motivo de muchas sorpresas y la razón por la cual es necesario conocer
los tipos de datos básicos y cómo R los interpreta. Cuando R encuentra una mezcla de tipos de datos (en este caso **numeric** y **character**) para combinarlos en un vector, va a forzarlos a ser del mismo tipo.

Considera:


``` r
vector_coercion <- c('a', TRUE)
str(vector_coercion)
```

``` output
 chr [1:2] "a" "TRUE"
```

``` r
otro_vector_coercion <- c(0, TRUE)
str(otro_vector_coercion)
```

``` output
 num [1:2] 0 1
```

Las reglas de coerción son: `logical` -> `integer` -> `numeric` -> `complex` ->
`character`, donde -> se puede leer como *se transforma en*.
Puedes intentar forzar la coerción de acuerdo a esta cadena usando las funciones `as.`:


``` r
vector_caracteres <- c('0','2','4')
vector_caracteres
```

``` output
[1] "0" "2" "4"
```

``` r
str(vector_caracteres)
```

``` output
 chr [1:3] "0" "2" "4"
```

``` r
caracteres_coercionados_numerico <- as.numeric(vector_caracteres)
caracteres_coercionados_numerico
```

``` output
[1] 0 2 4
```

``` r
numerico_coercionado_logico <- as.logical(caracteres_coercionados_numerico)
numerico_coercionado_logico
```

``` output
[1] FALSE  TRUE  TRUE
```

Como puedes notar, es sorprendete ver qué pasa cuando R forza la conversión de un tipo de datos a otro. Es decir, si tus datos no lucen como pensabas que deberían lucir, puede ser culpa de la coerción de tipos. Por lo tanto, asegúrate que todos los elementos de tus vectores y las columnas de tus **data.frames** sean del mismo tipo o te encontrarás con sorpresas desagradables.

Pero la coerción de tipos también puede ser muy útil. Por ejemplo, en los datos de `gatos`,
`le_gusta_cuerda` es numérica, pero sabemos que los 1s y 0s en realidad representan **`TRUE`** y **`FALSE`**
(una forma habitual de representarlos). Deberíamos usar el tipo de datos
**`logical`** en este caso, que tiene dos estados: **`TRUE`** o **`FALSE`**, que es exactamente
lo que nuestros datos representan. Podemos convertir esta columna al tipo de datos **`logical`**
usando la función `as.logical`:


``` r
gatos$le_gusta_cuerda
```

``` output
[1] 1 0 1
```

``` r
class(gatos$le_gusta_cuerda)
```

``` output
[1] "integer"
```

``` r
gatos$le_gusta_cuerda <- as.logical(gatos$le_gusta_cuerda)
gatos$le_gusta_cuerda
```

``` output
[1]  TRUE FALSE  TRUE
```

``` r
class(gatos$le_gusta_cuerda)
```

``` output
[1] "logical"
```

La función **combine**, `c()`, también agregará elementos al final de un vector existente:


``` r
ab <- c('a', 'b')
ab
```

``` output
[1] "a" "b"
```

``` r
abc <- c(ab, 'c')
abc
```

``` output
[1] "a" "b" "c"
```

También puedes hacer una serie de números así:


``` r
mySerie <- 1:5
mySerie
```

``` output
[1] 1 2 3 4 5
```

``` r
str(mySerie)
```

``` output
 int [1:5] 1 2 3 4 5
```

``` r
class(mySerie)
```

``` output
[1] "integer"
```

Finalmente, puedes darle nombres a los elementos de tu vector:


``` r
names(mySerie) <- c("a", "b", "c", "d", "e")
mySerie
```

``` output
a b c d e 
1 2 3 4 5 
```

``` r
str(mySerie)
```

``` output
 Named int [1:5] 1 2 3 4 5
 - attr(*, "names")= chr [1:5] "a" "b" "c" "d" ...
```

``` r
class(mySerie)
```

``` output
[1] "integer"
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 1

Comienza construyendo un vector con los números del 1 al 26.
Multiplica el vector por 2 y asigna al vector resultante, los nombres de A hasta Z
(Pista: hay un vector pre-definido llamado **`LETTERS`**)

:::::::::::::::  solution

## Solución del desafío 1


``` r
x <- 1:26
x <- x * 2
names(x) <- LETTERS
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Factores

Otra estructura de datos importante se llama **factor**.


``` r
str(gatos$color)
```

``` output
 Factor w/ 3 levels "atigrado","mixto",..: 2 3 1
```

Los factores usualmente parecen caracteres, pero se usan para representar información categórica.
Por ejemplo, construyamos un vector de **strings** con etiquetas para las coloraciones para todos los
gatos en nuestro estudio:


``` r
capas <- c('atigrado', 'carey', 'carey', 'negro', 'atigrado')
capas
```

``` output
[1] "atigrado" "carey"    "carey"    "negro"    "atigrado"
```

``` r
str(capas)
```

``` output
 chr [1:5] "atigrado" "carey" "carey" "negro" "atigrado"
```

Podemos convertir un vector en un **factor** de la siguiente manera:


``` r
categorias <- factor(capas)
class(categorias)
```

``` output
[1] "factor"
```

``` r
str(categorias)
```

``` output
 Factor w/ 3 levels "atigrado","carey",..: 1 2 2 3 1
```

Ahora R puede interpretar que hay tres posibles categorías en nuestros datos - pero también
hizo algo sorprendente: en lugar de imprimir los **strings** como se las dimos, imprimió una
serie de números. R ha reemplazado las categorías con índices numéricos, lo cual es necesario porque
muchos cálculos estadísticos usan esa representación para datos categóricos:


``` r
class(capas)
```

``` output
[1] "character"
```

``` r
class(categorias)
```

``` output
[1] "factor"
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 2

¿Hay algún **factor** en nuestro **data.frame** `gatos`? ¿Cuál es el nombre?
Intenta usar `?read.csv` para darte cuenta cómo mantener las columnas de texto como vectores de caracteres
en lugar de factores; luego escribe uno o más comandos para mostrar que el **factor** en
`gatos` es en realidad un vector de caracteres cuando se carga de esta manera.

:::::::::::::::  solution

## Solución al desafío 2

Una solución es usar el argumento `stringAsFactors`:


``` r
gatos <- read.csv(file="data/gatos-data.csv", stringsAsFactors=FALSE)
str(gatos$color)
```

Otra solución es usar el argumento `colClasses`
que permiten un control más fino.


``` r
gatos <- read.csv(file="data/gatos-data.csv", colClasses=c(NA, NA, "character"))
str(gatos$color)
```

Nota: Los nuevos estudiantes encuentran los archivos de ayuda difíciles de entender; asegúrese de hacerles saber
que esto es normal, y anímelos a que tomen su mejor opción en función del significado semántico,
incluso si no están seguros.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

En las funciones de modelado, es importante saber cuáles son los niveles de referencia. Se asume
que es el primer factor, pero por defecto los factores están etiquetados en
orden alfabetico. Puedes cambiar esto especificando los niveles:


``` r
misdatos <- c("caso", "control", "control", "caso")
factor_orden <- factor(misdatos, levels = c("control", "caso"))
str(factor_orden)
```

``` output
 Factor w/ 2 levels "control","caso": 2 1 1 2
```

En este caso, le hemos dicho explícitamente a R que "control" debería estar representado por 1, y
"caso" por 2. Esta designación puede ser muy importante para interpretar los
resultados de modelos estadísticos!

## Listas

Otra estructura de datos que querrás en tu bolsa de trucos es `list`. Una lista
es más simple en algunos aspectos que los otros tipos, porque puedes poner cualquier cosa
que tú quieras en ella:


``` r
lista <- list(1, "a", TRUE, 1+4i)
lista
```

``` output
[[1]]
[1] 1

[[2]]
[1] "a"

[[3]]
[1] TRUE

[[4]]
[1] 1+4i
```

``` r
otra_lista <- list(title = "Numbers", numbers = 1:10, data = TRUE )
otra_lista
```

``` output
$title
[1] "Numbers"

$numbers
 [1]  1  2  3  4  5  6  7  8  9 10

$data
[1] TRUE
```

Ahora veamos algo interesante acerca de nuestro **data.frame**; ¿Qué pasa si corremos la siguiente línea?


``` r
typeof(gatos)
```

``` output
[1] "list"
```

Vemos que los **data.frames** parecen listas 'en su cara oculta' - esto es porque un
**data.frame** es realmente una lista de vectores y factores, como debe ser -
para mantener esas columnas que son una combinación de vectores y factores,
el **data.frame** necesita algo más flexible que un vector para poner todas las
columnas juntas en una tabla. En otras palabras, un `data.frame` es una
lista especial en la que todos los vectores deben tener la misma longitud.

En nuestro ejemplo de `gatos`, tenemos una variable **integer**, una **double** y una **logical**. Como
ya hemos visto, cada columna del **data.frame** es un vector.


``` r
gatos$color
```

``` output
[1] mixto    negro    atigrado
Levels: atigrado mixto negro
```

``` r
gatos[,1]
```

``` output
[1] mixto    negro    atigrado
Levels: atigrado mixto negro
```

``` r
typeof(gatos[,1])
```

``` output
[1] "integer"
```

``` r
str(gatos[,1])
```

``` output
 Factor w/ 3 levels "atigrado","mixto",..: 2 3 1
```

Cada fila es una *observación* de diferentes variables del mismo **data.frame**, y
por lo tanto puede estar compuesto de elementos de diferentes tipos.


``` r
gatos[1,]
```

``` output
  color peso le_gusta_cuerda
1 mixto  2.1            TRUE
```

``` r
typeof(gatos[1,])
```

``` output
[1] "list"
```

``` r
str(gatos[1,])
```

``` output
'data.frame':	1 obs. of  3 variables:
 $ color          : Factor w/ 3 levels "atigrado","mixto",..: 2
 $ peso           : num 2.1
 $ le_gusta_cuerda: logi TRUE
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 3

Hay varias maneras sutilmente diferentes de indicar variables, observaciones y elementos de **data.frames**:

- `gatos[1]`
- `gatos[[1]]`
- `gatos$color`
- `gatos["color"]`
- `gatos[1, 1]`
- `gatos[, 1]`
- `gatos[1, ]`

Investiga cada uno de los ejemplos anteriores y explica el resultado de cada uno.

*Sugerencia:* Usa la función **`typeof()`** para examinar el resultado en cada caso.

:::::::::::::::  solution

## Solución al desafío 3


``` r
gatos[1]
```

``` output
     color
1    mixto
2    negro
3 atigrado
```

Podemos interpretar un **data frame** como una lista de vectores. Un único par de corchetes `[1]`
resulta en la primera proyección de la lista, como otra lista. En este caso es la primera columna del
**data frame**.


``` r
gatos[[1]]
```

``` output
[1] mixto    negro    atigrado
Levels: atigrado mixto negro
```

El doble corchete `[[1]]` devuelve el contenido del elemento de la lista. En este caso,
es el contenido de la primera columna, un *vector* de tipo *factor*.


``` r
gatos$color
```

``` output
[1] mixto    negro    atigrado
Levels: atigrado mixto negro
```

Este ejemplo usa el caracter `$` para direccionar elementos por nombre. *color* es la
primera columna del *data frame*, de nuevo un *vector* de tipo *factor*.


``` r
gatos["color"]
```

``` output
     color
1    mixto
2    negro
3 atigrado
```

Aquí estamos usando un solo corchete `["color"]` reemplazando el número del índice con
el nombre de la columna. Como el ejemplo 1, el objeto devuelto es un *list*.


``` r
gatos[1, 1]
```

``` output
[1] mixto
Levels: atigrado mixto negro
```

Este ejemplo usa un solo corchete, pero esta vez proporcionamos coordenadas de fila y columna.
El objeto devuelto es el valor en la fila 1, columna 1. El objeto es un *integer* pero como
es parte de un *vector* de tipo *factor*, R muestra la etiqueta "mixto" asociada con el valor entero.


``` r
gatos[, 1]
```

``` output
[1] mixto    negro    atigrado
Levels: atigrado mixto negro
```

Al igual que en el ejemplo anterior, utilizamos corchetes simples y proporcionamos
las coordenadas de fila y columna. La coordenada de la fila no se especifica,
R interpreta este valor faltante como todos los elementos en este *column* *vector*.


``` r
gatos[1, ]
```

``` output
  color peso le_gusta_cuerda
1 mixto  2.1            TRUE
```

De nuevo, utilizamos el corchete simple con las coordenadas de fila y columna.
La coordenada de la columna no está especificada. El valor de retorno es una *list*
que contiene todos los valores en la primera fila.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Matrices

Por último, pero no menos importante, están las matrices. Podemos declarar una matriz llena de ceros de
la siguiente forma:


``` r
matrix_example <- matrix(0, ncol=6, nrow=3)
matrix_example
```

``` output
     [,1] [,2] [,3] [,4] [,5] [,6]
[1,]    0    0    0    0    0    0
[2,]    0    0    0    0    0    0
[3,]    0    0    0    0    0    0
```

Y de manera similar a otras estructuras de datos, podemos preguntar cosas sobre la matriz:


``` r
class(matrix_example)
```

``` output
[1] "matrix" "array" 
```

``` r
typeof(matrix_example)
```

``` output
[1] "double"
```

``` r
str(matrix_example)
```

``` output
 num [1:3, 1:6] 0 0 0 0 0 0 0 0 0 0 ...
```

``` r
dim(matrix_example)
```

``` output
[1] 3 6
```

``` r
nrow(matrix_example)
```

``` output
[1] 3
```

``` r
ncol(matrix_example)
```

``` output
[1] 6
```

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 4

¿Cuál crees que es el resultado del comando
`length(matrix_example)`?
Inténtalo.
¿Estabas en lo correcto? ¿Por qué / por qué no?

:::::::::::::::  solution

## Solución al desafío 4

¿Cuál crees que es el resultado del comando
`length(matrix_example)`?


``` r
matrix_example <- matrix(0, ncol=6, nrow=3)
length(matrix_example)
```

``` output
[1] 18
```

Debido a que una matriz es un vector con atributos de dimensión añadidos, `length`
proporciona la cantidad total de elementos en la matriz.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 5

Construye otra matriz, esta vez conteniendo los números 1:50,
con 5 columnas y 10 renglones.
¿Cómo llenó la función **`matrix`** de manera predeterminada la matriz, por columna o por renglón?
Investiga como cambiar este comportamento.
(Sugerencia: lee la documentación de la función **`matrix`**.)

:::::::::::::::  solution

## Solución al desafío 5

Construye otra matriz, esta vez conteniendo los números 1:50,
con 5 columnas y 10 renglones.
¿Cómo llenó la función **`matrix`** de manera predeterminada la matriz, por columna o por renglón?
Investiga como cambiar este comportamento.
(Sugerencia: lee la documentación de la función **`matrix`**.)


``` r
x <- matrix(1:50, ncol=5, nrow=10)
x <- matrix(1:50, ncol=5, nrow=10, byrow = TRUE) # to fill by row
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 6

Crea una lista de longitud dos que contenga un vector de caracteres para cada una de las secciones en esta parte del curso:

- tipos de datos
- estructura de datos

Inicializa cada vector de caracteres con los nombres de los tipos de datos y estructuras de datos
que hemos visto hasta ahora.

:::::::::::::::  solution

## Solución al desafío 6


``` r
dataTypes <- c('double', 'complex', 'integer', 'character', 'logical')
dataStructures <- c('data.frame', 'vector', 'factor', 'list', 'matrix')
answer <- list(dataTypes, dataStructures)
```

Nota: es útil hacer una lista en el pizarrón o en papel colgado en la pared listando
todos los tipos y estructuras de datos y mantener la lista durante el resto del curso
para recordar la importancia de estos elementos básicos.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Desafío 7

Considera la salida de R para la siguiente matriz:


``` output
     [,1] [,2]
[1,]    4    1
[2,]    9    5
[3,]   10    7
```

¿Cuál fué el comando correcto para escribir esta matriz? Examina
cada comando e intenta determinar el correcto antes de escribirlos.
Piensa en qué matrices producirán los otros comandos.

1. `matrix(c(4, 1, 9, 5, 10, 7), nrow = 3)`
2. `matrix(c(4, 9, 10, 1, 5, 7), ncol = 2, byrow = TRUE)`
3. `matrix(c(4, 9, 10, 1, 5, 7), nrow = 2)`
4. `matrix(c(4, 1, 9, 5, 10, 7), ncol = 2, byrow = TRUE)`

:::::::::::::::  solution

## Solución al desafío 7

Considera la salida de R para la siguiente matriz:


``` output
     [,1] [,2]
[1,]    4    1
[2,]    9    5
[3,]   10    7
```

¿Cuál era el comando correcto para escribir esta matriz? Examina
cada comando e intenta determinar el correcto antes de escribirlos.
Piensa en qué matrices producirán los otros comandos.


``` r
matrix(c(4, 1, 9, 5, 10, 7), ncol = 2, byrow = TRUE)
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- Usar `read.csv` para leer los datos tabulares en R.
- Los vectores, factores, listas y dataframes son estructuras de datos en R
- Los tipos de datos básicos en R son **double**, **integer**, **complex**, **logical**, y **character**.
- Los **factors** representan variables categóricas en R.

::::::::::::::::::::::::::::::::::::::::::::::::::


