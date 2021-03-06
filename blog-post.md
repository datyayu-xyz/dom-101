# DOM 101: Aprendiendo a usar el DOM desde 0.
El DOM es un concepto que todo desarrollador web debe conocer. Aun así hay muchos que no saben como usarlo directamente, a pesar de que gracias a él es que podemos hacer páginas interactivas. Aprender a usar el DOM no sólo te ayuda a optimizar tu código y prevenir dependencias innecesarias sino que también te brinda un mejor entendimiento de como funcionan las cosas en tu pagina.

Muchas veces por conveniencia terminamos dependiendo de librerías como jQuery para modificar el html de nuestra pagina. Pero, ¿alguna vez te haz preguntado si realmente vale la pena incluir una librería entera sólo para modificar un par de lineas? ¿Y qué pasa si no tienes acceso a jQuery? ¿Cómo puedes agregar contenido dinámicamente a una pagina sin depender de otras librerías? Bueno, en este post revisaremos como utilizar el DOM, la manera nativa de hacerlo, sin necesidad de dependencias.

---

Si tienes algún problema siguiendo este post o encuentras algo que esté incorrecto, puedes revisar los [ejemplos disponibles en github](https://github.com/datyayu-xyz/dom-101).

---

## ¿Qué es el DOM?
El DOM (Document Object Model) es una representación con forma de árbol que contiene los nodos de html que se muestran en el navegador y que nos provee de bonitas APIs para modificarlo y extenderlo por medio de javascript.

Puesto de otra forma, es la manera en la que el navegador nos da acceso al contenido de nuestro html y también es por medio de este que podemos modificarlo y agregar interactividad a un sitio.

Como dije antes, el DOM tiene una estructura con forma de arbol. Esta compuesto por nodos que tienen nodos hijos y así sucesivamente. Por ejemplo, el nodo `html` tiene dos hijos: `head` y `body`; y estos a su vez tienen más nodos hijos. `head` puede tener un `title` o un `meta`, y `body` puede tener un `h1` o un `p`.

![](https://datyayu.xyz/content/images/2016/10/dom.gif)

Tal vez es más fácil verlo como etiquetas de html dentro de otras etiquetas, pero es importante que tengamos en cuenta esa relación entre elementos o nodos padres e hijos, ya que al usar javascript para modificar el DOM se emplean mucho esos conceptos.

---

Después de revisar como se estructura el DOM, podemos empezar a ver como utilizarlo.

## Crear un elemento
Lo primero a conocer, es que para crear elementos simplemente ocupamos usar la funcion `document.createElement('nombre-de-la-etiqueta')`, donde el nombre de la etiqueta puede ser cualquiera, como `div`, `ul` o `h1`.

```js
var elemento = document.createElement('div');
console.log(elemento); // <div></div>
```

## Modificar un elemento
Ya que sabemos como crear un elemento, ahora tenemos que agregarle algo de contenido. Para agregar texto a un nodo podemos hacerlo con `nodo.textContent = 'texto'`.

```js
var elemento = document.createElement('div');
elemento.textContent = 'Hola, Mundo!';

console.log(elemento); // <div>Hola, Mundo!</div>
```

También podemos cambiar los atributos de un elemento usando `nodo.setAttribute` o simplemente `nodo.atributo`, por ejemplo:

```js
var elemento = document.createElement('input');
elemento.setAttribute('type', 'text');
elemento.id = 'mi-input';

console.log(elemento); // <input type="text" id="mi-input">
```

Como consejo, si vas a usar este método para agregar o remover clases de css, lo mejor es usar [`nodo.classList`](https://developer.mozilla.org/es/docs/Web/API/Element/classList) ya que esto nos brinda un mejor manejo de las mismas y evita tener más de una vez la misma clase en nuestro elemento.

```js
var elemento = document.createElement('div');

elemento.classList.add('hello');
elemento.classList.add('hello');
console.log(elemento.classList.contains('hello')); // true

elemento.classList.remove('hello');
console.log(elemento.classList.contains('hello')); // false
```



## Agregar elementos
Ya vimos como crear y modificar Elementos pero para que estos sean utiles debemos poder agregarlos a nuestra pagina, no sólo mostrarlos en la consola.

Para este caso, lo más comun es usar `nodoPadre.appendChild(nodoHijo)`. Esto basicamente le dice que tome el elemento `nodoHijo` y lo inserte dentro del `nodoPadre`.

```js
var miNodo = document.createElement('div');
miNodo.textContent = 'Hola soy un elemento creado por ti';

document.body.appendChild(miNodo);
```

<iframe width="100%" height="300" src="//jsfiddle.net/datyayu/hzxgbj4t/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Como aclaración, `document.body` se usa para referirse a la etiqueta `<body></body>` de nuestro html, así que en el ejemplo anterior agregamos el `<div>` que creamos al `<body>` de la página.

También, si quieres agregar un elemento a un lado de otro (como nodos hermanos, y no como padre-hijo) puedes usar `nodoPadre.insertBefore(nodoAInsertar, nodoDeReferencia)`, donde le dices que  busque dentro del `nodoPadre` al `nodoDeReferencia` y ponga al `nodoAInsertar` justo antes de este.

```js
var contenedor = document.createElement('ul');
var hijo1 = document.createElement('li');
var hijo2 = document.createElement('li');

hijo1.textContent = '1';
hijo2.textContent = '2';

document.body.appendChild(contenedor);
contenedor.appendChild(hijo1);

contenedor.insertBefore(hijo2, hijo1);
```
<iframe width="100%" height="300" src="//jsfiddle.net/datyayu/f0qhqz5f/embedded/js,html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>


## Remover elementos
Para remover elementos del DOM, tenemos la función `nodoPadre.removeChild(nodoHijo)`, la cual elimina el `nodoHijo` dentro del `nodoPadre`.

```js
var miNodo = document.createElement('div');
miNodo.textContent = 'Hola soy un elemento creado por ti';
document.body.appendChild(miNodo);

document.body.removeChild(miNodo); // El elemento es removido del DOM
```

<iframe width="100%" height="300" src="//jsfiddle.net/datyayu/9Luj12v9/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>


## Buscar elementos
Hasta ahora hemos usado sólo elementos que creamos nosotros y que ya tenemos su referencia guardada en alguna variable, pero para interactuar con los elementos que ya existen en el DOM, ocupamos primero obtener esa referencia para usarla en nuestro código.

Dependiendo de cómo quieras consultar el DOM tienes diferentes alternativas.

- Si quieres encontrar un sólo elemento por su id, puedes usar `document.getElementById(id)`

```js
var miElemento = document.getElementById('id-del-elemento');
```

- Si quieres encontrar uno o más elementos usando sus clases, para eso está `document.getElementsByClassName(clase)`.

```js
var misElementos = document.getElementsByClassName('clase-del-elemento');
```

- Si quieres encontrar uno o más elementos por su etiqueta, tienes `document.getElementsByTagName(etiqueta)`.

```js
var misElementos = document.getElementsByTagName('div');
```

Estás anteriores son bastantes especificas pero si quieres algo más general similar a jQuery, tenemos `document.querySelector` y `document.querySelectorAll`.

```js
var miElementoConId = document.querySelector('#id-del-elemento');
var miElementoConClase = document.querySelector('.clase-del-elemto');
var miElementoConEtiqueta = document.querySelector('div');
var miElementoConSelectores = document.querySelector('div#mi-elemento');
```
Como vez, la ventaja de `querySelector` es que nos brinda una busqueda más flexible y si estás familiarizado con jQuery, usar `querySelector` es super fácil de entender. 

La diferencia entre `querySelector` y `querySelectorAll` es que el `querySelector` te regresa el primer elemento que encuentre con el selector que le especifiques, mientras que `querySelectorAll` te regresa una lista de todos los elementos que cumplan con ese selector.


> #### NodeList vs Array
> Como nota importante, a simple vista parecería que las funciones `getElementsByClassName`, `getElementsByTagName` y `querySelectorAll` te regresan un arreglo (`Array`) de elementos, pero en realidad lo que te regresa es un objeto llamado `NodeList`. Para la mayoría de los casos son iguales, pero `NodeList` carece de métodos como `forEach` o `entries` en navegadores viejos, por lo que hay que tener cuidado. Para iterar en un `NodeList`, puedes remplazar el `forEach` por un `for` si así lo requieres.

> `for (var i = 0; i < miNodeList.length; ++i) {
>  miNodeList[i] = /* cosas */
> }`

Y eso todo, con estos conocimientos ya puedes empezar a usar el DOM para añadir interactividad a tu pagina sin necesidad de incluir librerías y dependencias :D 

---

El repo con los ejemplos este post [está disponible en github](https://github.com/datyayu-xyz/dom-101) para cualquier duda que tengas o mejora que quieras agregar, así que no dudes en hacerlo!