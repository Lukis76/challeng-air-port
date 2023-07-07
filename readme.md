# Postulacion-bsale

## Check-In Aerolinea

Este proyecto es una simulación de check-in automático para una aerolínea, que asigna asientos a los pasajeros basándose en ciertas reglas y restricciones. La solución se implementa como una API REST y se conecta a una base de datos MySQL para obtener los datos necesarios.

### Tecnologias que se Usaron

- Javascript
- NodeJs
- ExpressJs
- Sequelize
- mysql2

### Reglas

La solución del desafío debe cumplir con las siguientes reglas:

1. Los asientos deben ser asignados secuencialmente, comenzando desde el asiento número 1.
2. Los pasajeros adultos (mayores de 18 años) deben ser asignados en función de su tipo de asiento:

   1. Orden por clases de Asiento: Los pasajeros deben ser asignados a asientos segun su clase en orden secuencial.

3. Los pasajeros menores de edad (menores de 18 años) deben ser asignados a asientos contiguos a sus acompañantes adultos, según las siguientes condiciones:

   1. Si el asiento de un acompañante adulto está ocupado, se asigna al menor de edad el siguiente asiento disponible contiguo a su acompañante en la misma clase.

   2. Si no hay asientos disponibles contiguos a su acompañante en la misma clase, se asigna al menor de edad el siguiente asiento disponible en la misma clase.</

4. Si un pasajero tiene varias tarjetas de embarque, tratar de que queden juntas o cerca en los asientos.
5. Los campos en la bd están llamados en Snake case, pero en la respuesta de la API deben ser transformados a Camel case.
6. El servicio debe tener la siguiente estructura:

```bash
GET /flights/:id/passengers
```

7. Respuesta Exitosa:

```json
{
    "code": 200,
    "data": {
        "flightId": 1,
        "takeoffDateTime": 1688207580,
        "takeoffAirport": "Aeropuerto Internacional Arturo Merino Benitez, Chile",
        "landingDateTime": 1688221980,
        "landingAirport": "Aeropuerto Internacional Jorge Cháve, Perú",
        "airplaneId": 1,
        "passengers": [
            {
                "passengerId": 98,
                "dni": 172426876,
                "name": "Abril",
                "age": 28,
                "country": "Chile",
                "boardingPassId": 496,
                "purchaseId": 3,
                "seatTypeId": 3,
                "seatId": 15
            },
            {...}
        ]
    }
}
```

8. Vuelo no encontrado:

```json
{
  "code": 404,
  "data": {}
}
```

9. Caso de Error:

```json
{
  "code": 400,
  "errors": "could not connect to db"
}
```

<br>

## Como Inicializar la API

Esta API proporciona un servicio para simular el check-in de pasajeros de una aerolínea. Permite consultar la simulación de check-in para un vuelo específico mediante el uso de un endpoint.

### ENDPOINT

```bash
GET /flights/:id/passengers
```

Donde :id representa el ID del vuelo para el cual se desea obtener la simulación del check-in.

## Uso de la API

Realiza una solicitud GET al endpoint de la API, especificando el ID del vuelo en la URL. Por ejemplo:

```bash
https://postulacion-bsale-test.up.railway.app/flight/1/passengers
```

La API responderá con la simulación del check-in para el vuelo solicitado en formato JSON. La respuesta seguirá la siguiente estructura:

```json
{
  "code": 200,
  "data": {
    "flightId": 1,
    "takeoffDateTime": 1688207580,
    "takeoffAirport": "Aeropuerto Internacional Arturo Merino Benitez, Chile",
    "landingDateTime": 1688221980,
    "landingAirport": "Aeropuerto Internacional Jorge Cháve, Perú",
    "airplaneId": 1,
    "passengers": [
      {
        "passengerId": 90,
        "dni": 983834822,
        "name": "Marisol",
        "age": 44,
        "country": "México",
        "boardingPassId": 24,
        "purchaseId": 47,
        "seatTypeId": 1,
        "seatId": 1
      },
      ...
    ]
  }
}
```

