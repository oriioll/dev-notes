
<h1><span style="background-color: #F7DF1E; color: black; padding: 2px 6px; font-weight: bold; border-radius: 3px; font-family: sans-serif;">JS</span> - Funcions, POO, Mètodes array - Map Set</h1>

## 1. Funcions

### A. Funcions clàssiques
- Si al cridar a la funció es passa un paràmetre de menys, el seu valor es queda com `undefined`. Si es passen de més, es guardaran a l’objecte arguments com a array.
- Al declarar la funció, als paràmetres se'ls pot assignar un valor per defecte:

```javascript
function nom(param1, param2) {
	return valor;
}

// Paràmetres per defecte
function nom(param1 = defaultValue1, param2 = defaultValue2){ }
```

### B. Funcions Anònimes
- Són funcions sense nom que es defineixen directament on es necessiten. Es creen mitjançant l'assignació d’una funció a una variable o passant-les com a paràmetre a una altra funció.
- També s'utilitzen com a callbacks o funcions d’ordre superior.

```javascript
// Assignant a una variable
let multiply = function(x, y) {
  return x * y;
};

// Dins d’un event
document.form1.colorButton.onclick = function(event){
  setBGColor();
};

// Dins d’un eventListener
my_element.addEventListener("click", function (e) {
  console.log(this.className);
  console.log(e.currentTarget === this);
});
```

### C. Funcions Fletxa
- Són funcions anònimes amb una sintaxi més curta utilitzant la fletxa: `=>`

```javascript
// Assignant a una variable
let multiply = (x, y) => x * y;

// O amb corxetes i return
let multiply = (x, y) => {
    return x * y;
}

// Dins d’un event
document.form1.colorButton.onclick = (event) => setBGColor();

// Dins d’un eventListener
my_element.addEventListener("click", (e) => {
  console.log(this.className);
  console.log(e.currentTarget === this);
});
```

---

## 2. POO en JavaScript

### A. Intro i mòduls
- Per poder programar amb POO i diferents arxius en JS has d’utilitzar mòduls i importar/exportar.

```html
<script src="src/main.js" type="module" defer></script>
```

```javascript
// Classe a arxiu extern:
export default class Animal {
    // constructor i atributs
    constructor(nom) {
        this.nom = nom;
    }
    // mètodes sense paraula ‘function’
    menjar() { }
}
```

```javascript
// Importació de classe per instanciar objectes o heretar:
import Animal from './Animal.js'; // Ara ja es pot instanciar
```

### B. Atributs, setters i getters
- Al crear una classe, els atributs públics no cal declarar-los si hi ha constructor. Si hi ha atributs privats, sí que s’han de declarar.

```javascript
// Ús atributs públics sense declarar
export default class Animal {
    constructor(nom, tipus) {
        this.nom = nom;
        this.tipus = tipus;
        this.pes = 124;
    }
    menjar() { }
}

// Ús atributs públics i privats amb declaració
export default class Animal {
    nom;
    tipus;
    #pes; // modificador d'accés privat (#)
    
    constructor(nom, tipus) {
        this.nom = nom;
        this.tipus = tipus;
        this.#pes = 124; // this amb # ja que és privat
    }
    menjar() { }
}

// Setters i getters
export default class Animal {
    #pes;
    
    constructor() {
        this.#pes = 124;
    }
    
    get pes() {
        return this.#pes;
    }
    
    set pes(pes) {
        this.#pes = pes;
    }
}

// Crida a setters i getters - ara es pot fer amb el punt
animal.pes = 324;
const pesAnimal = animal.pes;
```

### C. Herència i polimorfisme per Override
- A JS existeix l'herència entre classes amb la paraula `extends`. Els mètodes de la classe pare es poden sobreescriure (Override - opcional).

```javascript
export default class Cotxe extends Vehicle {
    #portes;
    
    constructor(nom, marca, portes) {
        super(nom, marca); // li passo al constructor del pare els seus valors
        this.#portes = portes;
    }
    
    // sobreescriure mètode arrancar que té la superclasse
    arrancar() {
        return "el cotxe arranca";
    }
}
```

- Per treballar amb objectes, es poden utilitzar els mètodes `.toString()` i `.valueOf()`. Es poden sobreescriure perquè retornin el desitjat.
  - Per defecte, `.toString()` retorna un string `[object Object]`
  - `.valueOf()` retorna l’objecte en si.

### D. Static
- A JS existeixen classes amb mètodes i atributs estàtics.
- Els mètodes estàtics no estan relacionats amb la instància i no poden utilitzar `this`.

```javascript
export default class Dau {
    static tirarDau() {
        return RandomNumber(1,10);
    }
}

// main.js
console.log(Dau.tirarDau()); // No s'instancia, nomClasse.nomMetodeStatic
```

- Els atributs estàtics són valors globals dins de la classe. No es creen ni es canvien per cada instància, sinó que són com variables globals.

```javascript
class Alumne {
    static contador = 0; // Variable static
    
    constructor() {
        Alumne.contador++; // Incrementa la variable static
    }
}

// main.js
const alum1 = new Alumne("Juan");
const alum2 = new Alumne("Ana");
console.log(Alumne.contador); // 2 -> s'accedeix des del nom de la classe
```

---

## 3. Mètodes Array

### A. Loops
- Mètode `.forEach()` per recórrer cada element de l’array.

```javascript
let nums = [10, 20, 30, 40, 50];
nums.forEach((num) => { 
    console.log(num);
});
```

### B. every, some, find, findIndex()
- `.every()`: Retorna `true` si tots els elements compleixen una condició.
- `.some()`: Retorna `true` si només algun element compleix la condició.

```javascript
let arrayNotas = [5.2, 3.9, 6, 9.75, 7.5, 3];
let totsAprovats = arrayNotas.every(element => element >= 5); // false
let algunAprovat = arrayNotas.some(element => element >= 5); // true
```

- `.find()`: Retorna el primer element que compleix la condició o `undefined`.
- `.findIndex()`: Retorna el seu índex.

```javascript
let primerAprobado = arrayNotas.find(element => element >= 5); // 5.2
```

### C. map, reduce i ArrayFrom
- `.map()`: Pot aplicar canvis a cada posició de l’array retornant un array nou amb els canvis.

```javascript
let arrayNotasSubidas = arrayNotas.map(nota => nota + nota * 0.10); // nou array
```

- `.reduce()`: Pot obtenir un valor calculat sobre tots els elements de l’array.

```javascript
// Suma de tots els elements
let sumaNotas = arrayNotas.reduce((total, nota) => total + nota); // 35.35

// Calcular nota més alta
let maxNota = arrayNotas.reduce((max, nota) => nota > max ? nota : max); // 9.75
```

- `Array.from()`: Permet crear un array a partir d’un altre aplicant un map.

```javascript
let arrayNotas = [5.2, 3.9, 6, 9.75, 7.5, 3];
let arrayNotasSubidas = Array.from(arrayNotas, nota => nota + nota * 0.10); 
```

---

## 4. Objectes Map() i Set()

### A. Map
- El Map és una classe de tipus col·lecció de la qual es poden instanciar objectes que guarden valors en format clau-valor. No hi pot haver més d’un element amb la mateixa clau.

```javascript
const map1 = new Map(); // Mapa buit
map1.set('a', 1);
map1.set('b', 2);
map1.set('c', 3);

console.log(map1.get('a')); // 1 -> valor amb clau 'a'

// actualitzar o afegir valor map.set('key', value)
map1.set('a', 97); // Ja existeix element amb clau 'a', s'actualitza el valor a 97

console.log(map1.get('a')); // 97
console.log(map1.size); // 3 -> mida del map

map1.delete('b'); // Esborrar element amb key 'b'

console.log(map1.has('v')); // false -> no existeix element amb clau 'v'
console.log(map1.size); // 2
console.log(map1.keys()); // a,c -> claus del map
```

- Es pot crear un array de les keys d’un mapa o dels seus valors amb `Array.from()`.

```javascript
let array = Array.from(map1.keys());
console.log(array); // ['a', 'c']
```

### B. Set
- El Set és una classe de tipus col·lecció que guarda valors (com un array) però que **no es poden repetir**.

```javascript
let gadgets = new Set(['movil', 'tablet', 'portatil']);

gadgets.add("reloj");
gadgets.add("tv");

gadgets.delete("reloj");

console.log(gadgets.has("reloj")); // false, s'ha esborrat
console.log(gadgets.size); // 4

gadgets.clear(); // Buida el set
```

- Es pot crear un Set a partir d’un array, però si hi ha elements repetits, al set només n'hi haurà un.