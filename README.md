# Next Generation of Javascript

### Let y const

Cuando declaramos una variable bajo la palabra reservada `var` javascript entiende que: 
- la variable podrá ser reasignada o inicializada durante cualquier momento es decir que podemos sobre ecribirla
```javascript
    var fruit = "🍉";
    function reasignName(){
        fruit = "🥝";
        console.log(fruit);
    }
    reasignName() //
    var fruit = "🍓"
    console.log(fruit) //
```
- el escope de esta pertenece a su contexto de ejecución 
- que una variable declarada fuera de un bloque como una función pertenece al alcance global.

```javascript
    var fruit = "🍉";
    function reasignName(){
        var fruits = [🍑,🍌,🍇,🥝,🥥]
        fruit = "🥝";
        console.log(fruit);
    }
    var fruit = "🍓"
    console.log(window.fruit); // 🍓
    console.log(window.fruits); // undefined
```
###### Nota: las variables no declaradas siempre perteneceran al contexto global y a diferencia de las variables declaradas estas podrán ser eliminadas. Antipatron
```javascript
    var cat = "🐯";
    pig = "🐷";
    (function () {
        lion = "🦁";
    })()
    console.log(cat, pig, lion); //🐯 🐷 🦁

    delete cat; //false
    delete pig; //true
    delete lion; //true

    console.log(cat, pig, lion); //🐯 undefined undefined
```

Let introdujo un nuevo paradigma llamado *"block scope"* esto hace referencia a que los if, switch conditions, while loops, foor loops, o cualquier cosa que veamos con *{}* es un bloque. Y con let y const podremos declarar variables en el block scope, lo que significa que estás variables existen sólo en el bloque correspondiente.

Diferencias entre *var* y *let*:
- las variables let podrán ser reasignadas más no re-inicializadas o sobre-escritas:
```javascript
    let fruit = "🍉";
    function printMyFruit(){
        let fruit = "🥝";
        console.log(fruit);
    }
    fruit = "🍓"
    printMyFruit(); // "🥝"
    console.log(fruit) // "🍓"
    let fruit = "🍌" // Uncaught SyntaxError: Identifier 'fruit' has already been declared.
```
- las variables let y const sólo existiran dentro del bloque de scope:
```javascript
    function fruitsShow(){
        if(true){
            var fruit1 = "🍏";//exist in function scope
            const fruit2 = "🍌";//exist in block scope
            let fruit3 = "🥝";//exist in block scope
    }
        console.log(fruit1); 
        console.log(fruit2);
        console.log(fruit3);
    }

    fruitsShow();
    // 🍏
    // error: fruit2 is not defined
    // error: fruit3 is not defined
```
y const?, comparte las mismas caracteristicas que let pero a diferencia de esta, no podrá ser reasignada, será por consiguiente una variable de sólo lectura 
```javascript
    const cat = "🐱";
    cat = "🐈"; //Uncaught TypeError: Assignment to constant variable.
```

## Arrow Functions

- Las funciones flecha tienen una sintaxis diferente:
```javascript
    function myFunc(){
        ....
    }
```
```javascript
    const myFunc = () => {
        ....
    }
```
-  Resuelve cierto tipo de issues que se tenian con la palabra *this*:
```javascript
    var object = {
        dino: "🦖",
        arrow: () => {
            console.log(this.name);
        },
        anonym: function () {
            console.log(this.name);
        }
    };
    object.arrow() //undefined
    object.anonym() //🦖
```
- las arrow function capturan el valor del scope externo más cercano, para este ejemplo el escope mas cercano es el window por lo tanto la variable `dino` no existe.
- las arrow function no tienen su propio `this`, `arguments`, `super` o `new.target` lo que quiere decir que este tipo de funciones no podrán ser usadas como constructores.
- las funciones tradicionales definen su propio valor de this.
- las arrow function no tienen sus propio `this` utiliza el valor del contexto de ejecución que la contiene.

## Class

