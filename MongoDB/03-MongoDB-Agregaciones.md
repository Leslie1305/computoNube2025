# Agregaciones en MongoDB (Frmework)

## Métodos para realizar agregaciones simples 

- distinc(): Delvueve valores no duplicados 
- countDocuments(): Cuenta los documentos en una collección
estimatedDocumentCount(): Cuenta de manera estimada durante un periodo de tiempo 

## Una Aggregation pipeline consta de una o mas estapas (stage) que procesan documentos

1. Cada etapa realiza un operacion en los documentos de entrada. Por ejemplo, una fase puede filtrar documentos, agrupar documentos y calcular valores. 

2. Los documentos que se generan en una fase pasan a la siguentes fase. 

3. Puede devolver resultados para grupos de doumentos como totales, máximos, minimo, etc.

### Se utiliza la causa "aggregate"

- Existen una serie de operadores que se pueden utilizar para realizar operaciones. Se tienen dististos tipos: estapa, de comparación, booleanos, aritmeticos, de cadena, etc.

## Metodos Simples: countDocucments y Distinct 

1. Contar los documentos de la colección libros

```json
db.libros.countDocuments()
```

2. Contar los documentos de la editorial Terra

```json
db.libros.countDocuments({
    editorial: {$eq: "Terra"}
})
```

```json
db.libros.countDocuments({
    editorial:"Terra"
})
```

3. Mostrar todos los libros mostrando solamente la editorial

```json
db.libros.find({}, {_id:0, editorial:1})
```

4. Mostrar todas las distintas editoriales

```json
db.libros.distinct("editorial")
```

[Documentación de Agregaciones](https://www.mongodb.com/docs/manual/aggregation/)

## $match. Una pipeline básica
## tienen dunciones de estapa

```json
db.libros.aggregate([
    {$match:{editorial:"Terra"}}
])
```

## $project. Incluir y renombrar campos

```json
db.libros.aggregate(
    [
        {
        $match:{editorial:"Terra"}
        },
        {
            $project:{
                _id:0,
                titulo:1,
                precio:1,
                NombreEditorial:"$editoral",
                editorial:1
            }
        }
    ]
)
```

- Generado por Mongo Compass

```json
[
  {
    $match:
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      {
        _id: 0,
        precio: 1,
        cantidad: 1,
        NombreEditorial: "$editorial",
        editorial: 1
      }
  }
]
```

```json
[
  {
    $match:
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  }
]
```

## $sort. Ordenaciones 

```json
[
  {
    $match:
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      {
        precio: 1
      }
  }
]
```

## $group. Agruapaciones

[Agrupaciones](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/)

- Cuantos documentos hay por cada una de las editoriales

```json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero Documentos": {
          $count: {}
        }
      }
  }
]
```
- Cuantos documentos hay por cada una de las editoriales por numero de documentos de manera descendente 

```json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero Documentos": {
          $count: {}
        }
      }
  },
  {
    $sort:
      {
        editorial: 1
      }
  }
]
```

- Utilizando Mongo Atlas Base de datos sample_airbnb
- Agrupar por tipo de propiedad, mostrando el numero de propiedades y el promedio de sus precios

```json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set: {
      Media_Total: {
        $trunc: "$Media"
      }
    }
  }
]
```

- Operador $unset

```json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set: {
      Media_Total: {
        $trunc: "$Media"
      }
    }
  },
  {
    $unset:
      ["Media", "Media_Total"]
  }
]
```

- Quitando solo un campo

```json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set: {
      Media_Total: {
        $trunc: "$Media"
      }
    }
  },
  {
    $unset:
      "Media"
  }
]
```

- Creando nuevas colecciones utilizando el operador $out
- Nota: Debe ser el ultimo en la agregacion

```json
[
  ¿
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      },
  {
    $set:
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      "Media"
  },
  {
    $out:
      {
        db: "sample_airbnb",
        coll: "media_propiedades"
      }
  }
]
```

- Ejemplos con operadores de comparación y lógicos 

```json
[
  {
    $project:
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $gt: ["$price", 300]
        },
        medio: {
          $and: [
            {
              $gte: ["$price", 100]
            },
            {
              $lt: ["$price", 300]
            }
          ]
        },
        baratito: {
          $lte: ["$price", 100]
        }
      }
  }
]
```

- $cound. Devuelve valores según una condición (es parecido a un operador ternario de un leguaje de programación)

```json
[
  {
    $project: {
      _id: 0,
      price: 1,
      name: 1,
      room_type: 1,
      caro: {
        $cond: [
          {
            $gt: ["$price", 300]
          },
          "Si",
          "No"
        ]
      },
      medio: {
        $cond: [
          {
            $and: [
              {
                $gte: ["$price", 100]
              },
              {
                $lt: ["$price", 300]
              }
            ]
          },
          "Si",
          "No"
        ]
      },
      baratito: {
        $cond: [
          {
            $lte: ["$price", 100]
          },
          "Si",
          "No"
        ]
      }
    }
  }
]
```

- Views   

db.createView()

```json
db.createView("Ganancias_libros",
"libros",
[
  {
    $match:
      {
        editorial: "Biblio"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre de Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      {
        "Total de Ganancias": -1
      }
  }
]
)
```

```json
db["Ganancias_libros"].find(
{"Total de Ganancias":{$lte:240}},
{titulo:1, "Total de Ganancias":1}).sort(
{titulo:-1})
```