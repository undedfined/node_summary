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
$ npm install --save express
```

Esto nos va a generar una carpeta llamada **node_modules** dentro de nuestro proyecto **node_server**, dentro de esta carpeta se guardan las dependencias que descargamos con cada `npm install`, en este caso express.

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

app.get("/user", function(req, res) {}) // obtener todos o un solo usuario
app.post("/user", function(req, res) {}) // crear un nuevo usuario
app.put("/user", function(req, res) {}) // modificar a un usuario
app.delete("/user", function(req, res) {}) // eliminar un usuario

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

Para trabajar con Mongo vamos a configurar un cluster en cloud.mongodb.com como ya se ha hecho en las clases pasadas, los pasos son los siguientes:

**1.** Crear una cuenta en cloud.mongodb.com. (sí ya tienes una cuenta, no es necesario)

**2.** Iniciar sesión.

**3.** Asegurarnos de que tenemos un cluster registrado, generalmente el primer cluster tiene el nombre "Cluster0".

3.1. En caso de NO tener un cluster en la cuenta, hacer click en el botón "Build a New Cluster".
3.2. Posteriormente se nos muestra una pantalla con tres opciones, vamos a seleccionar la primera que lleva el título de "Starter Clusters" y es gratuita.
3.3. La siguiente pantalla nos permite escoger entre tres proveedores donde vamos a almacenar nuestras bases de datos. Vamos a seleccionar la opción del medio "Google Cloud Platform" ya que lo vamos a estar utilizando en un futuro cuando hagamos el despliegue de nuestras aplicaciones.

3.4. Ahora vamos a seleccionar el datacenter más cercano donde se montará nuestro Cluster. En este caso Iowa en los Estados Unidos.

3.5. Finalmente le damos al botón inferior verde "Create Cluster".

3.6. Esperar de 3 a 5 minutos mientras se aloja nuestro cluster.

3.7. En la parte izquierda, en la sección de "Security" vamos a entrar en la opción "Database Access" y vamos a crear un nuevo usuario con los privilegios de lectura y escritura seleccionando el botón "+ Add New User". Es recomendable guardar en algún lugar el nombre de usuario y contraseña para poder utilizarlo en todos los ejercicios.
3.8. En la parte izquierda, en la sección de "Security" vamos a entrar en la opción "Network Access" donde vamos a registrar las direcciones IP autorizadas para acceder en nuestro cluster. Seleccionamos el botón "+ Add IP Address" y seleccionamos la opción "allow access from anywhere", por último lo confirmamos. Esto nos va a permitir acceder a nuestro cluster desde cualquier lugar.
**4.** En la parte izquierda, en la sección de "Atlas" entramos en la opción "Clusters" donde listamos los clusters disponibles.

**5.** Para conectar con un cluster vamos al botón "connect" debajo del nombre y versión de nuestro cluster. Si realizamos correctamente la configuración anterior de "Database Access" y "Network Access" vamos a tener la opción "Connect your application", la seleccionamos y copiamos el "Connection String" que se nos muestra.

Un cluster es un conjunto de ordenadores que se comportan como si fueran una sola computadora, el cluster de MongoDB nos permite crear, almacenar y acceder a múltiples bases de datos para nuestros proyectos gratuitamente.

Necesitamos tres cosas para poder continuar:

-   Un usuario registrado con permisos de lectura y escritura.
-   Acceso a nuestro cluster desde cualquier dirección IP.
-   El string de conexión, luce de la siguiente forma:
    mongodb+srv://username:password@clustername-xxxxx.gcp.mongodb.net/db_name?retryWrites=true&w=majority

Es string de conexión contiene lo siguiente:

-   mongodb+srv (el protocólo para conectar con mongodb)
-   username:password (las credenciales del usuario con permisos de lectura y escritura)
-   clustername-xxxxx (la dirección del cluster)
-   gcp.mongodb.net (el subdominio de google cloud server y el dominio de mongodb)
-   /db_name (el nombre de la base de datos dentro de nuestro cluster, es necesario cambiar este nombre cada que querrámos trabajar con una nueva base de datos)
-   ?retryWrites=true&w=majority (parametros de configuración para la conexión)

## Mongoose

Mongoose es una librería para Node JS que nos ayuda a conectar con una base de datos en MongoDB, adicionalmente nos da la funcionalidad necesaria para poder realizar acciones dentro de nuestra base de datos (crear, modificar, eliminar y consultar documentos).

Usamos el servidor que creamos anteriormente, hasta antes de la parte de middlewares deberíamos tener un orden como este en la carpeta:

```bash
/node_server
├── README.md
├── node_modules
├── package-lock.json
├── package.json
└── server.js
```

Para instalar mongoose necesitamos ejecutar el siguiente comando en nuestro proyecto de NPM:

```bash
$ npm install --save mongoose
```

Vamos a crear una carpeta `schemas` dentro de **node_server**, en esta carpeta vamos a definir como se van a comportar las entidades que vamos a guardar en nuestra base de datos de Mongo. Posteriormente vamos a crear un **index.js** dentro de la carpeta, este archivo va a agrupar todos los esquemas y a realizar la conexión, la estructura debería quedar de la siguiente forma:

```bash
/node_server
├── README.md
├── package-lock.json
├── package.json
├── schemas
│   └── index.js
└── server.js
```

Dentro de **/schemas/index.js** vamos a tener el siguiente código:

```javascript
// importamos la dependencia de mongoose
const mongoose = require("mongoose")

