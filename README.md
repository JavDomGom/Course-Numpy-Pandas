# Pandas

Pandas es una librería de Python creada específicamente para el análisis de datos. Tiene un elemento clave denominada **Dataframe**, que no es más que una serie de datos en una tabla donde podremos ver los registros en filas y columnas ordenados por un índice. Cada una de las columnas puede tener un tipo de dato diferente, por ejemplo datos de tipo entero, float, strings, objetos, etc.

## Importación de Pandas

Por convención la imporatción de la librería Pandas en nuestro código se realiza de la siguiente manera:

```python
import pandas as pd
```

A partir de este momento podremos utilizar el alias `pd` para invocar cualquier método de la librería Pandas. Por ejemplo, para importar un fichero CSV lo haríamos de la siguiente manera:

```python
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

```python
df = pd.read_csv(
    r'../datasets/Info_pais.csv',
    encoding='ISO-8859-1',
    delimiter=';'
)
```

Una vez definido el dataframe y almacenado en la variable `df`, si no estamos haciendo uso del atributo `nrows`, podremos mostrar una cabecera con los 5 primeros elementos utlizando el método `head()` de la siguiente manera:

```python
df.head()
```

<p align="center"><img src="img/pandas_00.png"></p>
<br>

A este método `head()` se le puede indicar dentro de los paréntesis el número de registros que se quieren mostrar, por ejemplo para mostrar solo los 20 primeros registros se haría se la siguiente manera:

```python
df.head(20)
```

<p align="center"><img src="img/pandas_01.png"></p>
<br>

## Ordenación de los datos

Ahora que ya tenemos importada la librería `pandas` e instanciado un dataframe `df` podremos tratar o manipular la información de diferentes maneras. En este caso vamos a ordenar los datos en base a una columna, por ejemplo la columna `Esperanza de vida`, y lo haremos en orden ascendente de la siguiente manera:

```python
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

```python
import matplotlib.pyplot as plt
```

A partir de este momento podremos utilizar en nuestro código el alias `plt` para acceder a todos los métodos de esta librería. Veamos un ejemplo en el que cargaremos un array de datos para el eje `x`, llamado por ejemplo `year`, y otro array de datos para el eje `y`, llamado por ejemplo `value`:

```python
year = [2020, 2021, 2022]
value = [5, 6, 9]
```

Finalmente podremos generar un gráfico de líneas mediante el método `plot()` de la librería `matplotlib`, al que le tendremos que pasar como argumentos primero el array de datos que queremos utilizar en el eje `x` y segundo el array que usaremos para el eje `y`:

```python
plt.plot(year, value)
```

<p align="center"><img src="img/matplot_00.png"></p>
<br>

Si quisieramos generar otro tipo de gráfico con los mismos datos podríamos utilizar por ejemplo el método `scatter()` al que también hay que pasarle como argumentos los datos de los ejes `x` e `y`:

```python
plt.scatter(year, value)
```

<p align="center"><img src="img/matplot_01.png"></p>
<br>

Esta sería una manera de crear gráficos muy sencillos a partir de un par de listas con datos, pero normalmente se suelen utilizar fuentes de datos más grandes y complejas como son los dataframes que hemos visto anteriormente. Para visualizar con la librería `matplotlib` la infomación de un dataframe podemos hacerlo de la siguiente manera. Primero importamos el dataframe, en este caso uno llamado `hum_temp.csv` con algunos datos random relativos a humedad y temperatura:

```python
import pandas as pd
df = pd.read_csv(
    r'../datasets/hum_temp.csv',
    encoding='ISO-8859-1',
    delimiter=';'
)
```

Comprobamos que se ha cargado correctamente:

```python
df
```

<p align="center"><img src="img/matplot_02.png"></p>
<br>

A continuación usamos el método `plot()` de la instancia `plt` al que le pasariamos el nombre del dataframe, en este caso `df` y entre corchetes el nombre de la columna que queremos utilizar para representarlo en el eje `y`, si no se especifica el eje `x` en este se utilizarán los valores del índice de los registros de datos:

```python
plt.plot(df['temperature'])
```

