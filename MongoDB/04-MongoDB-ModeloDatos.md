## Modelo de Datos en MongoDB

- Modelos
    -Embebidos
    -Referenciados 

- Modelos Emebebidos

**Uno a Uno**

```json
db.departamentos.insertMany(
[
    {
    _id:1,
    nombre: "Tecnologías de Información",
    responsable:{
        nombre: "Monico",
        apellidos: "Martinez Perez"
    },
    descripcion: "ejemplo de departamento"
    },
    {
    _id:2,
    nombre: "Contabilidad",
    responsable:{
        nombre: "Raul",
        apellidos: "Lopez Hernandez"
    },
    descripcion: "ejemplo de departamento"
    }
])
```

- Modelos Referenciales

**Uno a Uno**

```json
db.localidades.insertMany(
[
    {
    _id:"BA",
    ciudad: "Buenos Aires",
    paises:"Argentina",
    poblacion: "16 millones",
    turismo: ["edificios","tango","gatronomia","museos","parques"],
    direccion: "Avenida Tortolos",
    cod_departamento: 1
    },
    {
    _id:"SA",
    ciudad: "Santiago",
    paises:"Chile",
    poblacion: "20 millones",
    turismo: ["iglesias","vino","gatronomia","museos"],
    direccion: "Calle soyla Vaca del Corral",
    cod_departamento: 2
    }
    
])
```

```json
db.departamentos.aggregate(
[
    {
        $lookup:{
            from:"localidades",
            localField:"_id",
            foreignField:"cod_departamento",
            as: "localidades"
        }
    }
]
)
```

```json
[
  {
    $lookup:
      {
        from: "departamentos",
        localField: "_id",
        foreignField: "cod_departamentos",
        as: "localidades"
      }
  },
  {
    $project:
      {
        _id: 0,
        nombre: 1,
        "localidades.ciudad": 1
      }
  }
]
```