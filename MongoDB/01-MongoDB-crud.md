# Crud y consultas en MongoDB

## Crear una Base de Datos
Solo se crea si contiene por lo menos una coleccion
use db1

**use bd1**

## Como crear una coleccion
use db1
db.createCollection("Empleado")

## Mostrar lac colecciones
show collections

## Insertar un documento
```json
db.alumnos.insertOne(
{
 nombre: "Soyla",
 apellido1: "Vaca",
 edad: 32,
 ciudad: "San Miguel de la Piedras"
}
)
```

## Inserción de un documento más complejo con array 
```json
db.alumnos.insertOne(
{
    nombre: "Joaquin",
    apellido: "Dorian",
    apellidos2: "Guerrero",
    edad: 15,
    aficiones: [
               "Cerveza", "Hueva", "Canabis"
               ]
}
)
```
``` json
db.alumnos.insertOne(

{
nombre: "Jose Luis",
 apellido1: "Herrera",
 apellido2: "Gallardo",
edad: 41,
estudios: [
        "Ingenieria en sistemas computacionales",
        "Maestria en Tecnologias de la informacion"
],
experiencia: {
         lenguaje: "SQL",
         sbd: "SQL Server",
         aniosExp: 14 
}
}
)
```

``` json
db.alumnos.insert(
    {
        _id: 3, 
        nombre: "Sergio",
        apellido: "Ramos",
        equipo: "Moterrey",
        aficiones: ["Dinero","Hombres","Fiesta"],
        talentos: {
            futbol: true,
            bañarse: false
        }
    }
)
```

## Insertar Multiples COnsultas
``` json
db.alumnos.find({_id:3})
[
  {
    _id: 3,
    nombre: 'Sergio',
    apellido: 'Ramos',
    equipo: 'Moterrey',
    aficiones: [ 'Dinero', 'Hombres', 'Fiesta' ],
    talentos: { futbol: true, 'bañarse': false }
  }
]
```

```json
dbejemplo> db.alumnos.insertMany(
[
  {
    _id: 12,
    nombre: "Roberto",
    apellido: "Gomez",
    edad: "23",
    descripcion: "Es un comediante bueno"
},
{
    nombre: "Luis",
    apellido: "Suarez",
    edad: 43,
    habilidades: [
        "correr", "dormir", "morder"
        ],
        direciones:  {
             calle: "Del infierno",
             numero: 666,
             ciudad: "Mexico"
             },
             esposas: [
                {
                    nombre: "Mariso",
                    edad: 20,
                    pension: 350,
                    hijos: ["Joaquin", "Citlali"]
                    },
                    {
            nombre: "Lisa",
            edad: 46,
            pension: 6500.56,
            complaciente: true
            }
            
            ]
        }
    ]
)            

```

# Practica 

1. Bases de Datos, colecciones e inserts 

    . Nos conectamos con MongoSH al MongoDB
    . Crear una base de datos denominada Curso

    ```json 
    use curso
    ``` 

    . Verificar que la base de datos no existe 

    ``` json
    show dbs / show databases 
    ``` 
    crear una coleccion denomida "Facturas" con el comando  createCollection y comprobar que aparece tanto la coleccion como la base de datos


# Practica1

## Cargar Datos
[Libros.json](./data/libros.json)

## Busquedas. Condiciones simples de Igualdad. Metodo find()

1. Seleccionar todos los documentos de la coleccion libros.

    ``` json
db.libros.find({})
    ``` 

2. Mostrar todos lod documentos que sean la editorial bibio

    ``` json
    db.libros.find({editorial:"Biblio"})
    ``` 

3. Mostar todos los documentos que el precio sea 25 

    ``` json
    db.libros.find({precio:25})
    ``` 

4. Selecionar todos los documentos donde el titulo sea json para todos 

    ``` json
    db.libros.find({titulo: "JSON para todos"})
    ``` 

## Operadores de Comparacion 

[Operadores de comparacion](https://www.mongodb.com/docs/manual/reference/operator/query/)

[Operadores relacionales](./img/Operadores%20Relacionales.png)

1. Mostar todos los documentos donde el precio sea mayor a 25

``` json
 db.libros.find({ precio: { $gt: 25 } })
 ```

 2. Mostar los documentos donde el precio sea 25

``` json
 db.libros.find({ precio: { $eq: 25 } })
  ```

 3. Mostar los documentos cuaya cantidad sea menor a 5

``` json
 db.libros.find({ cantidad: { $lt: 5 } })
 ```

 4. Mostar los documentos que pertenezcana a la editorial biblio o planeta

 ``` json
  db.libros.find({ editorial:{$in:["Biblio", "Planeta"]}})
``` 

5. Mostrar todos los documentos de libros que cuesten 20 o 25

 ``` json
db.libros.find({ precio:{$in:[20, 25]}})
 ```
 
 6. Mostrar todos los documentos de libros que no cuesten 20 o 25

 ``` json
db.libros.find({ precio:{$nin:[20, 25]}})
``` 

7. Mostrar el primer documento de libros que cueste 20 o 25

 ``` json
 db.libros.findOne({ precio:{$in:[20, 25]}})
``` 


## Operadores Logicos

[Operadores Logicos](https://www.mongodb.com/docs/manual/reference/operator/query/)

![Operadores logicos](./img/Operadores%20Logicos.png)


### Operador AND

Dos posibles opciones de AND

1. La siemple, mediante condiciones separadas por comas

***sintaxis:***

``` json
db.coleccion.find({condicion1, condicion2}) -> Con esto asume que es una AND
``` 

2. Usando el operador $and 

***sintaxis:***

``` json
db.coleccion.find({$and:[{condicion1},{condicion2}]})
```


1. Mostrar todos aquellos libros que cuesten mas de 25 y cuya cantidad sea inferior a 15

***Forma Simple***


``` json
db.libros.find({ precio:{ $gt:25 }, cantidad:{$lt:15}})
``` 

2. Mostrar todos aquellos libros que cuestan mas de 25 y cuya cantidad sea inferior a 15 y id igual a 4

``` json
db.libros.find({ precio:{ $gt:25 }, cantidad:{ $lt:15 }, _id:{$eq:4}})
``` 

***Operador $and***

1. Mostrar todos aquellos libros que cuesten mas de 25 y cuya cantidad sea inferior a 15

``` json
db.libros.find(
{
    $and:[
        {precio:{$gt:25}},
        {cantida:{ $lt:15 }}
    ]
}
)
``` 
***Operador OR***

1. Mostar todos aquelos libros que cuesten más de 25 o cuya cantidad sea inferior a 15 
``` json
db.libros.find(
{
    $or:[
        {precio:{$gt:25}},
        {cantida:{ $lt:15 }}
    ]
}
)
``` 
### AND y OR combinadas 

1. Mostrar los libros de la editorial Biblio, con precio mayor a 40 o libros de la editorial Planeta con precio mayor a 30

``` json
db.libros.find(
{
    $or:[
        {$and:[{editorial:"Biblio"}, {precio:{$gt:30}}]},
        {$and:[{editorial:{$eq:"Planeta"}}, {precio:{$gt:20}}]}
    ]
})

``` 

``` json
db.libros.find(
{
    $or:[
        {editorial:"Biblio", precio:{$gt:30}},
        {editorial:{$eq:"Planeta"}, precio:{$gt:20}}
    ]
})

``` 

## Proyecto de Columnas

*** Sintaxis ***

db.coleccion.find(filtro, columnas)

db.libros.find({},{titulo:1})

1. Seleccionar todos los documentos, mostrando el titulo y la editorial

``` json
db.libros.find({},{titulo:1,editorial:1})
``` 

2. Seleccionar todos los documentos, mostrando el titulo y la editorial sin id

``` json
db.libros.find({},{titulo:1,editorial:1, _id:0})
``` 

3. Seleccionar todos los documentos de la editorial planeta, mostrando solamente el titulo y la editorial

``` json
db.libros.find({editorial:"Planeta"},{titulo:1,editorial:1, _id:0})
 
``` 

## Operador exists (Permite saber si un campo se encuentra o no en un documento)

``` json
db.libros.find(
{
    editorial:{$exists:true}
})
``` 

``` json
db.libros.insertOne(
{
    _id:10,
    titulo: "Mongo en entornos gráficos",
    editorial: "Terra",
    precio: 125
})
``` 

1. Mostrar todos lo documentos que no contengan el campo cantidad

``` json

db.libros.find(
    {
        cantidad:{$exists:false}
    }
)
``` 

## Operador Type (Permite preguntar si un determinado campo corresponde con un tipo)

[Operador Type](https://www.mongodb.com/docs/manual/reference/operator/query/type/#mongodb-query-op.-type)

1. Mostar todos los documentos donde el precio sean dobles

```json
db.libros.find(
{
    precio:{$type:1}
})
``` 

2. Mostar todos los documentos donde el precio sean enteros

```json
db.libros.find(
{
    precio:{$type:16}
})
``` 

```json
db.libros.insertOne(
{
    _id:11,
    titulo: "IA",
    editorial:"Tierra",
    precio:125.4,
    cantidad:20
})
``` 

```json
db.libros.find(
{
    precio:{$type:1}, {_id:0}
})
``` 

```json

db.libros.insertMany([
 {
    _id: 12,
    titulo: 'IA',
    editorial: 'Terra',
    precio: 125, 
	cantidad: 20
  },
  {
    _id: 13,
    titulo: 'Python para todos',
    editorial: 2001,
    precio: 200, 
	cantidad: 30
  }]
  )

``` 

1. Seleccionar los documentos donde la editorial sea de tipo entero

```json
db.libros.find(
{
    editorial:{$type:16}
})
``` 

```json

db.libros.find(
{
    editorial:{$type:"int"}
})
```

2. Seleccionar todos los documentos donde la editorial sea string

```json
db.libros.find(
{
    editorial:{$type:"string"}
})
``` 

```json
db.libros.find(
{
    editorial:{$type:2}
})
``` 

## Practica de Consultas

1. Instalar las tools de mongodb
[DatabaseTools](https://www.mongodb.com/try/download/database-tools)

2. Cargar el json empleados (Debemos estar ubicados en la carpeta donde se encuentra el JSON empleados)

- En local: 
  comando:
   mongoimport --db curso --collection empleados --file empleados.json

- Docker:
  comando:
   mongoimport --db curso --collection empleados --file empleados.json --port 2718(cambia el puerto)


# Modificando Documentos 
## Comandos importantes

1. updateOne -> Modificar un solo documentos
1. updateMany -> Modificar multiples documentos
1. replaceOne -> Sustiruir el contenido completo de un documento 

Tiene ek siguiente formato:

```json
db.collention.updateOne(
{filtro},
{operador:}
)
``` 

[Operadores Update](https://www.mongodb.com/docs/manual/reference/operator/update/)

### Operador set 

1. Modificar un documento 

```json
db.libros.updateOne({titulo:"Python para todos"}, {$set:{titulo:"Java para todos"}})
``` 

2. Actualizar el precio a 100 y la cantidad a 50 para el id:10

```json
db.libros.updateOne({_id:10},{$set:{precio:100,cantidad:50}})
```

### Modificar Multiples Documentos 

-- Modificar todos los documentos donde el precio sea mayor a 100 a un precio de 150

```json
db.libros.updateMany(
{precio:{$gt:100}},
{$set:{precio:150}}
)
```

2. Operador $inc y $mul

- Actulizar con un incremento de 5 todos los documentos 

```json
db.libros.updateMany(
    {},
    {$inc:{precio:5}}
)
```

- Actualizar con multiplicacion de 2 todos los documentos que la cantidad sean mayores 20

```json
db.libros.updateMany({cantidad:{$gt:20}},{$mul:{cantidad:2}})
```

- Actualizar todos los documentos donde el precio sea mayor a 20 y se multiplique por 2 la cantidad y el precio

```json
db.libros.updateMany({precio:{$gt:20}},{$mul:{precio:2}},{$mul:{cantidad:2}})
```

```json

db.libros.updateMany({precio:{$gt:20}},{$mul:{cantidad:2, precio:2}})
```

3. Remmplaza Documentos  (replaceOne)

```json
db.libros.replaceOne({_id:2},{titulo:"De la tierra a la luna", autor:"Julio Verne", precio:500})

```

# Borrar Documentos

1. deleteOne -> Elimina un solo documento
2. deleteMany -> Elimina Multiples dpcumetos

1. Eliminar el documento con id 2

```json
db.libros.deleteOne({_id:2})
```

2. Eliminar los documentos donde la cantida sea mayor o igual a 150

```json
db.libros.deleteMany({cantidad:{$gte:100}})
```

# Expresiones Regulares

1. Buscar los libros que contengann el titulo la letra T

```json
db.libros.find({titulo:/t/})
```

2. Buscar los libros que en el titulo contengan la palabra json

```json
db.libros.find({titulo:/JSON/})
```

3. Buscar todos los documentos que en el titulo termines en tos

```json
db.libros.find({titulo:/tos$/})
```

4. Todos los documentos que en el titulo comiencen con J

```json
db.libros.find({titulo:/^J/})
```


# Operador $regex

[Operador Regex](https://www.mongodb.com/docs/manual/reference/operator/query/regex/)

- Seleccionar los libros que contengan la palabra para

```json
db.libros.find({titulo:{$regex: 'para'}})
```

```json
db.libros.find({titulo: {$regex: /JSON/}})
```

- Distinguir entre mayúsculas y minúsculas

```json
db.libros.find({titulo: {$regex: {/json/}}}) ->No distingue entre mayúsculas y minúsculas
```

```json
db.libros.find({titulo: {$regex: /json/, $options:"i"}}) -> Si distingue entre mayúsculas y minúsculas
```

```json
db.libros.find({titulo: {$regex: /json/i}})
```

- Seleccionar todos los libros que comiencen con J o j

```json
db.libros.find({titulo: {$regex: /^j/i}})
```

- Seleccionar todos los libros que terminen es

```json
db.libros.find({titulo: {$regex: /es$/i} })
```
```json
db.libros.find({titulo: {$regex: 'es$', $options: 'i'} })
```

# Método sort (Ordenar documentos)

1. Ordenar los libros de manera ascendente por el precio

```json
db.libros.find({}, {titulo:1, precio:1, _id:0}).sort({precio: 1})
```

2. Ordenar los libros de manera descendente por el precio

```json
db.libros.find({}, {titulo:1, precio:1, _id:0}).sort({precio: -1})
```

3. Ordenar los libros de manera ascendente por la editorial y de manera descendente por el precio, mostrando el titulo, el precio y la editorial

```json
db.libros.find({}, {titulo:1, precio:1, editorial:1, _id:0}).sort({editorial: 1, precio:-1})
```

# Otros métodos: skip, limit, size

```json
db.libros.find({}).size()
```
```json
db.libros.find({titulo: {$regex:/Java/i}}).size()
```

- Buscar todos los libros pero mostrando los 2 primeros

```json
db.libros.find({}, {titulo:1, editorial:1, precio:1, _id:0}).limit(2)
```

- Mostrar los 3 últimos libros 

```json
db.libros.find({}, {titulo:1, editorial:1, precio:1, _id:0}).sort({precio:-1}).limit(3)

db.getCollection('libros')
  .find({}, { titulo: 1, editorial: 1, _id: 0 })
  .sort({ titulo: -1 })
  .limit(4);
```

```json
db.libros.find({}, {titulo: 1, editorial:1, precio:1}).skip(2)
```

-Seleccionar todos los libros ordenados por título de forma descendente saltando los dos primeros documentos y mostrando el tamaño

```json
db.libros.find({}, {titulo:1, editorial:1, precio:1, _id:0}).sort({titulo:-1}).skip(2).size()
```

# Borrar colecciones y base de 

```json
use db5
db.createCollection('ejemplo')
show collections

db.ejemplo.insertOne({nombre: 'Chapuin'})

db.ejemplo.drop()

db.dropDatabase()
```

mongosh "mongodb+srv://cluster0.epd6m.mongodb.net/" --apiVersion 1 --username leslie_neri