# CΓ³mo copiar un array en JavaScript

Diciembre del 2021
Al igual que vimos con objects en cΓ³mo copiar un object, clonar arrays no es trivial ya que en JavaScript todo se copia por referencia

CuΓ‘l es el problema con clonar arrays?

const miarray = ['Me caes muy bien', 'Filomena']

const miarray2 = miarray
miarray2[0] = 'Me caes fatal'
miarray2[1] = 'Eustakio'

console.log(miarray) // ["Me caes fatal", "Eustakio"]
console.log(miarray2) // ["Me caes fatal", "Eustakio"]
Lo suyo hubiera sido tener dos copias del array con contenido distinto e independiente, pero vemos que cuando cambiamos el segundo array tambiΓ©n estamos cambiando el primero, y no es lo que queremos

Para copiar todo el array podemos hacer lo siguiente:

Copia simple
const myArr = ['ππππ']

const myCopy = myArr
myArr[0] = 'π₯π₯π₯π₯'

console.log(myArr) // ["π₯π₯π₯π₯"]
console.log(myCopy) // ["π₯π₯π₯π₯"]
Copia shallow
Una copia shallow de arrays se refiere a copiar la primera βcapaβ o la primera dimensiΓ³n del array

Se puede hacer con un Array.from() o con el spread operator, que lo que hace es βdesplegarnosβ la estructura de datos

const myArr = [
'ππππ', // una dimensiΓ³n
'ππππ', // una dimensiΓ³n
'ππππ', // una dimensiΓ³n
['π¦π¦π¦π¦', 'ππππ'], // dos dimensiones
{ myOtherArray: ['π€πΎπ€πΎπ€πΎπ€πΎ'] }, // tres dimensiones
]

const myCopy = myArr // copia simple
const myShallowCopy = Array.from(myArr) // copia shallow, con el Array.from
const myShallowCopy2 = [...myArr] // copia shallow, con el spread operator

// ahora cambio las variables de mi array inicial y asΓ­ vemos cΓ³mo de independiente es mi copia

myArr[0] = 'π₯π₯π₯π₯'
myArr[1] = 'π₯π₯π₯π₯'
myArr[2] = 'π₯π₯π₯π₯'
myArr[3][0] = 'π₯π₯π₯π₯'
myArr[3][1] = 'π₯π₯π₯π₯'
myArr[4].myOtherArray[0] = 'π₯π₯π₯π₯'

// el array original y la copia simple, son idΓ©nticas

console.log(myArr) // ["π₯π₯π₯π₯", "π₯π₯π₯π₯", "π₯π₯π₯π₯", ["π₯π₯π₯π₯", "π₯π₯π₯π₯"], {myOtherArray: ["π₯π₯π₯π₯"]}]
console.log(myCopy) // ["π₯π₯π₯π₯", "π₯π₯π₯π₯", "π₯π₯π₯π₯", ["π₯π₯π₯π₯", "π₯π₯π₯π₯"], {myOtherArray: ["π₯π₯π₯π₯"]}]

// Las copias shallow, ves que ha protegido la primera dimensiΓ³n del array, pero no las demΓ‘s

console.log(myArr) // ["π₯π₯π₯π₯", "π₯π₯π₯π₯", "π₯π₯π₯π₯", ["π₯π₯π₯π₯", "π₯π₯π₯π₯"], {myOtherArray: ["π₯π₯π₯π₯"]}]
console.log(myShallowCopy) // ["ππππ", "ππππ", "ππππ", ["π₯π₯π₯π₯", "π₯π₯π₯π₯"], {myOtherArray: ["π₯π₯π₯π₯"]}]
console.log(myShallowCopy2) // ["ππππ", "ππππ", "ππππ", ["π₯π₯π₯π₯", "π₯π₯π₯π₯"], {myOtherArray: ["π₯π₯π₯π₯"]}]
Copia deep
Si una copia shallow era βsuperficialβ, una copia deep busca hacer una copia completa de todo el array, una copia de todas sus dimensiones

Y para hacerlo tenemos distintas maneras:

De forma manual con un loop que nos itere por todos los lados

Utilizando alguna librerΓ­a ultra-famosa como lodash

Vamos a ver los dos mΓ©todos, el primero (el manual) utilizando la soluciΓ³n de samanthaming, y el segundo con la librerΓ­a lodash

// importo lodash
import \_ from 'lodash'

// funciΓ³n manual para clonar arrays
const clone = items => items.map(item => (Array.isArray(item) ? clone(item) : item))

// el array original
const myArr = ['ππππ', ['ππππ'], { myOtherArray: ['ππππ'] }]

// copia shallow
const myShallowCopy = [...myArr]

// copia deep manual
const myDeepCopy = clone(myArr)

// copia deep con los dos mΓ©todos de lodash, uno para shallow y otro para deep
const myShallowLodashClone = _.clone(myArr)
const myDeepLodashClone =_.cloneDeep(myArr)

// cambio el array original
myArr[0] = 'π₯π₯π₯π₯' // 1 dimensiΓ³n
myArr[1][0] = 'π₯π₯π₯π₯' // 2 dimensiones
myArr[2].myOtherArray[0] = 'π₯π₯π₯π₯' // 3 dimensiones

// Y a ver quΓ© nos dan los clones

