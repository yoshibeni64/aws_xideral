# Biblioteca - MySQL

## Crear la base de datos

```sql
CREATE DATABASE biblioteca;

USE biblioteca;

DROP TABLE libros_nombre;

CREATE TABLE libros_nombre (
    libro_id INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(100),
    autor VARCHAR(100),
    genero VARCHAR(40),
    anio_publicacion INT,
    numero_paginas INT,
    calificacion DECIMAL(3,1),
    disponible BOOL
);
```

## Insertar los 50 libros

```sql
INSERT INTO libros_nombre
(titulo, autor, genero, anio_publicacion, numero_paginas, calificacion, disponible)
VALUES

-- Realismo mágico
('Cien años de soledad', 'Gabriel García Márquez', 'Realismo mágico', 1967, 417, 9.4, TRUE),
('El amor en los tiempos del cólera', 'Gabriel García Márquez', 'Realismo mágico', 1985, 348, 9.1, TRUE),
('Pedro Páramo', 'Juan Rulfo', 'Realismo mágico', 1955, 124, 9.3, FALSE),
('La casa de los espíritus', 'Isabel Allende', 'Realismo mágico', 1982, 433, 8.9, TRUE),
('Como agua para chocolate', 'Laura Esquivel', 'Realismo mágico', 1989, 246, 8.7, TRUE),
('Aura', 'Carlos Fuentes', 'Realismo mágico', 1962, 62, 8.8, FALSE),
('Crónica de una muerte anunciada', 'Gabriel García Márquez', 'Realismo mágico', 1981, 122, 9.0, TRUE),
('El otoño del patriarca', 'Gabriel García Márquez', 'Realismo mágico', 1975, 271, 8.6, FALSE),
('Los recuerdos del porvenir', 'Elena Garro', 'Realismo mágico', 1963, 320, 8.5, TRUE),
('La muerte de Artemio Cruz', 'Carlos Fuentes', 'Realismo mágico', 1962, 315, 8.7, TRUE),
('Rayuela', 'Julio Cortázar', 'Realismo mágico', 1963, 736, 9.2, FALSE),
('El reino de este mundo', 'Alejo Carpentier', 'Realismo mágico', 1949, 160, 8.9, TRUE),

-- Ciencia ficción
('1984', 'George Orwell', 'Ciencia ficción', 1949, 328, 9.5, TRUE),
('Fahrenheit 451', 'Ray Bradbury', 'Ciencia ficción', 1953, 249, 9.0, FALSE),
('Dune', 'Frank Herbert', 'Ciencia ficción', 1965, 688, 9.4, TRUE),
('Neuromante', 'William Gibson', 'Ciencia ficción', 1984, 271, 8.6, TRUE),
('Solaris', 'Stanisław Lem', 'Ciencia ficción', 1961, 204, 8.8, FALSE),
('Fundación', 'Isaac Asimov', 'Ciencia ficción', 1951, 296, 9.1, TRUE),
('La guerra de los mundos', 'H. G. Wells', 'Ciencia ficción', 1898, 192, 8.7, FALSE),
('¿Sueñan los androides con ovejas eléctricas?', 'Philip K. Dick', 'Ciencia ficción', 1968, 244, 9.0, TRUE),

-- Romance
('Orgullo y prejuicio', 'Jane Austen', 'Romance', 1813, 432, 9.6, TRUE),
('Jane Eyre', 'Charlotte Brontë', 'Romance', 1847, 532, 9.2, FALSE),
('Cumbres borrascosas', 'Emily Brontë', 'Romance', 1847, 416, 8.9, TRUE),
('Ana Karenina', 'León Tolstói', 'Romance', 1878, 864, 9.3, FALSE),
('Sentido y sensibilidad', 'Jane Austen', 'Romance', 1811, 409, 8.8, TRUE),
('Romeo y Julieta', 'William Shakespeare', 'Romance', 1597, 320, 8.5, TRUE),

-- Terror
('El resplandor', 'Stephen King', 'Terror', 1977, 688, 9.1, FALSE),
('It', 'Stephen King', 'Terror', 1986, 1138, 9.0, TRUE),
('Drácula', 'Bram Stoker', 'Terror', 1897, 418, 9.3, TRUE),
('Frankenstein', 'Mary Shelley', 'Terror', 1818, 280, 9.2, FALSE),
('El exorcista', 'William Peter Blatty', 'Terror', 1971, 385, 8.6, TRUE),
('Cementerio de animales', 'Stephen King', 'Terror', 1983, 424, 8.8, FALSE),

-- Fantasía
('El principito', 'Antoine de Saint-Exupéry', 'Fantasía', 1943, 96, 9.7, TRUE),
('El hobbit', 'J. R. R. Tolkien', 'Fantasía', 1937, 310, 9.4, TRUE),
('La comunidad del anillo', 'J. R. R. Tolkien', 'Fantasía', 1954, 423, 9.5, FALSE),
('El león, la bruja y el armario', 'C. S. Lewis', 'Fantasía', 1950, 208, 9.0, TRUE),
('Harry Potter y la piedra filosofal', 'J. K. Rowling', 'Fantasía', 1997, 309, 9.6, TRUE),
('Harry Potter y la cámara secreta', 'J. K. Rowling', 'Fantasía', 1998, 341, 9.2, FALSE),
('Harry Potter y el prisionero de Azkaban', 'J. K. Rowling', 'Fantasía', 1999, 435, 9.5, TRUE),
('Alicia en el país de las maravillas', 'Lewis Carroll', 'Fantasía', 1865, 200, 8.9, TRUE),
('El nombre del viento', 'Patrick Rothfuss', 'Fantasía', 2007, 662, 9.1, FALSE),

-- Misterio
('El código Da Vinci', 'Dan Brown', 'Misterio', 2003, 689, 8.7, TRUE),
('Asesinato en el Orient Express', 'Agatha Christie', 'Misterio', 1934, 256, 9.4, FALSE),
('El asesinato de Roger Ackroyd', 'Agatha Christie', 'Misterio', 1926, 288, 9.3, TRUE),
('El sabueso de los Baskerville', 'Arthur Conan Doyle', 'Misterio', 1902, 256, 9.0, TRUE),
('La sombra del viento', 'Carlos Ruiz Zafón', 'Misterio', 2001, 576, 9.2, FALSE),
('La chica del tren', 'Paula Hawkins', 'Misterio', 2015, 395, 8.6, TRUE),
('Perdida', 'Gillian Flynn', 'Misterio', 2012, 432, 8.9, TRUE),
('El silencio de los corderos', 'Thomas Harris', 'Misterio', 1988, 352, 9.1, FALSE),
('Diez negritos', 'Agatha Christie', 'Misterio', 1939, 272, 9.5, TRUE);
```

# Consultas

## 1. Mostrar todos los libros

```sql
SELECT *
FROM libros_nombre;
```

## 2. Mostrar solamente el título, autor y género

```sql
SELECT titulo, autor, genero
FROM libros_nombre;
```

## 3. Mostrar los libros disponibles

```sql
SELECT *
FROM libros_nombre
WHERE disponible = 1;
```

## 4. Buscar los libros de un género específico

```sql
SELECT *
FROM libros_nombre
WHERE genero = "Romance";
```

## 5. Mostrar los libros publicados después del año 2000

```sql
SELECT *
FROM libros_nombre
WHERE anio_publicacion > 2000;
```

## 6. Mostrar los libros con calificación mayor a 8

```sql
SELECT *
FROM libros_nombre
WHERE calificacion > 8;
```

## 7. Ordenar los libros del más reciente al más antiguo

```sql
SELECT *
FROM libros_nombre
ORDER BY anio_publicacion DESC;
```

## 8. Mostrar el libro con mayor calificación

```sql
SELECT *
FROM libros_nombre
ORDER BY calificacion DESC
LIMIT 1;
```

## 9. Calcular el promedio de páginas de los libros

```sql
SELECT AVG(numero_paginas) AS promedio_paginas
FROM libros_nombre;
```

## 10. Contar cuántos libros existen por género

```sql
SELECT genero, COUNT(*) AS cantidad
FROM libros_nombre
GROUP BY genero;
```

## 11. Buscar libros cuyo título contenga una palabra utilizando LIKE

```sql
SELECT *
FROM libros_nombre
WHERE titulo LIKE "El%";
```

## 12. Cambiar un libro de disponible a no disponible utilizando UPDATE

```sql
UPDATE libros_nombre
SET disponible = FALSE
WHERE libro_id = 1;
```