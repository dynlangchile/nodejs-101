NodeJS 101
=========

Un popurrí de recetas de cocina para meterse en NodeJS de una vez.

# Instalación de NodeJS

Visitemos [esta página](http://nodejs.org/), hay opciones para Windows, Linux y OS X. Encontraremos paquetes adecuados para cada tipo de máquina.

![Pantallazo](http://cl.ly/image/0O3g451W0d3B/Screen%20Shot%202013-01-15%20at%208.14.28%20AM.png)

También, desde luego, podemos compilar desde el código fuente (Teniendo `build-essentials` en Ubuntu, `XCode en OS X, etc):

````bash
$ curl -O http://nodejs.org/dist/v0.8.17/node-v0.8.17.tar.gz
$ tar xfvz node-v0.8.17.tar.gz
$ cd node-v0.8.17
$ ./configure
$ make
$ make install
````

Comprobamos con:

````bash
$ node

> process.version
'v0.8.17'
````
Empecemos:

# Consola

Hagamos algunos comandos en consola para entrar en calor:

Invocamos NodeJS con:

````bash
$ node
````

* Operaciones Matemáticas

````js
> 1+1
2
> 2/2
1
> (1+2)*3
9
> Math.sqrt(2)
1.4142135623730951
> 10%2
0
> 11%1
0
> 2**2
... 
> Math.pow(2,2)
4
````

* Manejo de Strings

````js

````

* Evaluación de expresiones regulares

(TODO)

* Funciones nativas en JavaScript

````js
> encodeURIComponent('20 Hola + +')
'20%20Hola%20%2B%20%2B'
> decodeURIComponent('20%20Hola%20%2B%20%2B')
'20 Hola + +'
````

* Invocar librerías nativas

  - [crypto](http://nodejs.org/api/crypto.html)

````js
> var crypto = require('crypto')
undefined
> crypto
{ Credentials: [Function: Credentials],
  createCredentials: [Function],
  Hash: [Function],
  createHash: [Function],
  Hmac: [Function],
  createHmac: [Function],
  Cipher: [Function],
  createCipher: [Function],
  createCipheriv: [Function],
  Decipher: [Function],
  createDecipher: [Function],
  createDecipheriv: [Function],
  Sign: [Function],
  createSign: [Function],
  Verify: [Function],
  createVerify: [Function],
  DiffieHellman: [Function],
  createDiffieHellman: [Function],
  getDiffieHellman: [Function],
  pbkdf2: [Function],
  randomBytes: [Function],
  pseudoRandomBytes: [Function],
  rng: [Function],
  prng: [Function] }
> var myString = 'password';
undefined
> crypto.createHash('md5').update(myString).digest("hex");
'5f4dcc3b5aa765d61d8327deb882cf99'
````

  - [path](http://nodejs.org/api/path.html)

````js
> var path = require('path')
undefined
> path.join('http://', 'www.google.com', 'reader')
'http:/www.google.com/reader'

// Ejemplo con `http:/` con un slash

> path.join('http:', 'www.google.com', 'reader')
'http:/www.google.com/reader'
````

* Generador de HahA aleatorio:

````js
> a='HAha';var b=[];for(var i=0;i<200;i++){var j=Math.floor(4*Math.random());if((i%2===0)&&(j%2===1)){j=j-1};if((i%2===1)&&(j%2===0)){j=j+1};b+=a[j]};console.log(b)

HahAHahAhAHaHAHahaHaHahAhaHAHAhahAHaHaHahaHAHahaHAHaHAhaHAHAhAhAHAHAhAhaHaHAHAhaHAHahAhahaHahAhAHaHAHAhaHahahahAHAHAhahAHAHaHAhAhAHAHAhahaHaHAhahahAHAhAHAHahAHAHahAhahahahAhahAHAhAhahahaHAhAHahaHaHAHa
````

Source: [hermanjunge.com](http://hermanjunge.com/post/35334164416/haha-generator)

# Scripts

Escribamos archivos con código sencillo:

* 'Un euler básico de mi colección'

[Problema Número 4, Proyecto Euler](http://projecteuler.net/problem=4)

  - `lib/euler_4.js`

````js
var maximum = 0;

for (var i = 999; i > 99; i--) {
  for (var j = 999; j > 99; j--) {
    var mult = i * j;
    if (isPalindrome(mult + '') && (mult > maximum)) maximum = i * j;
  };
};

console.log(maximum);

// Aux
function isPalindrome (str) {
  return str === str.split("").reverse().join("");
};
````

Lo ejecutamos con:

````bash
$ node lib/euler_4.js
906609
````

![Pantallazo](http://cl.ly/image/3F0x0f0D2y1E/Screen%20Shot%202013-01-15%20at%208.09.01%20PM.png)

* Ver si un `string` es una fecha válida:

(TODO)

Source: [hermanjunge.com](http://hermanjunge.com/post/33776860860/check-in-nodejs-whether-a-string-param-is-date-or-not)

* 'Autenticación Mega Básica'

 - `lib/mega_basic_auth.js`

````js

/**
 *  Los passwords son 'cocacola' y 'kryspo'
 *  (para que no se nos olviden)
 */
var database = {
  hermanjunge    : '6253e1406b64bbe6ba7b00ac0bf81257'
, elfenars       : 'b85905d73107fea2b11991dc9bc7f783'
}

// Requerimientos de modulo
var crypto    = require('crypto');
var readline  = require('readline');

var rl = readline.createInterface({
  input   : process.stdin
, output  : process.stdout
});

// Punto de Partida [main()]
rl.question('Ingrese Nombre de Usuario: ', inputUsuarioRecibido);
var usuario;   // <- Necesitamos una variable para almacenar el usuario que recibamos.

function inputUsuarioRecibido (user) {
  usuario = user;
  rl.question('Ingrese Password: ', inputPasswordRecibido);
}

function inputPasswordRecibido (password) {
  chequeaUsuarioPassword(usuario, password);
}

function chequeaUsuarioPassword (user, pass) {
  var encryptedPass = crypto.createHash('md5').update(pass).digest("hex");

  if (database[user] === encryptedPass) {
    console.log('Usuario Autenticado, Bienvenido');
  } else {
    console.log('Credenciales Incorrectas');
  }

  process.exit(0);
}
```

* ----- 'Que use un require de libreria, para descargarla con npm'

(TODO)

# Administración de Archivos

(TODO)

# Bases de Datos

## MySQL

(TODO)

## MongoDB

(TODO)

# HTTP

## Server

(TODO)

## Requests a otros server

(TODO)

# APIs

(TODO)
