# Laboratorio MongoDB

## Introducción

En este base de datos puedes encontrar un montón de alojamientos y sus reviews, esto está sacado de hacer webscrapping.

**Pregunta**. Si montaras un sitio real, ¿Qué posibles problemas pontenciales les ves a como está almacenada la información?

```md
Indica aquí que problemas ves
```

## Obligatorio

Esta es la parte mínima que tendrás que entregar para superar este laboratorio.

### Consultas

- Saca en una consulta cuantos alojamientos hay en España.

```js
db.listingsAndReviews
    .countDocuments({"address.country": "Spain"},
)
```

- Lista los 10 primeros:
  - Ordenados por precio de forma ascendente.
  - Sólo muestra: nombre, precio, camas y la localidad (`address.market`).

```js
db.listingsAndReviews
.find({"address.country": "Spain"},
      {_id:0, "nombre":"$name", "precio":"$price","camas":"$beds", "localidad": "$address.market" })
.limit(10)
.sort({"price":1})
```

### Filtrando

- Queremos viajar cómodos, somos 4 personas y queremos:
  - 4 camas.
  - Dos cuartos de baño o más.
  - Sólo muestra: nombre, precio, camas y baños.

```js
db.listingsAndReviews
.find({beds: 4, bathrooms:{$gt:2}},
      {_id:0, "nombre":"$name", "precio":"$price","camas":"$beds", "baños": "$bathrooms" })
```

- Aunque estamos de viaje no queremos estar desconectados, así que necesitamos que el alojamiento también tenga conexión wifi. A los requisitos anteriores, hay que añadir que el alojamiento tenga wifi.
  - Sólo muestra: nombre, precio, camas, baños y servicios (`amenities`).

```js
db.listingsAndReviews
.find({beds: 4, bathrooms:{$gt:2}, amenities:{$all:["Wifi"]}},
      {_id:0, "nombre":"$name", "precio":"$price","camas":"$beds", "baños": "$bathrooms", "servicios":"$amenities" })
```

- Y bueno, un amigo trae a su perro, así que tenemos que buscar alojamientos que permitan mascota (_Pets allowed_).
  - Sólo muestra: nombre, precio, camas, baños y servicios (`amenities`).

```js
db.listingsAndReviews
.find({beds: 4, bathrooms:{$gt:2}, amenities:{$all:["Wifi","Pets allowed"]}},
      {_id:0, "nombre":"$name", "precio":"$price","camas":"$beds", "baños": "$bathrooms", "servicios":"$amenities" })
```

### Operadores lógicos

- Estamos entre ir a Barcelona o a Portugal, los dos destinos nos valen. Pero queremos que el precio nos salga baratito (50 $), y que tenga buen rating de reviews (igual o superior a 88).
  - Sólo muestra: nombre, precio, camas, baños, rating y localidad.

```js
db.listingsAndReviews
.find({$or: [{"address.country":"Portugal"},{"address.market":"Barcelona"}],
      price:50,
      "review_scores.review_scores_rating": {$gte:88}},
    {_id:0, "nombre":"$name", "precio":"$price","camas":"$beds", "baños": "$bathrooms", "rating":"$review_scores.review_scores_rating","localidad": "$address.market"})
```

- También queremos que el huésped sea un superhost y que no tengamos que pagar depósito de seguridad
  - Sólo muestra: nombre, precio, camas, baños, rating, si el huésped es superhost, depósito de seguridad y localidad.

```js
db.listingsAndReviews
.find({$or: [{"address.country":"Portugal"},{"address.market":"Barcelona"}],
      price:50, 
      "review_scores.review_scores_rating": {$gte:88}, 
      "host.host_is_superhost":true,
       security_deposit:0},
    {_id:0, "nombre":"$name", "precio":"$price","camas":"$beds", "baños": "$bathrooms", "rating":"$review_scores.review_scores_rating","superhost":"$host.host_is_superhost","Depósito de seguridad":"$security_deposit","localidad": "$address.market"})
```

### Agregaciones

- Queremos mostrar los alojamientos que hay en España, con los siguientes campos:
  - Nombre.
  - Localidad (no queremos mostrar un objeto, sólo el string con la localidad).
  - Precio

```js
// Pega aquí tu consulta
```

- Queremos saber cuantos alojamientos hay disponibles por pais.

```js
// Pega aquí tu consulta
```

## Opcional

- Queremos saber el precio medio de alquiler de airbnb en España.

```js
// Pega aquí tu consulta
```

- ¿Y si quisieramos hacer como el anterior, pero sacarlo por paises?

```js
// Pega aquí tu consulta
```

- Repite los mismos pasos pero agrupando también por numero de habitaciones.

```js
// Pega aquí tu consulta
```

## Desafio

Queremos mostrar el top 5 de alojamientos más caros en España, con los siguentes campos:

- Nombre.
- Precio.
- Número de habitaciones
- Número de camas
- Número de baños
- Ciudad.
- Servicios, pero en vez de un array, un string con todos los servicios incluidos.

```js
// Pega aquí tu consulta
```