Analiza la respuesta de la API para obtener la información deseada. En este caso, la simulación del check-in contiene detalles del vuelo, como la fecha y hora de salida, aeropuertos de salida y llegada, ID del avión, y la lista de pasajeros con sus respectivos detalles, incluyendo ID del pasajero, DNI, nombre, edad, país, ID de la tarjeta de embarque, ID de la compra, tipo de asiento y número de asiento asignado.

Utiliza los datos de la simulación del check-in según sea necesario para realizar las acciones correspondientes.

## Consideraciones

### Asegúrate de reemplazar :id en la URL del endpoint con el ID real del vuelo que deseas consultar.

## Explicacion

Se intalan dependencias necesarias para configurar los archivos

```bash
npm i express sequelize mysql2 morgan cookie-parses body-parser cors </code>
```

- Se configuranron los archivos que son los puntos de entrada para que el servidor funciones de forma optima, tales como "index.js" el cual va a iniciar el servidor y quedarse escuchando la peticiones que se hagan, el archivo "app.js" donde configure el servidor de express, los middleware, rutas y manejo de errores.

- Configuro el servidor de express en App.js

- Creo las carpetas y archivos necesarios para el uso de la app: Modelos, Rutas, Controlador

- Configuro la base de datos con sequelize, configuro la conexion de la misma y verifico su correcto funcionamiento, hago las relaciones de los modelos

- Configuro y verifico que el servidor este escuchando correctamente los llamados

- En la carpetas de rutas declaro una ruta '/' de prueba para probar de forma local su funcionamiento y luego la ruta que sera utilizada para el check.in

- Comienzo a trabajar en los controladores y creo un carpeta auxiliar para modularizar todo y de hay extraer funciones que me ayuden en el codigo

- En el archivo controller.js realizo una ruta controladora para las consultas a la base de datos y asi manejar los datos recibidos y entregar la respuesta solicitadas.

- En este archivo tengo importados los archivos de la base de datos los cuales manejare para las consultas y el archivo seatAssing que me prestara funciones que se encargaran de los requerimiento

- Como la ruta se maneja por los id que contenga la tabla Flight, hago un busqueda por parametros donde pueda extraer el ID solicitado, una vez solicitado incluyo los modelos necesarios para traer los datos solicitados y un poco mas en caso de que quiera modelar otra ruta o generar distintas logicas para el check-in

- Luego hacer la consulta a Flight y obtener los datos que necesito hago las condicional requerida en caso de que el id no exista, luego llamo a la funcion que asignara los asientos y la paso los parametros requeridos, para finalizar envio la data con la estructura solicitada incluyendo la respuesta en caso de error

- En el archivo de seatAssing es donde esta toda la logica para el manejo de los asientos la cual contiene un funcion llamada de la misma manera que tiene dos parametros de los cuales se sacaran los datos para asignar los asientos

- Comezamos creando un variable con 3 claves que son 3 array vacios en los cuales vamos a introducir los pasajeros segun su tipo de tarjeta de embarque

- Luego cree dos variables donde filtre los pasajeros segun su edad (Adultos y menores de edad)

- Inicie un variable que me funcionara como contador y representara el proximo asiento disponible

- Recorro las variables que tiene los pasajeros segun su edad y voy asignandole los asientos utilizando una funcion auxiliar y creo un objeto con la data que debo enviar, agrego el pasajero en el array segun su tipo de asiento y continua en al siguiente incrementado el contador

- Se crea una variable Passenger que es un array con la data que se recolecto, y se retorna

- La función getSeatType se encarga de retornar el tipo de asiento correspondiente a partir del ID del tipo de asiento.

- La función findNextSeat busca el siguiente número de asiento disponible en un array de asientos asignados. Ordena los asientos de forma ascendente y devuelve el primer número de asiento que no esté ocupado.
