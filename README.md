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
