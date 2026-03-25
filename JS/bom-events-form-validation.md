<h1><span style="background-color: #F7DF1E; color: black; padding: 2px 6px; font-weight: bold; border-radius: 3px; font-family: sans-serif;">JS</span> - BOM, Events, Formularis</h1>

## 1. BOM - Browser Object Model

### A. Objecte Window
- Representa la finestra completa del navegador i es pot usar per moure, redimensionar i manipular la finestra del navegador.
- Propietats i mètodes:

```javascript
window.name // nom window actual
window.status // valor barra d'estat
window.screenX / window.screenY // distancia finestra a la cantonada esquerra/superior de la pantalla

window.outerWidth / window.outerHeight // alt o ample total de la finestra amb toolbar i scrollbar
window.innerWidth / window.innerHeight // alt o ample total de la finestra sense toolbar i scrollbar

window.pageX / window.pageY // distancia a la pagina (scroll)

// mètodes
.back() .forward() .home() .stop() .blur() // ...
```

- Es poden obrir noves finestres amb el mètode `.open(url, nom, options)`. 
- Llista d’opcions (`option=value`): `toolbar`, `location`, `directories`, `status`, `menubar`, `scrollbar`, `resizable`, `width`, `height`, `left`, `top`.
- També el `.open` té els següents mètodes:

```javascript
let myWindow = window.open("about:blank", "miWindow", "height=400,scrollbar=no");

myWindow.close() // tanca finestra
myWindow.moveTo(x,y) // mou a les coordenades indicades
myWindow.moveBy(x,y) // mou els píxels indicats
myWindow.resizeTo(x,y) // dona proporcions donades
myWindow.resizeBy(x,y) // afegeix aquell ample/alt
```

### B. window.location
- És l’objecte que conté info sobre la URL actual, disposa d’aquestes propietats i mètodes:

```javascript
window.location.href // retorna URL actual completa
window.location.href = "youtube.com" // asigna URL actual
window.location.protocol/hostname/pathname/search/hash // retorna parts de la URL actual
window.location.reload() // recarrega la pàgina
window.location.assign(url) // carga la pàgina passada per paràmetre
window.location.replace(url) // carga la pàgina passada per paràmetre sense guardar l'actual a l'historial
```

### C. window.history
- És l’objecte que conté info sobre l’historial i ens permet accedir a ell:

```javascript
window.history.length // número de pàgines guardades a l'historial (fletxes)
window.history.back() // va a la pàgina anterior
window.history.forward() // va a la pàgina següent
window.history.go(num) // es mou num vegades cap a endavant o endarrere si és negatiu
```

---

## 2. Events

- Els events són la manera que es té per controlar accions d’usuari i definir comportaments quan es produeixi algun.

### A. Tipus d’events

| EVENT | HANDLER | DESC |
| :--- | :--- | :--- |
| `click` | `onclick` | al hacer clic |
| `dblclick` | `ondblclick` | al hacer doble clic |
| `mouseover` | `onmouseover` | al poner el ratón encima |
| `mouseout` | `onmouseout` | al quitar el ratón de encima |
| `keydown` | `onkeydown` | al presionar una tecla |
| `keyup` | `onkeyup` | al soltar una tecla |
| `focus` | `onfocus` | al entrar o seleccionar un campo (inputs) |
| `blur` | `onblur` | al salir o deseleccionar un campo (inputs) |
| `change` | `onchange` | al cambiar un valor (inputs, selects) |
| `submit` | `onsubmit` | al enviar un formulario |

- Els *handlers* s’utilitzen definint un atribut a l’element del DOM; els "events" s'utilitzen dins d’un `eventListener`.
- Es poden esborrar eventListeners amb `removeEventListener(event, function)`.
- Quan a un `eventListener` li passes una funció, l’has de cridar **sense parèntesis** perquè l'executi quan hi hagi l’event i no al moment:

```javascript
btn.addEventListener('click', saludar)
```

### B. Object Event
- Es pot accedir al propi event des del listener amb un paràmetre per defecte i accedir a propietats de l’event que ha succeït:

```javascript
btn.addEventListener('click', (event) => {
    event.target // element exacte que rep l'acció
    event.type // nom de l'event
    event.clientX / event.clientY // coords dins la part visible del navegador
    event.pageX / event.pageY // coords X/Y de tot el document 
    event.screenX / event.screenY // coords absolutes del teu monitor físic
    event.offsetX / event.offsetY // coords internes des de la cantonada del mateix element
    event.movementX / event.movementY // píxels que s'ha mogut el mouse des de l'últim event
    event.relatedTarget // element secundari implicat
    event.ctrlKey // true si prems Control
    event.shiftKey // true si prems Shift
    event.altKey // true si prems Alt
    event.metaKey // true si prems la tecla Windows/Command
    event.key // nom de la tecla premuda
    event.preventDefault() // anul·la l'acció per defecte
    event.stopPropagation() // evita que l'event salti als pares
});
```

### C. Propagation
- La propagació és el procés pel qual un event activat en un element específic "puja" a través del DOM, transmetent-se i activant en cadena els seus elements pares. (Es pot aturar amb `event.stopPropagation()`).

### D. Timers
- A JS poden haver-hi cronòmetres i codi que s’executa cada X temps amb `setTimeout` o `setInterval`.

- **Timeout:**
```javascript
const miTemporizador = setTimeout(() => {
    console.log("han passat 3 segons"); // només s'executa una vegada
}, 3000);
```

- **Interval:**
```javascript
let contador = 0;
const miIntervalo = setInterval(() => {
    contador++;
    console.log("Repetició " + contador); // cada 2 segons
    if (contador === 5) {
        clearInterval(miIntervalo); // Es para l'interval
    }
}, 2000);
```

---

## 3. Validació formularis

### A. Validació HTML
- Els `<input>` poden tindre atributs per automatitzar la validació, es fa una vegada s’envia el formulari i al trobar un error es mostra i evita el submit:

```html
<input type="text" required> <input type="text" pattern="[0-9]"> <input type="text" minlength="3" maxlength="10"> <input type="number" max="5" min="3"> ```

### B. Regex
- Es poden utilitzar regex per validar valors directament amb JS.

```javascript
let value = "Hola";
let regex = /[a-zA-Z]/;
regex.test(value); // retorna true o false (valida o no)
```

### C. API Validation
- La validació via API és una manera d’estalviar validacions manuals amb mètodes dels HTMLElements de JS (`getElementById`).

```javascript
const input = document.getElementById('input');

input.checkValidity() // retorna true/false si és vàlid o no
input.reportValidity() // mostra la bafarada d'error nativa del navegador
input.setCustomValidity("missatge") // defineix un error personalitzat (bloqueja l'enviament)

// Propietats de validity:
input.validity.valueMissing // true si el camp 'required' està buit
input.validity.typeMismatch // true si el format no coincideix amb el 'type'
input.validity.patternMismatch // true si no compleix l'atribut 'pattern' (regex)
input.validity.tooShort / input.validity.tooLong // true si no compleix minlength o maxlength
input.validity.rangeUnderflow / input.validity.rangeOverflow // true si no compleix l'atribut min o max
input.validity.stepMismatch // true si no compleix l'atribut 'step'
input.validity.customError // true si s'ha marcat un error amb setCustomValidity
input.validity.valid // true si el camp és totalment vàlid
```