// realizamos la conexión
mongoose.connect(
    // string de conexión
    "mongodb+srv://username:password@clustername-xxxxx.gcp.mongodb.net/db_name?retryWrites=true&w=majority", // <-- aquí va una coma porque son parámetros y los parámetros se dividen por comas >:ccc
    // objeto de configuración, para que no salgan los warnings
    {
        useNewUrlParser: true,
        useUnifiedTopology: true
    }
)

// llamamos a la conexión con el identificador "db"
const db = mongoose.connection

// una vez que la base de datos esté conectada, imprimimos en la consola
db.once("open", function() {
    console.log("Base de datos abierta!")
})
```

Ahora vamos a representar algunos esquemas, pero antes es necesario que entendamos porqué necesitamos esquemas cuando estamos trabajando con MongoDB.

MongoDB es una base de datos NoSQL, significa que su información no la organiza por tablas, pero entonces ¿cómo la organiza? A diferencia de tablas donde sabes que cada registro contiene la información que cada una de sus columnas especifica, MongoDB organiza su información en colecciones de documentos donde cada documento puede ser diferente al otro. Esto resuelve muchos problemas de escabilidad pero también agrega cierta incertidumbre respecto a qué datos van a existir o estar disponibles en qué documentos.

Los esquemas sirven para indicar cómo se guardan y se comportan los documentos, no se implementan desde la capa de Mongo sino desde Node JS.

En resumen un esquema es solo un plano o mapa de cómo se guardan los documentos en sus respectivas colecciones.

Siguiendo el ejemplo con las entidades de REST, vamos a generar un esquema para la entidad Usuario. Dentro de la carpeta `schemas` vamos a crear un nuevo archivo Usuario.js (en este caso el archivo representa una entidad por lo que la primera letra es mayúscula).

**/schemas/Usuario.js**

```javascript
// traemos la dependencia de mongoose
const mongoose = require("mongoose")

// declaramos un nuevo UserSchema
// mongoose.Schema es un constructor
// como parametro recibe un objeto que especifica los tipos de dato
// que va a tener cada atributo
const UserSchema = new mongoose.Schema({
    name: String,
    age: Number,
    developer: Boolean,
    // otra forma de escribir los tipos de dato es por medio de otro objeto,
    // esto nos sirve para especificar mas cosas sobre los tipos de dato,
    // por ejemplo si desearamos que un tipo de dato fuera requerido
    // tendríamos que hacer lo siguiente
    job: { type: String, required: true }, // job es un string, y es requerido
    age: { type: Number, min: 18 }, // age es un numero y su valor mínimo es 18
    email: { type: String, maxlength: 40, required: true } // email es un string requerido y su longitud máxima es de 40 caracteres
})

// Una vez definido nuestro esquema es necesario conciliarlo o "ligarlo"
// a una colección en la base de datos, para eso ocupamos la función model
// del objeto mongoose

// model() recibe dos parametros:
// 1.- un string con el nombre de la colección
// 2.- un esquema
const User = mongoose.model("User", UserSchema)

// Finalmente exportamos el objeto *User* que creamos con el la funcion model
module.exports = User
```

**_Nota: Al momento de importar mongoose podemos de-estructurar el objeto para solo trabajar con el Schema y la función model, de la siguiente forma:_**

```javascript
const { Schema, model } = require("mongoose")
```

Por último en nuestro archivo **/schemas/index.js** vamos importar todos los esquemas en la carpeta y exportarlos en conjunto.

**/schemas/index.js**

```javascript
// importamos el esquema Usuario que se encuentra al mismo nivel en la carpeta
const User = require("./User.js")

