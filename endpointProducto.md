
# GET

## GET api/v1/productos/

```json
[
    {
        "id": 1,
        "nombre": "Morral",
        "descripcion": "Morral tejido a mano",
        "precio": 9990,
        "categoria": {
            "id" : 1,
            "nombre": "Accesorio"
        },
        "stock": 10
    },
    {
        "id": 2,
        "nombre": "Telar",
        "descripcion": "Telar de 50 x 50 cm",
        "precio": 19990,
        "categoria": {
            "id" : 1,
            "nombre": "Adorno"
        },
        "stock": 10
    }
]
```

## GET api/v1/productos/{id}

```json
{
    "id": 2,
    "nombre": "Telar",
    "descripcion": "Telar de 50 x 50 cm",
    "precio": 19990,
    "categoria": {
        "id" : 1,
        "nombre": "Adorno"
    },
    "stock": 10
}
```

## POST api/v1/productos/

```json
{
    "nombre": "Telar de Madera",
    "descripcion": "Telar de 150 x 150 cm",
    "precio": 199900,
    "categoria": {
        "id": 1,
        "nombre": "Categoria"
    },
    "stock": 10
}
```

## PUT api/v1/productos/{id}

```json
{
    "nombre": "Telar de Madera",
    "descripcion": "Telar de 150 x 150 cm",
    "precio": 199900,
    "categoriaId": 1,
    "stock": 10
}
```

## DELETE api/v1/productos/{id}

