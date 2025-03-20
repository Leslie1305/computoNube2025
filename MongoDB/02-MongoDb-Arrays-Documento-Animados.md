# Busqueda de Arrays y Documentos Anidados

## Buscar dentro de un documento anidado 
```json
db.ciudades.find({
    alcalde:{
        nombre:"Crystal",
        apellidos:"Long",
        edad:76
    }
})
```

```json
db.ciudades.find({
    "alcalde.nombre":"Crystal" 
})
```

1. Seleccionar las ciudades donde los alcaldes tengas mas de 70 años

```json
db.ciudades.find(
{
    "alcalde.edad":{$gt:70}
}
)
```

2. Realizar la consulta anterior pero utilizando una proyección

```json
db.ciudades.find(
{"alcalde.edad":{$gt:70}},
{_id:0,nombre:1, "alcalde.edad":1}
)
```

3. Seleccionar las ciudades donde el alcalde tenga 76 años o 41

```json
db.ciudades.find({
    $or:[
        {"alcalde.edad":76},
        {"alcalde.edad":41}
        ]
})
```

## Consultas con Arrays

1. Crear una coleccion cazas 
db.createCollection("Cazas")

2. Cargar los documentos de la coleccion cazas, de la data cazas

## Ejemplos de consultta con Arrays

1. Buscar los documentos que tengan USA como paises

```json
db.cazas.find(
{paises:"usa"}
)
```

2. Buscar los documentos donde cualquier elemento del array dimensiones sea mayor a 5

```json
db.cazas.find(
{dimensiones:{$gt:5}}
)
```

3. Buscar los documentos donde las dimensiones del avión por ancho que es la posición 1, sea superior a 14

```json
db.cazas.find({
    "dimensiones.1":{$gt:14}
})
```

```json
db.cazas.find({
    "dimensiones.1":{$gt:14},
    {_id:0, modelo:1, dimensiones:1 }
})
```

## Arrays. Operador $all

1. Buscar los aviones que estan sirviendo en Egipto en Israel

```json
db.cazas.find({
    paises:{$all:["egipto","israel"]}
})
```

2. Seleccionar los cazas que sircen en inglaterra y españa 

```json
db.cazas.find({
    $and:[
        {paises:"inglaterra"},
        {paises:"españa"}
    ]
})
```

```json
db.cazas.find({
    paises: {$all:["inglaterra", "españa"]}
})
```

## Arrays Operador $elemMatch -> Permite hacer busquedas dentro de arrays, este busca la lista de condiciones que esta dentro del operador

1. Buscar que aviones tienen uso dentro de Inglaterra

```json
db.cazas.find({
    paises:{$elemtMatch:{$eq:"inglaterra"}}
})
```

2. Buscar los aviones donde las dimensiones sean menores a 20 o mayores a 15

```json
db.cazas.find({
    dimensiones:{$elemMatch:{$lt:20,$gt:15}}
})
```

## Buscar en arrays de documentos anidados

1. Utilizar la coleccion ciudades 
2. Buscar los consejeros de nombre Jeri Flower de edad 78 

```json
db.ciudades.find({
    consejeros:{identificador:1,nombre:"Jeri Flowers", edad:78}
})
```

3. Buscar en consejeros donde la edad mayor 70

```json
db.ciudades.find({
    "consejeros.edad":{$gt:70}
})
```

4. Buscar en consejeros en la posición 2 que esten entre 70 y 80 años

```json
db.ciudades.find({
    "consejeros.2.edad":{$gt:70, $lt:80}},
    {_id:0, "consejeros.nombre":1,"consejeros.edad":1
})
```

5. Burcar los consejeros de la posicion 0 que sean mayores a 70 

```json
db.ciudades.find({
    "consejeros.0.edad":{$gt:70}},
    {_id:0, "consejeros.nombre":1,"consejeros.edad":1
})
```

6. Buscar consejeros que sean mayores a 70
```json
db.ciudades.find({
    "consejeros.edad":{$gt:70}},
    {_id:0, "consejeros.nombre":1,"consejeros.edad":1
})
```