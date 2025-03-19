# Arrays y documentos anidados

1. Para hacer esta práctica vamos a cargar unos datos ficticios de empresas.
2. Tienes un fichero denominado “personas.json”
3. Debes poner el resultado de las consultas en cada apartado

- Buscar las personas que vivan en Colombia

```json
{
  "direccion.pais": "Colombia"
}
```

[Resultados](./Imagenes-Practica04/colombia.png)

- Personas a las que le guste el color rosa

```json
{
  "colores": "pink"
}
```

[Resultados](./Imagenes-Practica04/color-pink.png)

- Personas cuyo tercer color preferido sea el amarillo (yellow). Recuerda que los arrays comienzan por Cero. Deben salir 3

```json
{
  "colores.2": "yellow"
}
```

[Resultados](./Imagenes-Practica04/color.2-yellow.png)

- Personas cuyo tercer color preferido sea el amarillo (yellow) y que vivan en Mauritania (debe salir uno)

```json
{
  "colores.2": "yellow",
  "direccion.pais": "Mauritania"
}
```

[Resultados](./Imagenes-Practica04/yellow-Mauritania.png)

- Usando el operador $all averiguar las personas que les gusta el plata (silver) y el salmon (salmon)

```json
{
  "colores": 
  { "$all": 
  ["silver", "salmon"] }
}
```

[Resultados](./Imagenes-Practica04/silver-salomon.png)

- Con el operador $elemMatch, averigua las personas que les guste el rosa (Pink) o el rojo (red)

```json
{
  "colores": 
  { "$elemMatch": 
  { "$in": ["pink", "red"] } 
  }
}
```

[Resultados](./Imagenes-Practica04/pink-red.png)