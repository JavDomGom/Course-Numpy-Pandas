# Pandas

Pandas es una librería de Python creada específicamente para el análisis de datos. Tiene un elemento clave denominada **Dataframe**, que no es más que una serie de datos en una tabla donde podremos ver los registros en filas y columnas ordenados por un índice. Cada una de las columnas puede tener un tipo de dato diferente, por ejemplo datos de tipo entero, float, strings, objetos, etc.

## Importación de Pandas

Por convención la imporatción de la librería Pandas en nuestro código se realiza de la siguiente manera:

```Python
import pandas as pd
```

A partir de este momento podremos utilizar el alias `pd` para invocar cualquier método de la librería Pandas. Por ejemplo, para importar un fichero CSV lo haríamos de la siguiente manera:

```Python
df = pd.read_csv(
    r'file.csv',
    index_col=0,
    nrows=5,
    encoding='ISO-8859-1',
    delimiter=';'
)
```
En este ejemplo se han utilizado algunos parámteros del método `read-csv`, donde:
* `r'file.csv'` es el archivo CSV con el que se va a trabajar. La `r` del inicio indica que la cadena de texto que hay dentro de las comillas se va a tratar en crudo (raw). Este parámetro es obligatorio.
* `index_col` es el índice de la columna que queremos utilizar como índice. Es opcional y si no se utiliza el dataframe mostrará como índice los números del índice de cada registro.
* `nrows` es el número de registros que queremos leer del fichero. Es opcional.
* `encoding` es el tipo de codificación de los datos. Es opcional.
* `delimiter` es el delimitador de datos empleado en el CSV. Es opcional, pero si no se utiliza no delimitará los datos de cada columna.

Para ver un ejemplo real usaremos el dataset llamado `Info_pais.csv` de la carpeta `datasets` con los siguiente atributos:

```Python
df = pd.read_csv(
    r'../datasets/Info_pais.csv',
    encoding='ISO-8859-1',
    delimiter=';'
)
```

Una vez definido el dataframe y almacenado en la variable `df`, si no estamos haciendo uso del atributo `nrows`, podremos mostrar una cabecera con los 5 primeros elementos utlizando el método `head()` de la siguiente manera:

```Python
df.head()
```

<p align="center"><img src="img/pandas_00.png"></p>
<br>

A este método `head()` se le puede indicar dentro de los paréntesis el número de registros que se quieren mostrar, por ejemplo para mostrar solo los 20 primeros registros se haría se la siguiente manera:

```Python
df.head(20)
```

<p align="center"><img src="img/pandas_01.png"></p>
<br>

## Ordenación de los datos

Ahora que ya tenemos importada la librería `pandas` e instanciado un dataframe `df` podremos tratar o manipular la información de diferentes maneras. En este caso vamos a ordenar los datos en base a una columna, por ejemplo la columna `Esperanza de vida`, y lo haremos en orden ascendente de la siguiente manera:

```Python
df_order = df.sort_values(
    'Esperanza de vida',
    ascending=True
)
```

El atributo `ascending` es opcional, si no se utiliza siempre es ascendente, pero conviene ponerlo con valor `True` o `False`, según nos interese.

Si ahora mostramos la cabecera con los 5 primeros registros veremos que están ordenados de menor a mayor según los datos de la columna `Esperanza de vida`:

<p align="center"><img src="img/pandas_02.png"></p>
<br>

## Visualización con Matplotlib

Matplotlib es una librería de visualización que tiene una gran veriedad de gráficos y que es fácilmente configurable. Más información acerca de los gráficos disponibles en https://matplotlib.org/gallery/index.html. Matplotlib viene con la instalación de Anaconda. Para utilizar esta librería debemos importarla de la siguiente manera:

```Python
import matplotlib.pyplot as plt
```

A partir de este momento podremos utilizar en nuestro código el alias `plt` para acceder a todos los métodos de esta librería. Veamos un ejemplo en el que cargaremos un array de datos para el eje `x`, llamado por ejemplo `year`, y otro array de datos para el eje `y`, llamado por ejemplo `value`:

```Python
year = [2020, 2021, 2022]
value = [5, 6, 9]
```

Finalmente podremos generar un gráfico de líneas mediante el método `plot()` de la librería `matplotlib`, al que le tendremos que pasar como argumentos primero el array de datos que queremos utilizar en el eje `x` y segundo el array que usaremos para el eje `y`:

```Python
plt.plot(year, value)
```

<p align="center"><img src="img/matplot_00.png"></p>
<br>

Si quisieramos generar otro tipo de gráfico con los mismos datos podríamos utilizar por ejemplo el método `scatter()` al que también hay que pasarle como argumentos los datos de los ejes `x` e `y`:

```Python
plt.scatter(year, value)
```

<p align="center"><img src="img/matplot_01.png"></p>
<br>

Esta sería una manera de crear gráficos muy sencillos a partir de un par de listas con datos, pero normalmente se suelen utilizar fuentes de datos más grandes y complejas como son los dataframes que hemos visto anteriormente. Para visualizar con la librería `matplotlib` la infomación de un dataframe podemos hacerlo de la siguiente manera. Primero importamos el dataframe, en este caso uno llamado `hum_temp.csv` con algunos datos random relativos a humedad y temperatura:

```Python
import pandas as pd
df = pd.read_csv(
    r'../datasets/hum_temp.csv',
    encoding='ISO-8859-1',
    delimiter=';'
)
```

Comprobamos que se ha cargado correctamente:

```Python
df
```

<p align="center"><img src="img/matplot_02.png"></p>
<br>

A continuación usamos el método `plot()` de la instancia `plt` al que le pasariamos el nombre del dataframe, en este caso `df` y entre corchetes el nombre de la columna que queremos utilizar para representarlo en el eje `y`, si no se especifica el eje `x` en este se utilizarán los valores del índice de los registros de datos:

```Python
plt.plot(df['temperature'])
```

<p align="center"><img src="img/matplot_03.png"></p>
<br>

Como se puede ver, los datos de la columna `temperature` aparecen en el eje `y` y el índice de los datos en el eje `x`.

Veamos una manera mejor de representar los datos de este dataframe, en este caso usaremos el método `plot()` con el dataframe `df` de la siguiente manera:

```Python
df['humidity'].plot()
```

<p align="center"><img src="img/matplot_04.png"></p>
<br>

Si eliminamos el nombre de la columna entre corchetes y especificamos únicamente el nombre del dataframe `df` podremos ver en el mísmo gráfico todos los registros de cada columna en líneas diferentes, cada una con un color distinto.

```Python
df.plot()
```

<p align="center"><img src="img/matplot_05.png"></p>
<br>

### Ejemplo Standard & Poor's 500

Veamos otro ejemplo en el que vamos a trabajar con otro dataset. En este caso vamos a ver la evolución de la cotización de un índice bursatil como el Standard & Poor's 500.

Primero creamos un dataframe llamado `df_sp500` en el que importaremos el archivo CSV `SP500_data.csv` de la siguiente manera:
```Python
df_sp500 = pd.read_csv(
    r'../datasets/SP500_data.csv',
    encoding='ISO-8859-1',
    delimiter=','
)
```

Si imprimimos la cabecera podremos visualizar los 5 primeros registros:

```Python
df_sp500.head()
```

<p align="center"><img src="img/matplot_06.png"></p>
<br>

Para visualizar este dataframe primero importaremos la librería `matplotlib` de la siguiente manera:

```Python
import matplotlib.pyplot as plt
```

Ahora vamos a representar la columna del cierre bursatil de cada día, columna `Close`:

```Python
df_sp500['Close'].plot()
```

<p align="center"><img src="img/matplot_07.png"></p>
<br>

En este gráfico podremos ver la evolución del índice bursatil, pero si nos fijamos en el eje `x` ha representado el índice de cada registro. Para poder representar en el eje `x` la fecha del dato debemos especificar que el índice del datframe `df_sp500` ha de ser la columna `Date`, de la siguiente forma:

```Python
df_sp500.index = df_sp500['Date']
```

Si volvemos a mostrar la cabecera veremos que ahora se han insertado en el índice los valores del campo Date:

```Python
df_sp500.head()
```

<p align="center"><img src="img/matplot_08.png"></p>
<br>

volvemos a representar el gráfico de nuevo con la intrucción de antes:

```Python
df_sp500['Close'].plot()
```

<p align="center"><img src="img/matplot_09.png"></p>
<br>

Ahora vemos que en el eje `x` se representan los valores del campo o columna `Date`.

### Ejemplo COVID-19 en España por comunidades autónomas