// exportamos un objeto
module.exports = {
    // ya que el atributo y el valor tienen el mismo nombre "User" podemos
    // solo escribirlo una vez.
    // { User } -->> { User:User }
    User
}
```

De esta forma cada que un archivo externo requiera de alguno de estos esquemas, ya no tendrá que referirse a ellos uno por uno, solo tendrá que hacer require del módulo `schemas`.

```javascript
const { User, Song, Etc } = require("./schemas")
```

Antes de pasar a la siguiente sección vamos a aprender de cómo utilizar estas entidades que retorna el `mongoose.model()` después de declarar un esquema, para el ejemplo vamos a trabajar con User.

Supongamos que se crea un archivo **/test.js** inmediatamente en la carpeta **node_server** y nuestro directorio queda de la siguiente forma:

```bash
/node_server
├── README.md
├── package-lock.json
├── package.json
├── schemas
│   ├── User.js
│   └── index.js
├── server.js
└── test.js
```

**_test.js es solo un archivo de prueba y únicamente va a servir para explicar el funcionamiento de los modelos de mongoose, no debe ser incluido y solo debe ser tomado como referencia para futuros ejemplos_**

**/test.js**

```javascript
// para este ejemplo vamos a estar trabajando con async/await ya que nos permiten
// un manejo mas entendible de las promesas en javascript

// requerimos el modelo User de la carpeta *schemas*
const { User } = require("./schemas")

async function createNewUser() {
    // los modelos por sí solos son constructores, esto significa que podemos
    // invocarlos con la palabra *new* y crear a partir de ellos un objeto

    // de la misma forma, User() va a esperar un objeto con la información
    // necesaria para crear un documento en la base de datos.

    // entonces tenemos dos tipos de objetos, uno con la información plana y
    // necesaria para la creación del documento,
    // y otro objeto (el que retorna el constructor) que va a representar
    // directamente al documento en la base de datos

    const data = {
        // obj con info necesaria
        name: "OwO",
        surname: "UwU",
        age: 123
    }

    const user = new User(data) // obj documento

    // el identificador user ahora es una representación del documento
    // que va a guardarse en la base de datos

    // sin embargo todavía no existe en la base de datos,
    // hace falta guardarlo, para eso utilizamos el método .save()
    // y como devuelve una promesa, esperamos su resolución con await
    await user.save()

    console.log("saved user!", user) // usuario guardado!
}

async function getUsers() {
    // a pesar de que *User* es un constructor, también tiene métodos estaticos
    // que podemos utilizar para realizar operaciones en MongoDB, uno de estos
    // métodos es el de .find()

    // .find() nos va a retornar una promesa, y en su resolución van a llegar todos
    // los documentos en la colección *users* que cumplan ciertas condiciones,
    // en este caso cuando .find() no recibe ningún objeto en su parámetro
    // va a retornar todos los documentos de la colección dentro de un arreglo

    const users = await User.find()
    console.log("todos los usuarios!", users)

    // si deseáramos obtener todos los usuarios que cumplan cierta condición
    // tendríamos que pasar un objeto dentro de .find() que especifique
    // la condición

    // por ejemplo supongamos que quiero obtener a todos los usuarios que son
    // desarrolladores

    const developers = await User.find({ developer: true })
    console.log("todos los desarrolladores!", developers)

    // para describir mas a profundidad mis consultas vamos a necesitar un objeto
    // que indique exactamente qué es lo que buscamos para qué campo en mi esquema

    // por ejemplo supongamos que quiero obtener a todos los usuarios mayores
    // a 22 años

    const elders = await User.find({ age: { $gt: 22 } })
    console.log("usuarios mayores de 22!", elders)

    // finalmente, si no queremos una lista mas bien un usuario individual
    // podemos usar .findOne()

    // .findOne() también puede recibir un objeto para especificar más la búsqueda
    // y va a retornar al primer documento que encuentre que cumpla
    // la condición

    const someUser = await User.findOne({ email: "uwu@uwu.com" })
    console.log("usuario con el correo uwu@uwu.com", someUser) // { _id:"123", name: "uwu", email: "uwu@uwu.com" }

    // en caso de que un documento en .findOne() no haya sido encontrado
    // su valor va a retornar null

    const otherUser = await User.findOne({ email: "noexiste@uwu.com" })
    console.log("usuario con el correo noexiste@uwu.com", otherUser) // null

    // en cambio, cuando ningún documento es encontrado en un .find()
    // solo se retorna un arreglo vacío

    const nonDevs = await User.find({ developer: false })
    console.log("usuarios no desarrolladores", nonDevs) // [] arreglo vacío, porque todos somos desarrolladores!
}
```

## Integrando

Con lo que hemos aprendido basta para desarrollar un CRUD simple. CRUD significa (Create, Read, Update, Delete) y se refiere al desarrollo de estas acciones para una entidad singular. Por ejemplo cuando decimos que vamos a crear un CRUD de User significa que vamos a representar la funcionalidad suficiente para crear, consultar, modificar y eliminar nuestros documentos en la colección de usuarios.

Nuestro archivo **/schemas/User.js** quedaría de la siguiente forma:

```javascript
const { Schema, model } = require("mongoose")

const UserSchema = new Schema({
    name: String,
    surname: String,
    age: Number,
    email: String,
    password: String
})

const User = model("User", UserSchema)

module.exports = User
```

El archivo **/schemas/index.js** quedaría prácticamente igual a la última vez:

```javascript
const mongoose = require("mongoose")

mongoose.connect(
    "mongodb+srv://username:password@clustername-xxxxx.gcp.mongodb.net/db_name?retryWrites=true&w=majority",
    {
        useNewUrlParser: true,
        useUnifiedTopology: true
    }
)

const db = mongoose.connection

db.once("open", function() {
    console.log("Base de datos abierta!")
})

// Aquí importamos los esquemas de las demás entidades
const User = require("./User")

// Aquí las exportamos
module.exports = {
    User
}
```

Por último, el archivo **/server.js** quedaría de la siguiente forma:

```javascript
const express = require("express")
const app = express()

// importamos los esquemas
// ya que tenemos un index.js dentro de la carpeta schemas
// no es necesario escribirlo, basta solo con escribir el nombre
// de la carpeta
const { User } = require("./schemas")

app.get("/user", function(req, res) {}) // consultar uno o muchos usuarios
app.post("/user", function(req, res) {}) // crear nuevo usuario
app.put("/user", function(req, res) {}) // modificar usuario
app.delete("/user", function(req, res) {}) // eliminar un usuario

app.listen(8080, function() {
    console.log("Servidor escuchando :)")
})
```

Vamos a empezar a trabajar en cada uno de los endpoints:

-   _GET_ **/user**
-   _POST_ **/user**
-   _PUT_ **/user**
-   _DELETE_ **/user**

```javascript
// empezamos con post
// post('/user') va a escuchar cuando se haga una petición de tipo POST
// a la URL /user

// la función es lo que se va a ejecutar, aquí nos llega el objeto de
// request y el de response
app.post('/user', async function(req, res)
	// vamos a tomar el cuerpo de la petición y a mostrarlo en la consola
	// esto debería mostrarnos el objeto JSON que se envió
	// desde un cliente para la creación de un usuario
	console.log(req.body) // { name: "Uwu", surname: "Owo", age: 30, email: "correo", password: "1234" }

	// creamos un nuevo documento User
	const user = new User(req.body)

	// y finalmente esperamos a que se guarde el usuario
	await user.save()

	// adicionalmente retornamos el objeto de usuario en formato JSON
	res.json(user)
})

// con esto estaríamos terminando un registro simple de usuario, vamos ahora
// a realizar una consulta

app.get('/user', async function(req, res) {
	// el cliente hace una petición para obtener a todos los usuarios
	// primero los obtenemos y después los contestamos en formato JSON

	const users = await User.find()
	res.json(users)

	// esa estuvo facil jajaja
})

// ¿Qué pasa si el cliente quisiera obtener un usuario específico dado un id?
// express nos permite tomar ciertos parámetro por medio de la URL, de la
// siguiente forma
app.get('/user/:id', async function(req, res) {
	// los dos puntos en la url indican que "id" es un valor variable
	// que va a proporcionar el cliente cuando ejecute esta petición

	// para encontrar UN SOLO usuario por medio de su identificador podríamos
	// utilizar dos metodos, .findOne() y .findById(), vamos a probar con ambos

	// primero necesitamos tomar el parámetro que se nos envió en la URL,
	// se obtiene dentro del objeto de request, en otro objeto llamado params
	// de la siguiente forma
	console.log(req.params.id)

	// req.params.id corresponde con el nombre del parametro después
	// de los dos puntos en la URL /user/:id

	// la primera forma .findOne()
	const user = User.findOne({ _id: req.params.id })
	console.log("buscar usuario donde _id === req.params.id", user)

	// la segunda forma .findById()
	const sameUser = User.findById(req.params.id)
	console.log("usuario por su id!", sameUser)

	res.json(user)
})
```