console.log(myArr) // ["π₯π₯π₯π₯", ["π₯π₯π₯π₯"], {myOtherArray: ["π₯π₯π₯π₯"]}]
console.log(myShallowCopy) // ["ππππ", ["π₯π₯π₯π₯"], {myOtherArray: ["π₯π₯π₯π₯"]}]
console.log(myDeepCopy) // ["ππππ", ["ππππ"], {myOtherArray: ["π₯π₯π₯π₯"]}]
console.log(myShallowLodashClone) // ["ππππ", ["π₯π₯π₯π₯"], {myOtherArray: ["π₯π₯π₯π₯"]}]
console.log(myDeepLodashClone) // ["ππππ", ["ππππ"], {myOtherArray: ["ππππ"]}]
Las copias shallow sΓ³lo copian la primera dimensiΓ³n, como hemos visto antes
La copia deep con la funciΓ³n propia no consigue copiar todas las dimensiones
Y una copia completa sΓ­ la conseguimos con \_.cloneDeep de lodash
PodrΓ­amos modificar la funciΓ³n para conseguir los mismos resultados de lodash, de hecho la tienes en lodash.clonedeep, por lo que no tienes que importar toda la librerΓ­a

O directamente puedes ir al github, ver la funciΓ³n, y utilizarla tu mismo

Copia deep casi completa con JSON
Hay otro mΓ©todo tremendamente prΓ‘ctico y que no requiere de librerΓ­as externas

Es el mΓ©todo JSON, y se trata de convertir nuestro array en un objeto JSON, y luego volver a convertir ese objeto en un nuevo array

En el proceso las referencias se consolidan en nuevos objetos, con lo que aunque la funciΓ³n JSON no estΓ‘ pensada para clonar datos, nos sirve perfectamente a este propΓ³sito

// el array original
const myArr = ['ππππ', ['π₯π₯π₯π₯'], { myOtherArray: ['π¦π¦π¦π¦'] }]

// la copia con JSON
const myJSONCopy = JSON.parse(JSON.stringify(myArr))

// cambio el array original
myArr[0] = 'π₯π₯π₯π₯' // 1 dimensiΓ³n
myArr[1][0] = 'π₯π₯π₯π₯' // 2 dimensiones
myArr[2].myOtherArray[0] = 'π₯π₯π₯π₯' // 3 dimensiones

// Y a ver quΓ© nos da

console.log(myArr) // ["π₯π₯π₯π₯", ["π₯π₯π₯π₯"], {myOtherArray: ["π₯π₯π₯π₯"]}]
console.log(myJSONCopy) // ["ππππ", ["π₯π₯π₯π₯"], {myOtherArray: ["π¦π¦π¦π¦"]}]
Conseguimos clonar todo el objeto como hacΓ­amos antes con el \_.cloneDeep de lodash

Ventajas? Se lee muy fΓ‘cilmente y no dependes de una librerΓ­a externa
Desventajas? Cierta incompatibilidad con algunos tipos de datos que no se pueden clonar bien (como Date)
En cuanto a velocidad, haciendo un benchmark con measurethat.net vemos lo siguiente:

Con estructuras sencillas, la soluciΓ³n con lodash es un 1.5x mΓ‘s rΓ‘pida que la soluciΓ³n con JSON
Con estructuras βalgoβ complejas como la que verΓ‘s debajo, la soluciΓ³n JSON ya empieza a ir mΓ‘s rΓ‘pido que la de lodash
const myArr = [
'ππππ',
['π₯π₯π₯π₯'],
{ myOtherArray: ['π¦π¦π¦π¦'] },
'ππππ',
['π₯π₯π₯π₯'],
{ myOtherArray: ['π¦π¦π¦π¦'] },
'ππππ',
['π₯π₯π₯π₯'],
{ myOtherArray: ['π¦π¦π¦π¦'] },
'ππππ',
['π₯π₯π₯π₯'],
{ myOtherArray: ['π¦π¦π¦π¦'] },
'ππππ',
['π₯π₯π₯π₯'],
{ myOtherArray: ['π¦π¦π¦π¦'] },
'ππππ',
['π₯π₯π₯π₯'],
{ myOtherArray: ['π¦π¦π¦π¦'] },
'ππππ',
['π₯π₯π₯π₯'],
{ myOtherArray: ['π¦π¦π¦π¦'] },
'ππππ',
['π₯π₯π₯π₯'],
{ myOtherArray: ['π¦π¦π¦π¦'] },
'ππππ',
['π₯π₯π₯π₯'],
{ myOtherArray: ['π¦π¦π¦π¦'] },
]
Con este array, la soluciΓ³n mΓ‘s rΓ‘pida serΓ­a la de JSON

Eso sΓ­, con ninguno de estos mΓ©todos conseguirΓ­amos clonar un array que tenga una funciΓ³n en su estructura, lo puedes ver abajo

const myArr = [
'ππππ',
['π₯π₯π₯π₯'],
{ myOtherArray: ['π¦π¦π¦π¦'], myMethod: () => console.log('hola como estamos') },
]
Este mΓ©todo tiene una funciΓ³n en la estructura, pues escojas el mΓ©todo que escojas, esa funciΓ³n no la podrΓ‘s clonar

Interesante
[https://www.youtube.com/watch?v=luJ6GpZh4BM&ab_channel=hdeleon.net]