En este otro ejemplo vamos a trabajar con un dataset que he obtenido de [este site](https://datos.gob.es/en/catalogo/e05070101-evolucion-de-enfermedad-por-el-coronavirus-covid-19). Se tratan de los casos detectados de COVID-19 por comunidades autónomas en España.

Primero creamos un dataframe llamado `df_covid19_ccaas` en el que importaremos el archivo CSV `datos_ccaas.csv` de la siguiente manera:
```Python
df_covid19_ccaas = pd.read_csv(
    r'../datasets/datos_ccaas.csv',
    encoding='ISO-8859-1',
    delimiter=','
)
```

Si imprimimos la cabecera podremos visualizar los 5 primeros registros:

```Python
df_covid19_ccaas.head()
```

<p align="center"><img src="img/matplot_10.png"></p>
<br>

Como siempre, importamos la librería `matplotlib` si no lo tuviéramos importada de ejecuciones anteriores.

```Python
import matplotlib.pyplot as plt
```

Estableceremos tal y como hemos visto en el ejemplo anterior el campo `fecha` como índice y los datos de la columna `num_casos` en el eje `y` de la siguiente manera:

```Python
df_covid19_ccaas.index = df_covid19_ccaas['fecha']
df_covid19_ccaas['num_casos'].plot()
```

<p align="center"><img src="img/matplot_11.png"></p>
<br>

## Ejemplo esperanza de vida frente a renta per capita por países

En este ejemplo vamos a utilizar el caso de uso que vimos anteriormente en el que teníamos la esperanza de vida frente a la renta per capita por países. Vamos a ver si existe una correlación entre estas dos variables.

```Python
df = pd.read_csv(
    r'../datasets/Info_pais.csv',
    encoding='ISO-8859-1',
    delimiter=';'
)
```

Ahora pondremos los datos en orden ascendente según los valores de la columna `Esperanza de vida` de la siguiente manera:

```Python
df_order = df.sort_values(
    'Esperanza de vida',
    ascending=True
)

df_order.head()
```

<p align="center"><img src="img/pandas_02.png"></p>
<br>

Es el momento de crear el gráfico mediante el método `scatter()`. Representaremos en el eje `x` los datos de la columna `Renta per capita` y en el eje `y` los datos de la columna `Esperanza de vida`.

```Python
plt.scatter(df_order['Renta per capita'], df_order['Esperanza de vida'])
```

<p align="center"><img src="img/matplot_12.png"></p>
<br>

De momento se aprecia un gráfico que nos da una idea aproximada de cómo se ven los datos. Podemos añadir un título y etiquetas a los ejes `x` e `y` del gráfico de la siguiente manera:

```Python
plt.scatter(
    df_order['Renta per capita'],
    df_order['Esperanza de vida']
)
plt.title('Renta per capita vs Esperanza de vida')
plt.xlabel('Renta per capita')
plt.ylabel('Esperanza de vida')
```

En el gráfico que acabamos de generar los puntos  aparecen uniformes. Vamos a configurar el gráfico para que sean proporcionales tanto en tamaño como en color para cada país. Para ello debemos crear una nueva columna llamda por ejemplo `df_order['Poblacion_normalizada']` que normalice frente al máximo de población, por lo que le asignaremos como valores el valor de la columna `Población` dividido entre el valor máximo de esta columna `Población`, de este modo lo estaríamos escalando o normalizando.

```Python
df_order['Poblacion_normalizada'] = df_order['Poblacion']/max(df_order['Poblacion'])
```

De este modo, el país que tenga la población más alta quedaría escalado a `1` y el resto de países quedarían normalizado en base a este valor máximo.

Existen grandes diferencias en número de habitantes entre unos países y otros, por ejemplo China con 1200 millones y otros que podrían tener 50000. Para evitar que un país con esta gran cantidad de habitantes inunde el gráfico es recomendable que en vez de dividir la población de cada país entre el máximo de población, hacer la división del máximo entre `10000`, para no tener un factor tan elevado.

```Python
df_order['Poblacion_normalizada'] = df_order['Poblacion']/(max(df_order['Poblacion'])/10000)

df_order.head()
```

<p align="center"><img src="img/matplot_13.png"></p>
<br>


Ahora podremos usar esta nueva columna con los datos escalados para crear una visualización de los datos mucho más potente.

Es el momento de generar un nueva visualización utilizando la nueva columna de datos normalizados que hemos generado, por ejemplo con el siguiente código:

```Python
plt.scatter(
    df_order['Renta per capita'],
    df_order['Esperanza de vida'],
    s=df_order['Poblacion_normalizada']
)
plt.title('Renta per capita vs Esperanza de vida')
plt.xlabel('Renta per capita')
plt.ylabel('Esperanza de vida')
```

Para modificar el tamaño hemos utilizado el parámetro `s` (size) y como valor usamos la columna `Poblacion_normalizada`.

<p align="center"><img src="img/matplot_14.png"></p>
<br>

El problema que vemos es que la visualización es muy pequeña, pero podemos mejorar esto añadiendo a nuestro código lo siguiente para aumentar las pulgadas de nuestro gráfico:

```Python
plt.scatter(
    df_order['Renta per capita'],
    df_order['Esperanza de vida'],
    s=df_order['Poblacion_normalizada']
)
plt.title('Renta per capita vs Esperanza de vida')
plt.xlabel('Renta per capita')
plt.ylabel('Esperanza de vida')

fig = plt.gcf()
fig.set_size_inches(14.5, 10)
```

<p align="center"><img src="img/matplot_15.png"></p>
<br>

De este modo se ve mucho más grande. Si fuera necesario se pueden cambiar los valores de la función `set_size_inches()` por otros más adecuados para ajustar el tamaño.

Si nos fijamos bien, ahora se representa cada burbuja de cada país de un tamaño diferente, dependiendo del valor que tenga en la nueva columna que hemos generado con los datos normalizados.

Ahora vamos a modificar el color. Para ello debemos añadir el atributo `c` (color), a conutnuación del atributo `s` (size), y como valor vamos a usar de nuevo la columna `Poblacion_normalizada`. Quedaría del siguiente modo:

```Python
plt.scatter(
    df_order['Renta per capita'],
    df_order['Esperanza de vida'],
    s=df_order['Poblacion_normalizada'],
    c=df_order['Poblacion_normalizada']
)
plt.title('Renta per capita vs Esperanza de vida')
plt.xlabel('Renta per capita')
plt.ylabel('Esperanza de vida')

fig = plt.gcf()
fig.set_size_inches(14.5, 10)
```

<p align="center"><img src="img/matplot_16.png"></p>
<br>

En esta nueva visualización vemos que ya no solo se representa cada país en un tamaño diferente, sino que también en un color en función de la población.

También podremos añadir la etiqueta del nombre del país dentro de cada burbuja utilizando el método `annotate()`, por ejemplo añadiéndoselo solo a los `10` primeros países con el siguiente código:

```Python
plt.scatter(
    df_order['Renta per capita'],
    df_order['Esperanza de vida'],
    s=df_order['Poblacion_normalizada'],
    c=df_order['Poblacion_normalizada']
)
plt.title('Renta per capita vs Esperanza de vida')
plt.xlabel('Renta per capita')
plt.ylabel('Esperanza de vida')

fig = plt.gcf()
fig.set_size_inches(14.5, 10)

for i in range(1, 10):
    plt.annotate(
        df_order['País'][i],
        (
            df_order['Renta per capita'][i],
            df_order['Esperanza de vida'][i]
        )
    )
```

<p align="center"><img src="img/matplot_17.png"></p>
<br>

Otro especto que podríamos mejorar es la representación de los datos del eje `y`, son los datos de `120` países y actualmente se ven muy juntos, se ve mal. esto se soluciona fácilmente con la fución `yticks()` a la que le pasaremos tres argumentos, que son un `1` representando el primer dato, `120` representamdo el último dato, y `10` para indicar que los queremos mostrar de diez en diez.

```Python
plt.scatter(
    df_order['Renta per capita'],
    df_order['Esperanza de vida'],
    s=df_order['Poblacion_normalizada'],
    c=df_order['Poblacion_normalizada']
)
plt.title('Renta per capita vs Esperanza de vida')
plt.xlabel('Renta per capita')
plt.ylabel('Esperanza de vida')

fig = plt.gcf()
fig.set_size_inches(14.5, 10)

for i in range(1, 10):
    plt.annotate(
        df_order['País'][i],
        (
            df_order['Renta per capita'][i],
            df_order['Esperanza de vida'][i]
        )
    )


plt.yticks(ticks=range(1, 120, 10))
```

<p align="center"><img src="img/matplot_18.png"></p>
<br>

De este modo hemos creado una visualizción de los datos muy potente y de una manera muy sencilla. La conclusión que podemos sacar de este gráfico es que efectivamente existe una correlación entre la renta per capita y la esperanza de vida según el país. Podemos ver que conforme la renta per capita aumenta la esperanza de vida también aumenta. También podemos deducir que el número de población no afecta a la esperanza de vida, ya que en la visualización que hemos creado se pueden ver países con una gran cantidas de población que no están entre los valores más bajos en cuanto a esperanza de vida se refiere.

# Conceptos estadísticos básicos

## Variables discretas y continuas

Se dice que una variable es *discreta* cuando no puede tomar ningún valor entre dos consecutivos, y que es *continua* cuando puede tomar cualquier valor dentro de un intervalo. Por ejemplo:

* Variable discreta:
```Python
numero_alumnos_por_clase = [28, 31, 30, 27, 25, 36]
```
* Variable continua:
```Python
temperatura_madrid = [28.3, 29.0, 22.47, 30.02, 17.6]
```

## Cálculo de la media y de la mediana

La *media* y la *mediana* son dos conceptos estadísticos básicos que debemos conocer. La media se calcula sumando todos los valores de un conjunto y dividiéndo el resultado entre el número total de valores.

<p align="center"><img src="img/latex_media.png" width="175"></p>
<br>

Veamos un ejemplo, supongamos que tenemos la variable `v1` con la siguiente lista de valores:

```Python
v1 = [4, 6, 3, 5, 8, 9, 2, 12, 16, 4, 7, 42, 13, 6, 7]
```

Para calcular la media primero debemos sumar todos los valores de la variable `v1` y dividirlo entre la cantidad de valores:

```Python
sum(v1)/len(v1)
9.6
```

En este caso la media de los valores de la variable `v1` es `9.6`.

La mediana es el valor central de los valores de una variable, una vez estos están ordenados de manera ascendente. Veamos un ejemplo, utilizando los valores de la variable `v1` de antes, solo que con los elementos de la lista ordenados de forma ascendente y alamcenados en una nueva variable llamada `v2`:

```Python
v2 = [2, 3, 4, 4, 5, 6, 6, 7, 7, 8, 9, 12, 13, 16, 42]
```

Para calcular la mediana podríamos hacer una función llamada `mediana` que reciba como argumento una lista de números, en nuestro caso le pasaremos la variable `v2`, por ejemplo:

```Python
def mediana(lista):
    lst_sorted = sorted(lista)
    lst_len = len(lista)
    index = (lst_len - 1) // 2 # Floor division.

    if (lst_len % 2):
        return lst_sorted[index]
    else:
        return (lst_sorted[index] + lst_sorted[index + 1])/2.0

mediana(v2)
7.0
```

Como resultado obtenemos que la mediana de estos valores es `7`, que coincide con la posición central de los elementos de la variable `v2`. Si la variable tuviese un número par de elementos, la mediana sería la suma de los dos elementos centrales dividido entre dos.

En la siguiente representacin gráfica podemos ver la distribución de los valores del ejemplo anterior en los que la media (`9.6`) se representa como una línea azúl y la mediana (`7.0`) como línea verde.

<p align="center"><img src="img/matplot_19.png"></p>
<br>

Existe una diferencia entre la media y la mediana porque hay un valor que dista mucho del resto de valores de la secuencia, en nuestro caso es el `45`. A este tipo de datos se los denomina **outliers**. Si por el contrario todos los valores de la secuencia tuvieran un valor más o menos parecido podríamos ver que la media y la media tendrían también un valor similar.

Otro ejemplo con valores en los que encontramos un outlaier aún más elevado:

```Python
v1 = [4, 6, 3, 5, 8, 9, 2, 12, 16, 4, 7, 282, 13, 6, 7]
v2 = [31, 23, 25, 20, 21, 29, 24, 26, 30, 27, 25, 24, 23, 32, 24]

def mediana(lista):
    lst_sorted = sorted(lista)
    lst_len = len(lista)
    index = (lst_len - 1) // 2 # Floor division.

    if (lst_len % 2):
        return lst_sorted[index]
    else:
        return (lst_sorted[index] + lst_sorted[index + 1])/2.0

mediana(v2)

print(f'La media de v1 es: {sum(v1)/len(v1)}')
print(f'La media de v2 es: {sum(v2)/len(v2)}')
print(f'La mediana de v1 es: {mediana(v1)}')
print(f'La mediana de v2 es: {mediana(v2)}')
```

Obtenemos el siguiente resultado:

```
La media de v1 es: 25.6
La media de v2 es: 25.6
La mediana de v1 es: 7
La mediana de v2 es: 25
```

Aquí vemos que la media en ambas variables `v1` y `v2` es `25.6`, sin embargo en las medianas existe una enorme diferencia, debido a que el resultado se ve más impactado por estos **outliers**, en este caso `282`, que dista mucho del resto de valores de la secuencia.

La media es un valor estadístico muy importante, pero habitualmente se utiliza la mediana ya que acaba siendo más representativo sobre la distrubición de nuestros datos, y evita todos estos posibles **outliers**.

## Varianza y desviación de una variable

La varianza de una variable nos indica la dispersión de un conjunto de datos respecto a su valor medio. Esto significa que si tenemos una varianza alta en nuestra variable, los valores de la variable van a estar más alejados del valor medio.

Matemáticamente la varianza de una variable se calcula de la siguiente manera:

1. A cada valor de la variable hay que restarle el promedio de la variable.
2. El resultado anterior se eleva al cuadrado.
3. Hay que hacer un sumatorio con el resultado de las operaciones anteriores para cada valor de la variable.
4. Se divide entre el número de valores de la variable.

<p align="center"><img src="img/latex_varianza.png" width="125"></p>
<br>


La deviación estándar se puede calcular con la raíz cuadrada de la varianza:

<p align="center"><img src="img/latex_desviacion_estandar.png" width="90"></p>
<br>

Veamos un caso de uso. Tenemos un avariable `x` que tiene como valor el peso de los tomates que hemos recolectado durantre un periodo de 8 días.

* Tomate x_1 = 60gr
* Tomate x_2 = 56gr
* Tomate x_3 = 61gr
* Tomate x_4 = 68gr
* Tomate x_5 = 51gr
* Tomate x_6 = 53gr
* Tomate x_7 = 69gr
* Tomate x_8 = 54gr

```Python
x = [60, 56, 61, 68, 51, 53, 69, 54]
```

El primer paso que hay que dar es calcular la media de la variable `x`, tal y como hemos visto anteriormente:

<p align="center"><img src="img/latex_media_caso_de_uso_00.png" width="175"></p>
<br>

```Python
sum(x)/len(x)
59.0
```

En el numerador tenemos la suma de todos los valores de la variable `x`, que en este caso de uso es `472`, en el denominador tenemos el número de elementos o datos que tiene la variable `x`, en este caso `8`. Como resultado la media es `59` gramos.

En el segundo paso tendremos restar a cada valor de la variable `x` la media y luego elevar el resultado al cuadrado. Al final hay que sumar todos los valores obtenidos en esta última operación, tal y como se muestra en la siguiente tabla:

<p align="center"><img src="img/latex_media_caso_de_uso_01.png" width="260"></p>
<br>

```Python
for n in map(lambda i : pow(i-59, 2), x):
    print(n)
1
9
4
81
64
36
100
25
```

```Python
sum(map(lambda i : pow(i-59, 2), x))
320
```

El último paso para calcular la varianza de `x` es dividir `320` entre el número de elementos de la variable `x`, que en este caso es `8`. Como resultado tenemos que la varianza de `x` es `40`.

<p align="center"><img src="img/latex_media_caso_de_uso_02.png" width="220"></p>
<br>

```Python
320/8
40.0
```

Mas allá de la varianza podemos calcular también la desviación estándar de la variable `x`, basta con calcular la raíz cuadrada de la varianza.

<p align="center"><img src="img/latex_media_caso_de_uso_03.png" width="210"></p>
<br>

```Python
import math
math.sqrt(40)
6.324555320336759
```

Obtenemos como resultado `6.32`. Esto nos permite tener un valor que se encuentra dentro del orden de magnitud de nuestra variable `x`, es decir, entre todos nuestros tomates existe una desviación estándar de `6.32` gramos.

Veamos qué pasaría si tuviéramos otro caso en el que recogiéramos otros 8 tomates y almacenáramos el peso de cada uno de estos tomates en esta nueva variable `y`:

* Tomate y_1 = 50gr
* Tomate y_2 = 66gr
* Tomate y_3 = 51gr
* Tomate y_4 = 78gr
* Tomate y_5 = 41gr
* Tomate y_6 = 63gr
* Tomate y_7 = 59gr
* Tomate y_8 = 64gr

```Python
y = [50, 66, 51, 78, 41, 63, 59, 64]
```
Vemos que hay una mayor dispersión en los datos de partida, sin embargo la media es `59`, la misma que en el caso de la variable `x`.

```Python
sum(y)/len(y)
59.0
```

Si ahora calculamos del mismo modo que antes el numerador del término de la varianza podremos ver que en la tabla hay valores mucho más elevandos que lo que teníamos previamente:

<p align="center"><img src="img/latex_media_caso_de_uso_04.png" width="260"></p>
<br>

```Python
for n in map(lambda i : pow(i-59, 2), y):
    print(n)
81
49
64
361
324
16
0
25
```

```Python
sum(map(lambda i : pow(i-59, 2), y))
920
```

Esto se debe a que la secuencia original de datos dista mucho más del valor promedio de `59` gramos. Con estos nuevos datos, si calculamos la varianza en este caso obtendremos un valor de `115`.

<p align="center"><img src="img/latex_media_caso_de_uso_05.png" width="225"></p>
<br>

```Python
920/8
115.0
```

Y la desviación estándar serían `10.72` gramos

<p align="center"><img src="img/latex_media_caso_de_uso_06.png" width="225"></p>
<br>

```Python
import math

math.sqrt(115)
10.723805294763608
```

Lo que hemos aprendido con estos dos ejemplos es que conforme el conjunto de valores tiene una mayor dispersión respecto al valor promedio, al final tendrá un valor de varianza superior. Por lo tanto la varianza nos proporciona una medida de la volatilidad o incertidumbre de una determinada variable o conjunto de datos. De hecho, si todos los valores de la variable fueran iguales el valor de la varianza sería `0`, es decir, no habría ningún tipo de incertidumbre.

Se suele utilizar más la varianza puesto que está menos influenciada por los valores positivos o negativos que pudiera haber en la diferencia entre el valor y el promedio, por lo tanto la varianza es más representativa a la hora de calcular la dispersión de los datos de nuestro conjunto de datos.

# NumPy

NumPy es una librería de Python enfocada en el cálculo numérico que nos permite realizar operaciones de una manera sencilla y rápida. Su objeto base es un vector de números denominado Array. Es una alternativa a las listas que hemos visto hasta ahora y nos va a permitir realizar una serie de funciones muy potentes. A diferencia de las listas, donde se opera de forma independiente en cada uno de los elementos, en los Arrays las operaciones se van a realizar sobre todo el Array simultáneamente. A continuación se muestra una diferencia entre listas y Arrays de NumPy a la hora de realizar una suma de elementos, por ejemplo, tenemos estas dos listas:

```Python
a = [1, 2, 3]
b = [4, 5, 6]
```

Si sumamos las dos listas obtenemos lo siguiente:

```Python
a + b
[1, 2, 3, 4, 5, 6]
```

Como se puede ver, no suma los elementos, solo concatena la lista `b` a continuación de la lista `a`. Veamos cómo se comparta una suma de Arrays con el mismo ejemplo:

```Python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

a + b
array([5, 7, 9])
```

En este caso se han sumado cada uno de los elementos del Array `a` con los elementos del array `b`.

Otra gran diferencia respecto a las listas tradicionales es que en un Array solo se admite un tipo de dato, normalmente numérico. Además NumPy nos va a servir como base de cálculo para otras librerías como Pandas o SciKit Learn.

## Importación de NumPy

Por convención la imporatción de la librería NumPy en nuestro código se realiza de la siguiente manera:

```Python
import numpy as np
```

## Ejemplo Índice de Masa Corporal (IMC)

En este ejemplo vamos a calcular el índice de masa corporal sobre los valores `peso` y `altura` de tres personas.

```Python
altura = [1.7, 1.65, 1.82]
peso = [67, 55, 72]
```

El cálculo que hay que hacer para obtener el IMC es dividir el peso entre el cuadrado de la altura:

```Python
peso / altura**2
```

Si quisiéramos calcular el IMC mediante listas tradicionales podríamos hacerlo fácilmente usando la función `zip()`, iterar a través de un bucle `for` e ir añadiéndo los resultados mediante compresión de listas del siguiente modo:

```Python
[p / a**2 for p, a in zip(peso, altura)]
[23.18339100346021, 20.202020202020204, 21.736505252988767]
```

El cálculo está bien hecho, pero de este modo es más complicado, entre otras cosas porque el cálculo de va haciendo secuencialmente elemento a elemento entre las listas, y en este caso no es mucho problema ya que son listas de solo 3 elementos, pero podría ser un problema al trabajar con listas de miles de elementos. Se realizar el mismo cálculo de una manera mucho más eficiente usando los Arrays de NumPy, veamos el ejemplo:

```Python
np_altura = np.array([1.7, 1.65, 1.82])
np_peso = np.array([67, 55, 72])

imc = np_peso / np_altura**2
imc
array([23.183391  , 20.2020202 , 21.73650525])
```

Además este tipo de objetos Array de NumPy son iterables del mismo modo que lo son algunas colecciones estándar de Python como las listas, tienen algunas propiedades como los slices que nos permiten navegar dentro del Array y acceder a determinados elementos ubicados en ciertas posiciones.

Otro uso interesante de los Arrays es que podemos evaluar todos los elementos del Array con una simple operación, como por ejemplo saber qué valores son mayores que `21`:

```Python
imc > 21
array([ True, False,  True])
```

En este caso el primer resultado es `True` puesto que se cumple que `23.183391` es mayor que `21`, el segundo es `False` porque `20.2020202` no es mayor que `21` y el tercero es `True` puesto que `21.73650525` sí es mayor que `21`.

Si quisiéramos obtener solo los elementos del array que cumplen la condición anterior podremos hacerlo de la siguiente manera:

```Python
imc[imc > 21]
array([23.183391  , 21.73650525])
```

## Ejemplo cálculo áreas de triángulos

En este ejemplo vamos a calcular masivamente con Arrays de NumPy el área de varios triángulos y quedarnos solo con aquellas áreas que sean `> 6.5`. Para calcular el área de un triángulo hay que multiplicar la base por la altura y dividirlo entre dos.

```Python
base * altura / 2
```

Para ello tenemos los siguiente dos Arrays:

```Python
bases_tri = np.array([2, 2.37, 3.05, 1.75, 4, 2.81])
alturas_tri = np.array([1.21, 2.6, 4.4, 7.03, 4.01, 5.25])
```

Ahora multiplicamos las bases por las alturas y lo dividimos entre `2` de la siguiente manera:

```Python
areas_tri = bases_tri * alturas_tri / 2
areas_tri
array([1.21   , 3.081  , 6.71   , 6.15125, 8.02   , 7.37625])
```

Como solo nos interesan las áreas que son `> 6.5` podremos obtener los valores que cumplan la condición de la siguiente manera:

```Python
areas_tri[areas_tri > 6.5]
array([6.71   , 8.02   , 7.37625])
```

También podemos hacer una conjunción de condiciones, por ejemplo para obtener solo las áreas que son `> 6.5` y también `< 8`, para ello usaremos el AND lógico mediante el símbolo `&` de la siguiente manera:

```Python
areas_tri[(areas_tri > 6.5) & (areas_tri < 8)]
array([6.71   , 7.37625])
```

## Arrays de dos dimensiones en NumPy

Con NumPy también podremos crear un Array de dos dimensiones. En realidad es una matriz de `m` filas por `n` columnas.

<p align="center"><img src="img/latex_array_2d_00.png" width="350"></p>
<br>

La sintaxis para crearla es la siguiente:

```Python
nombre_array = np.array([[valores_fila_1],
                         [valores_fila_2],
                         [valores_fila_m]])
```

Veamos cómo crear un array bidimensional como este:

<p align="center"><img src="img/latex_array_2d_01.png" width="135"></p>
<br>

```Python
a = np.array([[2, 7, 8], [4, 8, 10]])
a
array([[ 2,  7,  8],
       [ 4,  8, 10]])
```

Si quisiéramos obtener el valor `10` de nuestro array de dos dimensiones tendríamos que especificar el índice de la fila seguido del índice de la columna, teniendo en cuenta que los índices siempre empiezan por el número `0`. En este ejemplo el valor `10` se encuentra en la fila con índice `1` y la columna con índice `2`.

```Python
a[1, 2]
10
```

Ahora supongamos que queremos obtener todas las filas pero solo los valores de las columnas  primera y segunda. En este caso tendríamos que especificar en primer lugar que queremos todas las filas mediante los dos puntos `:`, y a continuación un slice `0:2` para indicar solo las columnas desde el índice `0` hasta el `1`, ya que en un slice el número que se indica al final no se muetra, es donde se para.

```Python
a[:, 0:2]
array([[2, 7],
       [4, 8]])
```

## Cálculo estadístico con NumPy

Para realizar cálculo estadístico NumPy nos ofrece una gran veriedad de funciones muy útiles y potentes. En esta sección veremos algunas de ellas que nos pueden servir para solucionar algunos cálculos que hemos visto en puntos anteriores de una manera mucho más sencilla y rápida.

Por ejemplo, supongamos que tenemos el siguiente Array con varios registros de temperaturas de una ciudad:

```Python
temperaturas = np.array([12, 13.5, 13, 14, 13.2, 14.8, 15, 15.16, 16, 16.2, 15.7, 17, 17.2, 16.8, 14, 14.2, 14.7, 16, 17.5])
```

Podríamos calcular la media o promedio usando la función `mean(array)` de la siguiente manera:

```Python
np.mean(temperaturas)
15.050526315789472
```

La mediana usando la función `median(array)`:

```Python
np.median(temperaturas)
15.0
```

Podremos obtener los valores mínimos y máximos con las funciones `min(array)` y `max(array)`.

```Python
np.min(temperaturas)
12.0
```

```Python
np.max(temperaturas)
17.5
```

Para calcular la varianza podremos usar la función `var(array)` de la siguiente manera:

```Python
np.var(temperaturas)
2.2893207756232683
```

La desviación estándar podremos obtenerla usando la función `std(array)` de la siguiente manera:

```Python
np.std(temperaturas)
1.5130501563475245
```

También tenemos la función `percentile(array, k)` a la que tendremos que pasarle como argumentos el array y también el percentil que queremos obtener, por ejemplo el `90%`:

```Python
np.percentile(temperaturas, 90)
17.04
```

Esto lo que quiere decir es que en el array de temperaturas no se supera la temperatura `17.04` en el `90%` de los casos, o dicho de otro modo, solo el `10%` de los datos supera la temperatura de `17.04`.

## Generación de datos random con NumPy

Una cualidad de NumPy muy interesante es que nos permite generar datos random tomando como partida diferentes parámetros estadísticos, como por ejemplo una media y una desviación estándar, y generar un Array de valores con dicha distribución estadística. La función es `random.normal()` necesita que le pasemos como argumentos una media, la desviación estándar y un número de muestras. Su sintaxis es la siguiente:

```Python
nombre_array = np.random.normal(
    media,
    desviacion_estandar,
    numero_muestras
)
```

Por ejemplo, si quisiéramos

```Python
array_gauss = np.random.normal(2, 0.5, 1000)
```

Como resultado tendremos un Array de `1000` elementos con la distribución estadística que hemos insertado. Si visualizamos los datos se ven de la siguiente manera:
