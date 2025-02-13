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

![Operadores relacionales](./img/Operadores%20Relacionales.png)

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

``` json
 
``` 