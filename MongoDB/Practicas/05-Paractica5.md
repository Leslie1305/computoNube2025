# Agregaciones

1. Para hacer esta práctica vamos a cargar unos datos ficticios de empresas.
2. Tienes un fichero denominado “productos.json”
3. Debes poner el resultado de las consultas en cada apartado

- Cuenta los productos de tipo “medio”, usando un método básico

```json
[
  {
    $match: {
      tipo: "medio"
    }
  },
  {
    $count: "total"
  }
]
```

[Resultados](./Imagenes-Practica05/total.png)

- Indicar con un distinct, las empresas (fabricantes) que hay en la colección

```json
db.productos.distinct("fabricante")
```

[Resultados](./Imagenes-Practica05/distinct.png)

- Usando aggregate, visualizar los productos que tengan más de 80 unidades

```json
db.productos.aggregate([
  { 
    $match: { 
        unidades: { $gt: 80 } } 
        }
])

```

[Resultados](./Imagenes-Practica05/aggregate.png)

- Con $project visualizar solo el nombre, unidades y precio de los productos que tengan menos de 10 unidades

```json
db.productos.aggregate([
  { 
    $match: { 
        unidades: { $lt: 10 } } 
        },
  { 
    $project: { 
        nombre: 1, 
        unidades: 1, 
        precio: 1 } 
    }
])

```

[Resultados](./Imagenes-Practica05/visualizar.png)

- Con $project ponemos el fabricante pero le cambiamos el nombre por “empresa”. Usamos el mismo comando anterior

```json
db.productos.aggregate([
  { 
    $match: { unidades: { $lt: 10 } } 
    },
  { $project: { 
    nombre: 1, 
    unidades: 1, 
    precio: 1, 
    empresa: "$fabricante" } }
])
```

[Resultados](./Imagenes-Practica05/cambio-empresa.png)

- Añadir a la consulta anterior un campo calculado que se llame total y que multiplique precio por unidades.

```json
db.productos.aggregate([
  { 
    $match: { unidades: { $lt: 10 } } 
    },
  { 
    $project: { 
        nombre: 1, 
        unidades: 1, 
        precio: 1, 
        empresa: "$fabricante", 
        total: { $multiply: ["$precio", "$unidades"] } } 
    }
])
```

[Resultados](./Imagenes-Practica05/campo-calculado.png)

- Hacer que el nombre salga en mayúsculas con el operador $toUpper

```json
db.productos.aggregate([
  { 
    $match: { unidades: { $lt: 10 } } 
    },
  { 
    $project: { 
        nombre: { $toUpper: "$nombre" }, 
        unidades: 1, 
        precio: 1, 
        empresa: "$fabricante" } 
        }
])
```

[Resultados](./Imagenes-Practica05/toUpper.png)

- Añadir un campo calculado que ponga el nombre del producto y el tipo concatenado con el operador $concat. Le llamamos al campo “completo”

```json
db.productos.aggregate([
  { 
    $match: { 
        unidades: { $lt: 10 } } 
        },
  { 
    $project: { 
        nombre: 1, 
        unidades: 1, 
        precio: 1, 
        empresa: "$fabricante", 
        completo: { 
            $concat: ["$nombre", " - ", "$tipo"] } 
            } 
            }
])
```

[Resultados](./Imagenes-Practica05/completo.png)

- Ordena el resultado por el campo “total”

```json
db.productos.aggregate([
  { 
    $match: { 
        unidades: { $lt: 10 } } 
        },
  { 
    $project: { 
        nombre: 1, 
        unidades: 1, 
        precio: 1, 
        empresa: "$fabricante", 
        total: { $multiply: ["$precio", "$unidades"] } 
        }},
  { 
    $sort: { 
        total: 1 } 
        }
])
```

[Resultados](./Imagenes-Practica05/ordenar.png)

- Haciendo una nueva consulta, averiguar el numero de productos por tipo de producto

```json
db.productos.aggregate([
  { 
    $group: { 
        _id: "$tipo", 
        total: { $sum: 1 } } 
        }
])
```

[Resultados](./Imagenes-Practica05/nuevaConsulta.png)

- Añadir el valor mayor y el menor

```json
db.productos.aggregate([
  { 
    $group: { 
        _id: "$tipo", 
        total: { $sum: 1 }, 
        max_price: { $max: "$precio" }, 
        min_price: { $min: "$precio" } }
        }
])
```

[Resultados](./Imagenes-Practica05/mayor-menor.png)

- Añade el total de unidades por cada tipo

```json
db.productos.aggregate([
  { 
    $group: { 
        _id: "$tipo", 
        total: { $sum: 1 }, 
        total_unidades: { $sum: "$unidades" } } 
        }
])
```

[Resultados](./Imagenes-Practica05/cadaTipo.png)

- Con el operador $set y el operador “$substr” visualiza todos los datos del producto "Small Metal Tuna" y los primeros 5 caracteres del nombre.

```json
db.productos.aggregate([
  { 
    $match: { 
        nombre: "Small Metal Tuna" } 
        },
  { 
    $set: { 
        nombre_corto: { 
            $substr: ["$nombre", 0, 5] } } 
            }
])
```

[Resultados](./Imagenes-Practica05/primeros5.png)

- Creamos una salida que tenga el nombre del articulo y el total (precio por unidades) y lo guardamos en una colección denominada productos2

```json
db.productos.aggregate([
  { 
    $project: { 
        nombre: 1, 
        total: { 
            $multiply: ["$precio", "$unidades"] } 
            }},
  { 
    $out: "productos2" }
])
```

- Comprobamos que se ha creado

```json
db.productos2.find()
```

- Hacemos un find para comprobar el resultado

```json
db.productos2.find()
```

[Resultados de las dos anteriores consultas y esta](./Imagenes-Practica05/productos2.png)

- Usando $cond y $project vamos a visualizar el nombre del producto, el precio y un campo llamado valoración que ponga “barato” si el precio es menor de 250 y caro si es mayor o igual

```json
db.productos.aggregate([
  { $project: { 
    nombre: 1, 
    precio: 1, 
    valoracion: { 
      $cond: { 
        if: { $lt: ["$precio", 250] }, 
        then: "barato", 
        else: "caro" 
      } 
    } 
  } 
  }
])
```

[Resultados](./Imagenes-Practica05/barato.png)