<p align="center"><img src="img/matplot_03.png"></p>
<br>

Como se puede ver, los datos de la columna `temperature` aparecen en el eje `y` y el índice de los datos en el eje `x`.

Veamos una manera mejor de representar los datos de este dataframe, en este caso usaremos el método `plot()` con el dataframe `df` de la siguiente manera:

```python
df['humidity'].plot()
```

<p align="center"><img src="img/matplot_04.png"></p>
<br>

Si eliminamos el nombre de la columna entre corchetes y especificamos únicamente el nombre del dataframe `df` podremos ver en el mísmo gráfico todos los registros de cada columna en líneas diferentes, cada una con un color distinto.

```python
df.plot()
```

<p align="center"><img src="img/matplot_05.png"></p>
<br>

### Ejemplo Standard & Poor's 500

Veamos otro ejemplo en el que vamos a trabajar con otro dataset. En este caso vamos a ver la evolución de la cotización de un índice bursatil como el Standard & Poor's 500.

Primero creamos un dataframe llamado `df_sp500` en el que importaremos el archivo CSV `SP500_data.csv` de la siguiente manera:
```python
df_sp500 = pd.read_csv(
    r'../datasets/SP500_data.csv',
    encoding='ISO-8859-1',
    delimiter=','
)
```

Si imprimimos la cabecera podremos visualizar los 5 primeros registros:

```python
df_sp500.head()
```

<p align="center"><img src="img/matplot_06.png"></p>
<br>

Para visualizar este dataframe primero importaremos la librería `matplotlib` de la siguiente manera:

```python
import matplotlib.pyplot as plt
```

Ahora vamos a representar la columna del cierre bursatil de cada día, columna `Close`:

```python
df_sp500['Close'].plot()
```

<p align="center"><img src="img/matplot_07.png"></p>
<br>

En este gráfico podremos ver la evolución del índice bursatil, pero si nos fijamos en el eje `x` ha representado el índice de cada registro. Para poder representar en el eje `x` la fecha del dato debemos especificar que el índice del datframe `df_sp500` ha de ser la columna `Date`, de la siguiente forma:

```python
df_sp500.index = df_sp500['Date']
```

Si volvemos a mostrar la cabecera veremos que ahora se han insertado en el índice los valores del campo Date:

```python
df_sp500.head()
```

<p align="center"><img src="img/matplot_08.png"></p>
<br>

volvemos a representar el gráfico de nuevo con la intrucción de antes:

```python
df_sp500['Close'].plot()
```

<p align="center"><img src="img/matplot_09.png"></p>
<br>

Ahora vemos que en el eje `x` se representan los valores del campo o columna `Date`.

### Ejemplo COVID-19 en España por comunidades autónomas

En este otro ejemplo vamos a trabajar con un dataset que he obtenido de [este site](https://datos.gob.es/en/catalogo/e05070101-evolucion-de-enfermedad-por-el-coronavirus-covid-19). Se tratan de los casos detectados de COVID-19 por comunidades autónomas en España.

Primero creamos un dataframe llamado `df_covid19_ccaas` en el que importaremos el archivo CSV `datos_ccaas.csv` de la siguiente manera:
```python
df_covid19_ccaas = pd.read_csv(
    r'../datasets/datos_ccaas.csv',
    encoding='ISO-8859-1',
    delimiter=','
)
```

Si imprimimos la cabecera podremos visualizar los 5 primeros registros:

```python
df_covid19_ccaas.head()
```

<p align="center"><img src="img/matplot_10.png"></p>
<br>

Como siempre, importamos la librería `matplotlib` si no lo tuviéramos importada de ejecuciones anteriores.

```python
import matplotlib.pyplot as plt
```

Estableceremos tal y como hemos visto en el ejemplo anterior el campo `fecha` como índice y los datos de la columna `num_casos` en el eje `y` de la siguiente manera:

```python
df_covid19_ccaas.index = df_covid19_ccaas['fecha']
df_covid19_ccaas['num_casos'].plot()
```

<p align="center"><img src="img/matplot_11.png"></p>
<br>
