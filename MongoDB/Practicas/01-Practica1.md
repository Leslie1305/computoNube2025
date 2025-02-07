# Practica 1: Base de Datos, colecciones e inserts

1. Conectarnos con mongosh a MongoDB
1. Crear una base de dato llamada Curso
1. Comprobar que la base de datos no exista
1. Crear una coleccion que se llame facturas y mostrarla

``` json
db.createCollection("Facturas")
show collections
```
5. Insertar un documento con los siguientes datos

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 10 |
| Ciente | Frutas Ramirez |
| Total | 223 |

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 20 |
| Ciente | Ferreteria Juan |
| Total | 140 |

```json
 db.Facturas.insertOne(
    {
        cod_facturas : 10,
        cliente : "Frutas Ramirez",
        total: 223 
    })
```
```json
db.Facturas.insertOne(
    {
        cod_facturas : 20,
        cliente : "Ferreteria Juan",
        total: 140 
    })
```
6. Crear una nueva coleccion pero usando directamente el insertOne
insertar un documento en la coleccion productos con los siguentes datos: 

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 1 |
| Nombre | Tornillo x 1 |
| Precio | 2 |

```json
db.productos.insertOne(
    {
        cod_producto : 1,
        cliente : "Tornillo x 1",
        precip: 2,
        unidades : 1500
    }
)
```

```json
db.productos.insertOne(
    {
        _id : '67a4fc3bb0415e7b0ccb0ce4',
        cliente : 1,
        nombre: "Tornillo x 1",
        precio: 2,

    }
)
```
7. Crear un nuevo documento de producto que contenga un array. Con los siguentes datos:

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 2 |
| Nombre | Martillo |
| Precio | 20 |
| Unidades | 50 |
| Fabricantes | fab1, fab2, fab3, fab4 |

``` json
db.productos.insertOne(
    {
        cod_producto : 2,
        cliente : "Martillo",
        precio: 20,
        unidades : 50,
        fabricantes : [
             "fab1, fab2, fab3, fab4"
        ]
    }
)
```
8. Borrar la colecion Facturas y comprobar que se borro 

``` json
db.Facturas.drop()
```

```json
show collections
```

9. Insertar un documento en una coleccion denominada **Fabricantes** para probar lo subdocumentos y la clave _id personalizada

| Codigo   | Valor   |
|-------------|-------------|
| id | 1 |
| Nombre | Fab 1 |
| Localidad | ciudad: Buenos Aires, pais: Argentina, calle: Calle Pez 27, cod_postal: 29000 |

```json
db.fabricantes.insertOne(
    {
        _id : 1,
        nombre : "Fab 1",
        localidad : {
            ciudad: "Buenos Aires",
            pais: "Argentina",
            calle: "Calle Pez 27",
            cod_postal: 29000
        }
    }
)
```

10. Realizar una insercion e varion documento en la coleccion productos

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 3 |
| Nombre | Alicates |
| Precio | 10 |
| Unidades | 20 |
| Fabricantes | fab1, fab2, fab5 |

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 4 |
| Nombre | Arandela |
| Precio | 1 |
| Unidades | 500 |
| Fabricantes | fab2, fab3, fab4 |

db.productos.insertOne(
    {
        cod_producto : 3,
        cliente : "Alicates",
        precio: 10,
        unidades : 20,
        fabricantes : [
             "fab1, fab2, fab5"
        ]
    }
)
db.productos.insertOne(
    {
        cod_producto : 4,
        cliente : "Arandela",
        precio: 1,
        unidades : 500,
        fabricantes : [
             "fab2, fab3, fab4"
        ]

    }
)