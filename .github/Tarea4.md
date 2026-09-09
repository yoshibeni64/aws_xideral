# Conceptos y operaciones estadísticas

## Contar elementos - `array.count(n)`

Nos indica el número de veces que aparece un valor `n` en un conjunto de datos.

**Ejemplo:**

```python
calificaciones = [9, 10, 10, 10, 8, 7, 9, 10]
conteo = calificaciones.count(10)
print(conteo)
# Devuelve: 4
```

## Media aritmética - `np.mean(array)`

Nos indica el promedio de un conjunto de datos. El promedio es la suma de los datos de un conjunto entre la cantidad de datos del conjunto.

**Ejemplo:**

```python
calificaciones = [8, 9, 10]
promedio = np.mean(calificaciones)
print(promedio)
# Devuelve: 9.0
```

## Desviación estándar - `np.std(array)`

En estadística, la desviación estándar nos indica qué tan lejanos a la media (promedio) están los valores de un conjunto de datos. Una menor desviación estándar indica que los datos están cercanos a la media.

**Ejemplo:**

```python
calificaciones = [8, 9, 10]
desviacion = np.std(calificaciones)
print(desviacion)
# Devuelve: 0.816496580927726
```

## Valor mínimo - `np.min(array)`

Nos indica el valor más pequeño dentro de un conjunto de datos.

**Ejemplo:**

```python
calificaciones = [8, 9, 10]
minimo = np.min(calificaciones)
print(minimo)
# Devuelve: 8
```

## Valor máximo - `np.max(array)`

Nos indica el valor más grande dentro de un conjunto de datos.

**Ejemplo:**

```python
calificaciones = [8, 9, 10]
maximo = np.max(calificaciones)
print(maximo)
# Devuelve: 10
```

## Q1 - Primer cuartil (25%) - `np.percentile(array, 25)`

Nos indica el valor por debajo del cual se encuentra aproximadamente el 25% de los datos.

**Ejemplo:**

```python
calificaciones = [6, 7, 8, 9, 10]
q1 = np.percentile(calificaciones, 25)
print(q1)
# Devuelve: 7.0
```

## Q2 - Segundo cuartil (50%) - `np.percentile(array, 50)`

Nos indica el valor que divide los datos en dos partes iguales. También es llamado mediana.

**Ejemplo:**

```python
calificaciones = [6, 7, 8, 9, 10]
q2 = np.percentile(calificaciones, 50)
print(q2)
# Devuelve: 8.0
```

## Q3 - Tercer cuartil (75%) - `np.percentile(array, 75)`

Nos indica el valor por debajo del cual se encuentra aproximadamente el 75% de los datos.

**Ejemplo:**

```python
calificaciones = [6, 7, 8, 9, 10]
q3 = np.percentile(calificaciones, 75)
print(q3)
# Devuelve: 9.0
```
