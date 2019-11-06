# ¿Como crear un servidor en Node JS?

En este **README** voy a explicar cómo crear un servidor web en Node JS usando la librería **Express**. Posteriormente vamos a conectar nuestra **API REST** con una base de datos en Mongo por medio de la librería **mongoose.**

## Para empezar

Necesitamos descargar la última versión LTS de Node JS, actualmente esta versión es la 12.
Para descargar entramos al siguiente link: [https://nodejs.org/es/](https://nodejs.org/es/).

Una vez descargado procedemos a instalarlo. Cuando esté instalado abrimos una terminal y escribimos:

```bash
$ node --version
$ npm --version
```

Deberá saltarnos en la terminal la versión de node que acabamos de instalar, teniendo esto estamos listos para continuar con la creación de nuestro servidor.

Vamos a crear una carpeta llamada **node_server** donde vamos a tener todos los archivos referentes a nuestro servidor.

```bash
$ mkdir node_server
$ cd node_server
```

Dentro de la carpeta vamos a iniciar un proyecto de **NPM** para que podamos descargar y posteriormente utilizar dependencias externas como son Express y Mongoose.

```bash
$ npm init -y
```

Esto nos va a devolver en la terminal un JSON con la siguiente información:

```json
{
    "name": "node_server",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"
}
```

Ahora vemos que se genera un archivo **package.json** dentro de nuestra carpeta, este archivo contiene toda la información referente a nuestro proyecto para que pueda posteriormente ser utilizado por otr@s desarrollador@s.

Finalmente creamos dentro de la carpeta un archivo **server.js** que es el archivo donde vamos a escribir nuestro servidor de Node JS.

La carpeta quedaría de la siguiente forma:

```bash
/node_server
├── server.js
└── package.json
```

## Primeros pasos

Suponiendo que la terminal se encuentra donde la carpeta **node_server**, vamos a correr el siguiente comando para instalar las dependencias:

```bash
$ npm install --save express mongoose
```

Esto nos va a generar una carpeta llamada **node_modules** dentro de nuestro proyecto **node_server**, dentro de esta carpeta se guardan las dependencias **express** y **mongoose** que acabamos de descargar.

Vamos ahora a abrir nuestro proyecto con el editor de código de nuestra preferencia y vamos a escribir lo siguiente:

**server.js**

```javascript
const express = require("express") // traemos la dependencia express
const app = express() // iniciamos el objeto del servidor

// cuando el servidor esté listo, ejecutamos una función
app.listen(8080, function() {
    console.log("Servidor escuchando :)") // enviamos un mensaje a la consola
})
```

Es necesario correr nuestro servidor para probar que funciona, vamos a la terminal y suponiendo que estamos ubicados en la carpeta **node_server** vamos a ejecutar el siguiente comando:

```bash
$ node server.js
```

Al poco tiempo tendremos que ver en la consola el mensaje:

```bash
$ node server.js
Servidor escuchando :)
```

Si no salió como debería asegúrate de estar en la carpeta **node_server** y de que no tengas algún erro de sintaxis en tu código.

Una mejor forma de correr nuestro código de Node JS es por medio de la herramienta **nodemon** que automáticamente va correr de nuevo el archivo que le indiquemos cada que encuentre cambios en nuestro proyecto.

Si no la tienes instalada tendrás que correr el siguiente comando:

**windows / linux**

```bash
$ npm install -g nodemon
```

**mac**

```bash
$ sudo npm install -g nodemon
```

(-g significa "global" e indica que esta dependencia va a instalarse globalmente en la computadora, puede que si te encuentras en una macbook tengas que utilizar **sudo** por una cuestión con los permisos)

Para probar si la dependencia esta finalmente instalada usaremos el siguiente comando:

```bash
$ nodemon --version
```

Esto nos arrojará la versión instalada de **nodemon**, de salir todo bien ahora vamos a ejecutar nuestros archivos utilizando esta herramienta, y de la siguiente forma:

```bash
$ nodemon server.js
```

## REST

REST es una arquitectura que corre sobre el protocolo HTTP, sugiere una mejor organización de cada uno de los endpoints dentro de nuestra aplicación.

Una de las características de REST es que no guarda ningún "estado" en memoria que refiera e identifique a cada uno de sus clientes, esto significa que el servidor desconoce a quienes lo utilizan hasta el momento de la petición.

El protocolo entre otras cosas dicta que para cada entidad en nuestra lógica existe una URL con su nombre. Por ejemplo si dentro de nuestra lógica tenemos las siguientes entidades:

-   Usuario
-   Canción
-   Playlist
-   Género

Significa que vamos a tener que recibir información de las siguientes URL:

-   /user
-   /song
-   /playlist
-   /genere

Idealmente cada una de estas entidades va a pasar por ciertos procesos a lo largo de nuestra lógica, y para cada uno de estos procesos HTTP sugiere ciertos métodos para cada petición:

-   **Consultar** (GET)
-   **Crear** (POST)
-   **Modificar** (PUT)
-   **Eliminar** (DELETE)

Ahora vamos a representar esto en código con la entidad User:

```javascript
const express = require("express") // traemos la dependencia express
const app = express() // iniciamos el objeto del servidor

app.get("/user") // obtener todos o un solo usuario
app.post("/user") // crear un nuevo usuario
app.put("/user") // modificar a un usuario
app.delete("/user") // eliminar un usuario

// cuando el servidor esté listo, ejecutamos una función
app.listen(8080, function() {
    console.log("Servidor escuchando :)") // enviamos un mensaje a la consola
})
```

A esto se le conoce como endpoints, y representan las acciones finales que un cliente puede realizar con nuestro servidor.

Cada uno de estos endpoints deben recibir una petición y enviar una respuesta, por lo tanto dos objetos **request** y **response** pueden ser manejados dentro de cada endpoint de la siguiente forma:

```javascript
app.get("/user", function(request, response) {
    // request: objeto con la petición
    // response: objeto para manejar la respuesta
})
```

En este caso estamos indicando qué es lo que va a ocurrir cuando un cliente haga una petición al endpoint **/user** por medio del metodo **GET**, será necesario declarar cada uno de los métodos y endpoints posteriormente pero por el momento vamos a trabajar únicamente con este endpoint.

Si nuestro servidor está corriendo con **nodemon** no será necesario pararlo y volverlo a correr, vamos a abrir una nueva ventana de navegador y entraremos a la siguiente URL:

    http://localhost:8080/user

**http** es el protocolo de nuestro servidor.
**localhost** se refiere a la máquina sobre la que está corriendo actualmente el servidor.
**8080** es el puerto donde le indicamos al servidor que escuchara las peticiones.
**/user** es el endpoint que acabamos de crear.
**GET** es el método por el cual se hacen todas las peticiones desde la barra de un navegador.

Por lo tanto podemos estar seguros de que al entrar a http://localhost:8080/user estamos entrando al endpoint que representamos arriba en código, sin embargo nuestro navegador se va a quedar esperando para siempre sin que suceda nada.

Esto es porque HTTP dicta que para cada petición debe de existir una respuesta. Y actualmente nuestro endpoint no hace ni responde absolutamente nada. Vamos a hacer que cuando un usuario le pegue al endpoint **/user** con el método **GET** respondamos un texto simple:

```javascript
// podemos acortar el nombre de los objetos *request* y *response*
// ya que su valor no depende de su nombre,
// si no de su posición en los argumentos
app.get("/user", function(req, res) {
    // res es un objeto, y send() uno de sus métodos con el que podemos enviar
    // texto plano de vuelta al cliente
    res.send("saludos cordiales")
})
```

Si ahora volvemos a ejecutar la URL http://localhost:8080/user vemos que nos envía de regreso el texto plano "saludos cordiales". Sin embargo esto no es muy útil ya que el texto plano difícilmente podría representar una estructura de datos más compleja como un **objeto o un arreglo**.

Javascript es una tecnología que usamos tanto del lado del **frontend** como del lado del **backend**, es ideal poder tener la misma notación para la información que enviamos entre un cliente y un servidor.

Aquí es donde entra JSON, que significa "JavaScript Object Notation" y nos permite representar en texto plano a estructuras complejas dentro del lenguaje, como son los tipos de dato nativos **(string, number, boolean)** así como tipos de dato mas complejos **(object, array)**.

La forma de utilizarlo no es muy diferente, en el siguiente ejemplo enviaremos un objeto en formato JSON como respuesta al endpoint **/user**.

```javascript
app.get("/user", function(req, res) {
    const users = [
        {
            name: "UwU",
            surname: "xd"
        },
        {
            name: "OwO",
            surname: "lol"
        }
    ]
    // enviamos en la respuesta nuestro arreglo de usuarios
    // en formato JSON
    res.json(users)
})
```

Con lo que hemos aprendido hasta ahora podemos representar todos los endpoints de nuestra aplicación y hacer que retornen información en formato JSON. Más adelante vamos a interactuar con una base de datos para conseguir la información pero por lo pronto estaremos respondiendo con objetos y arreglos del mismo archivo **server.js**.

## Middleware

Un middleware es un pedazo de funcionalidad que se ejecuta antes de que una petición llegue al endpoint. Es un punto medio dentro del flujo de cada acción que permite modular y optimizar el funcionamiento de cada endpoint.

Supongamos que tenemos dos endpoints, **/playlist** y **/song**. Para ambos endpoints es necesario poder identificar si las credenciales del cliente que ejecuta la petición son válidas. Podríamos representarlo de la siguiente manera:

```javascript
app.get("/playlist", function(req, res) {
    if (req.headers.credential === "valida") {
        // el usuario tiene credenciales validas
        // responder con todas las playlist
    } else {
        // el usuario no tiene credenciales validas
        // enviar un error
    }
})

app.post("/song", function(req, res) {
    if (req.headers.credential === "valida") {
        // el usuario tiene credenciales validas
        // responder con todas las canciones
    } else {
        // el usuario no tiene credenciales validas
        // enviar un error
    }
})
```

A simple vista realizar la verificación de las credenciales en cada endpoint no parece un problema, pero tenemos que hacernos dos preguntas aquí: **¿Qué pasa si tenemos mas endpoints?, ¿Qué pasa si tenemos que cambiar los parámetros de validación para cada uno?**

Un middleware nos permitiría manejar la petición antes de que llegue a cualquiera de los endpoints, es una funcionalidad en medio (por eso se llama middleware) que decide si la petición continua su camino o es interrumpida.

Adicionalmente los middleware también pueden **agregar/modificar ciertos atributos de los objetos request y response** para funcionalidad extra que quisiéramos darle a cada endpoint.

Los middleware se utilizan de la siguiente manera:

```javascript
// el método use() sirve para declarar un middleware
app.use(function(req, res, next) {
    // se obtienen los objetos request y response, adicionalmente
    // se nos pasa la referencia a una función *next* que podemos utilizar
    // para indicar que el middleware ha terminado su trabajo y quiere
    // continuar al siguiente middleware o terminar en el endpoint que indica
    // la petición
    if (req.headers.credential === "valida") {
        // el usuario tiene credenciales validas, continuar
        // al siguiente middleware o endpoint
        req.currentUser = { name: "UwU", surname: "OwO" }
        next()
    } else {
        res.status(400).json({ msg: "Credenciales inválidas" })
    }
})

app.get("/playlist", function(req, res) {
    // el objeto req en este caso es el mismo que se manejó
    // previamente en el middleware, por lo tanto
    // tiene disponible el atributo *currentUser*
    console.log(req.currentUser) // { name: "UwU", surname: "OwO" }
    res.json({ msg: "Todo bien" })
})

app.post("/song", function(req, res) {
    // currentUser también esta disponible aquí
    // es la magia de los middlewares
    console.log(req.currentUser) // { name: "UwU", surname: "OwO" }
    res.json({ msg: "Todo bien" })
})
```

En este ejemplo el middleware está interceptando la petición antes de que llegue al endpoint GET de **/playlist** y POST de **/song**, y añade al objeto de **request** un nuevo atributo "currentUser" que contiene la información del usuario autenticado en la plataforma.

Uno de los middlewares mas utilizados en express es el **express.json()** que nos permite parsear el cuerpo de las peticiones en formato JSON. Se utiliza de la siguiente forma:

```javascript
app.use(express.json())
```

Y nos permite acceder al cuerpo de las peticiones POST y PUT (por ejemplo) de la siguiente forma:

```javascript
app.post("/song", function(req, res) {
    console.log(req.body) // cuerpo de la petición POST
    res.json({ msg: "Cuerpo de la petición recibido correctamente!" })
})
```

Algo importante dentro de los middlewares es que su ejecución depende del orden con el que se registraron.

```javascript
app.use(function(req, res, next) {
    console.log(req.body) // undefined
    next()
})

// nota importante, ejecutar express.json() retorna una nueva función,
// es esta función la que se utiliza como un middleware
app.use(express.json())

app.use(function(req, res, next) {
    console.log(req.body) // { nombre: "Canción a registrar", likes: 300 }
    next()
})
```

## Router

El router de express nos da la posibilidad de dividir y manejar la lógica para una o muchas rutas y middlewares por medio de una URL.

Para crear un nuevo router usamos express.Router:

```javascript
const express = require("express")
const app = express()

const Router = express.Router
const userRouter = Router() // declaramos la nueva ruta

// userRouter ahora puede tener endpoints separados al de la app principal
userRouter.get("/users", function(req, res) {
    res.json([{ name: "Uwu" }, { name: "Owo" }])
})

// también puede declarar sus propios middlewares que se ejecutarán
// independientes a los middlewares de la app
userRouter.use(function(req, res, next) {
    // hacer algo aquí
    req.saludos = "Hola uwu"
    next()
})

userRouter.post("/user", function(req, res) {
    console.log(req.saludos) // Hola uwu
    res.json({ msg: req.saludos })
})

// finalmente le asignamos la URL a esta nueva ruta
app.use("/auth", userRouter)
```

En el código anterior declaramos una nueva ruta "userRouter" que tiene un middleware y dos endpoints. Asignamos esta nueva ruta a una URL **/auth** y la forma en la que se consumen estos endpoints ahora es:

    GET /auth/users
    POST /auth/user

## MongoDB

MongoDB es una base de datos NoSQL, esto significa que no organiza su información por medio de tablas, y esta información puede o no estar relacionada entre sí.

En el caso de Mongo la información se guarda en documentos, cada documento representa un objeto de javascript con los tipos de dato que se utilizarían en javascript.

Para trabajar con Mongo vamos a configurar un cluster en cloud.mongodb.com como ya se ha hecho en las clases pasadas,
