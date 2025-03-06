## Consultas 

1. Cargar el archivo empleados.json 


- En local: 
  comando:
   mongoimport --db curso --collection empleados --file empleados.json

- Docker:
  comando:
   mongoimport --db curso --collection empleados --file empleados.json --port 2718(cambia el puerto)


   2. Utilizar la base de datos Curso

```json
use curso
``` 

3. Buscar todos los empleados que trabajen en Google

```json
db.empleados.find({empresa:"Google"})
db.empleados.find({ empresa:{$eq:"Google"}})
``` 

4. Empleados que vivan en Perú

```json
db.empleados.find({pais:"Peru"})
db.empleados.find({ empresa:{$eq:"Peru"}})
``` 

5. Empleados que ganen más de 8000 dolares

```json
db.empleados.find({salario:{$gt:8000}})
``` 

6. Empleados con ventas inferiores a 10000

```json
db.empleados.find({ventas:{$lt:10000}})
``` 

7. Realizar la consulta anterior pero devolviendo una sola dila

```json
db.empleados.findOne({ventas:{$lt:10000}})
``` 

8. Empleados que trabajan en Google o en Yahoo con el operador $in

```json
db.empleados.find({ empresa:{$in:["Google", "Yahoo"]}})
``` 

9. Empelados de amazon que ganen mas de 9000 dolares

```json
db.empleados.find({ empresa:{$eq:"Amazon"}, salario:{$gt:9000}})
``` 

10. Empleados que trabajan en Google o en Yahoo con el operador $or

```json
db.empleados.find({ $or:[{empresa:"Amazon"}, {empresa:{$eq:"Yahoo"}}]})
``` 

11. Empleados que trabajen en Yahoo que ganen mas de 6000 o empleados que trabajen en Google

```json
db.empleados.find(
{ 
    $or:[ 
        { $and:[{empresa:"Yahoo"}, {salario:{$gt:6000}}]},
        { $and:[{empresa:{$eq:"Google"}}, {ventas:{$lt:20000}}]}
]})
``` 

12. Viualizar el nombre, apellidos y el país de cada empleado

```json
db.empleados.find({},{nombre:1,apellidos:1,pais:1, _id:0})
``` 