- Es una sintaxis de azucar para lo que conociamos como prototype.
```javascript
    class Person {
        ...    
    }

    function Person () {
        ...
    }
```
- Al igual que las funciones constructoras dentro de nuestras clases tendremos un contructor.
```javascript
    class Person {
        constructor(){
            this.name = name;
        }
    }

    function Person (name) {
        this.name = name;
    }
```
- Podremos hacer todo con clases excepto modificar un prototypo ya existente o nativo. 🐒
- Es el sistema de herencia de javascript esto significa que nuestras clases podrán tener metodos, nuestras clases podrán ser instanciadas y tambien podrán ser extendidas.
```javascript
    class Person {
        constructor(name){
            this.name = name;
        }
        //method
        printName() {
            console.log(this.name)
        }
    }
    //instance
    var me = new Person("Lu");
    me.printName() // "Lu"

    //extends
    class Developer extends Person {
        constructor(name, lenguage){
            super(name);
            this.lenguage = lenguage;
        }
        //method
        imAdeveloper() {
            console.log(`Hi I'm ${this.name} and I'm ${this.lenguage} developer`)
        }

    }
    //instance
    var meDev = new Developer('Lu', 'js');
    meDev.imAdeveloper();
```
- Los methodos son funciones atachadas a las classes.

## Spread & Rest Operators *{...}*
Es una forma de recibir multiples argumentos en llamadas a funciones o multiples elementos en un array literal.

- Spread en funciones
```javascript
    function spread(...myArguments) {
        console.log(myArguments)
    }

    spread("🦊","🐰","🐷","🐼","🐸","🐥","🐨","🦁","🐰"); // ["🦊","🐰","🐷","🐼","🐸","🐥","🐨","🦁","🐰"]
```
- Spread en array literales
```javascript
    var insects = ["🐛","🐝","🐞"];
    var animals = ["🦊","🐰","🐷",...insects,"🐼","🐸","🐥","🐨","🦁","🐰"];
    console.log(animals); //["🦊", "🐰", "🐷", "🐛", "🐝", "🐞", "🐼", "🐸", "🐥", "🐨", "🦁", "🐰"]
```
- Spread en objetos
```javascript
    var insects = { insects: ["🐛","🐝","🐞"]};
    var object= {
        ...insects,
        animals: ["🦊","🐰","🐷","🐼","🐸","🐥","🐨","🦁","🐰"]
    }
    console.log(object); //{insects: Array(3), animals: Array(9)}
```
## Destructuring
Este es un metodo simplificado de javascript para extraer multiples propiedades de un arreglo o un objeto, es llamado desconstrucción y usa una sintaxis similar al a de los arrys literales o objetos.

- destructuring con Arreglos
```javascript
    var animals = ["🦊","🐰","🐷","🐼","🐸","🐥","🐨","🦁","🐰"];
    var [animal1, animal2, animal3] = animals;
    console.log(animal1); //🦊
    console.log(animal2); //🐰
    console.log(animal3); //🐷
```
- destructuring con Arreglos y valores por defecto;
```javascript
    var animals = ["🦊","🐰"];
    var [animal1, animal2, animal3 = "🐥"] = animals;
    console.log(animal1); //🦊
    console.log(animal2); //🐰
    console.log(animal3); //🐥
```

- destructuring con Arreglos e ignorar elementos;
```javascript
    var animals = ["🦊","🐰", "🐥"];
    var [animal1, , animal3] = animals;
    console.log(animal1); //🦊
    console.log(animal3); //🐥
```
- destructuring con Objetos
```javascript
    var jungle = {
        animals: ["🦊","🐰","🐷","🐼","🐸","🐥","🐨","🦁","🐰"],
        insects: ["🐛","🐝","🐞"]
    }

    var { animals } = jungle;

    console.log(animals); //["🦊","🐰","🐷","🐼","🐸","🐥","🐨","🦁","🐰"]
```
## Referencia.
Cuando pasamos un argumento a un funcion o asignamos un valor primitivo (null, boolean, undefined, number, string, symbol) a una variable, este proceso siempre lo hará *"por valor"*, es decir que este tipo de elemento será copiado. Cuando nos referimos aun valor no primitivo (Object, Array, Function) javascript hace una copia de la referencia, esto quiere decir que tenemos un puntero por el cual podemos acceder al valor original

```javascript 
    const tree = {
        type: "apple tree 🍎";
    };

    const otherTree = tree;

    tree.type = "orange tree 🍊";

    console.log(otherTree) //"orange tree 🍊"
```

- referencias circulares, son aquellas en las que un objecto contiene una referencia así mismo.
```javascript
    const a = [1, 2, 3];
    a.push(a);
    console.log(a); //[1,2,3,[1,2,3]]
```



