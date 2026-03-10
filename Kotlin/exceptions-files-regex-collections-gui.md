<h1><span style="background-color: #8567d9; color: white; padding: 2px 6px; font-weight: bold; border-radius: 3px; font-family: sans-serif;">KT</span> - Excepcions, Fitxers, String i Regex, Col·leccions i GUI</h1>

## 1. Excepcions



### A. Errors en temps d’execució
- Un error en temps d’execució és un error que salta quan el codi s’està executant i arriba a un punt al que es llança una excepció o error.

### B. Excepcions personalitzades
- Són excepcions creades amb un nom personalitzat com si fossin una subclasse de la classe Exception, li pots passar el missatge que vulguis tant al constructor al moment de cridar l'excepció.
- Creació excepció personalitzada:

```kotlin
class AuditoriaException: Exception {
  constructor(): super("Excepció auditoria")
}
```

- Llançament excepció:

```kotlin
if(x == 1) {
  throw AuditoriaException()
}
```

### C. Try - Catch i mètodes classe Exception
- Per a gestionar excepcions s’utilitza l’estructura try - catch. El try intenta tot el que hi hagi a dins i si en temps d’execució es llança una excepció o error el programa no peta sinó que cau al bloc catch i executa el que hi hagi dins.
- Exemple try - catch per gestió excepcions:

```kotlin
try {
  var num: Int = readln().toInt() // Si no es pot convertir a enter, llença una excepció i el catch la intenta capturar
  println("Has posat un enter")
} catch (e: NumberFormatException) {
  println(e.message)
  println("error, no s'ha introduit un enter")
}
```

- També es poden capturar diferents tipus d’excepcions al catch (pot haver més d’1 catch).

---

## 2. Fitxers

### A. Text pla (.txt)
- Pots guardar text dins d’un fitxer de text per després llegir-lo i poder guardar dades. Per treballar amb un arxiu utilitzes un objecte de tipus Path.
- Creació objecte Path:

```kotlin
import kotlin.io.path.*
val path = Path("./txt/arxiu.txt")
```

- Per llegir fitxers pots fer: `readText()` que retorna string amb tot el contingut o `readLines()` que retorna una List d’strings amb tot el contingut.
- Exemple lectura arxiu:

```kotlin
val path = Path("./txt/arxiu.txt")
val text: String = path.readText()
val lines: List<String> = path.readLines()
```

- Per escriure a fitxer pots fer `writeText("text")` que sobreescriu el que hi ha o `appendText("text")` que escriu a continuació de l'últim caràcter.
- Exemple escriptura arxiu:

```kotlin
var path = Path("./txt/file.txt")
path.writeText("Hello World")
path.appendText("text a continuacio")
```

### B. Binari (.bin)
- Els arxius binaris serveixen per escriure i llegir objectes però utilitzant la classe File i no Path. Les classes per llegir o escriure han de ser de tipus `:Serializable`.
- Lectura arxiu:

```kotlin
fun llegirPersones(): List<Persona> {
  ObjectInputStream(FileInputStream("./files/personatges.bin")).use { ois ->
      return ois.readObject() as List<Persona>
  }
} // retorna una llista dels objectes guardats a l'arxiu   
```

- Escriptura arxiu:

```kotlin
val persona: Persona = Persona("Oriol", 10)
ObjectOutputStream(FileOutputStream("./files/personatges.bin")).use { oos -> 
    oos.writeObject(persona) 
}
```

---

## 3. String i Regex

### A. String
- Un String és un tipus de dada no mutable que no es pot tractar com un array (`string[1] = 'c'`). Per fer això podem utilitzar objectes de la classe StringBuilder per poder manipular-los com a llistes.
- Ús StringBuilder:

```kotlin
val builder: StringBuilder = StringBuilder(strOriginal)
builder[5] = 'R' // Funciona

// Té mètodes propis
// Conversió a string normal
val resultado: String = builder.toString()
```

### B. Regex
- Un Regex és un patró utilitzat per verificar o validar si les dades les compleixen (si una frase té x caràcters o nums etc…). Per utilitzar un regex hem d’utilitzar un objecte de tipus Regex.

```kotlin
var regex: Regex = Regex("[0-9]{3}") // 3 nums del 0 al 9
```

- Per comprovar si un String compleix un regex podem utilitzar el mètode `.matches()`.

```kotlin
string.matches(regex) // retorna true/false
```

### C. Sintaxi Regex

```kotlin
/* CLASSES DE CARÀCTERS */
"[a-z]"       // Una lletra minúscula de la 'a' a la 'z'
"[A-Z]"       // Una lletra majúscula de la 'A' a la 'Z'
"[0-9]"       // Un dígit (del 0 al 9). També es pot escriure com \d
"[a-zA-Z]"    // Qualsevol lletra (majúscula o minúscula)
"[a-zA-Z0-9]" // Alfanumèric (lletres o números)
"."           // Comodí: qualsevol caràcter (lletra, número, símbol...)
"\s"          // Un espai en blanc (espai, tabulador, etc.)

/* QUANTIFICADORS */
"{n}"         // Exactament 'n' vegades. Ex: [0-9]{3} (3 números)
"{n,m}"       // Entre 'n' i 'm' vegades. Ex: [a-z]{2,4} (de 2 a 4 lletres)
"+"           // 1 o més vegades (com a mínim una)
"*"           // 0 o més vegades (opcional, pot no hi ser o estar mil vegades)
"?"           // 0 o 1 vegada (fa que el caràcter sigui opcional)

/* LÍMITS */
"^"           // Indica l'inici del String. Ex: ^A (ha de començar per A)
"$"           // Indica el final del String. Ex: z$ (ha d'acabar en z)
"|"           // Operador "OR" (O lògic). Ex: (hola|adeu)

/* EXEMPLES DE CODI */
"^[0-9]{4}$"          // Exactament 4 números (ni més ni menys)
"^[A-Z][a-z]+$"       // Comença per Majúscula seguida de lletres minúscules (Nom propi)
"^[0-9]{4}[A-Z]{3}$"  // Matrícula: 4 números i 3 lletres majúscules
```

---

## 4. Col·leccions



### A. Classe Collection
- Les col·leccions són estructures de dades per guardar llistes de dades. Poden ser de lectura (mida fixa - no pots afegir o eliminar elements) o mutables (mida variable).
- Les col·leccions tenen uns mètodes heretats de Collection:

```kotlin
list.size
set.contains("element")
list.joinToString() // passa a string
list.isEmpty() // true/false si està buida o no
```

- Les col·leccions mutables tenen uns altres mètodes heretats de Mutable:

```kotlin
mutableList.clear()
mutableSet.add("element")
mutableSet.remove("element")
mutableList.removeAt(2) // elimina element a l'índex 2 
```

### B. List
- La List o MutableList és una col·lecció indexada similar a l’array que guarda les dades per ordre d’inserció.

### C. Set
- El Set o MutableSet és una col·lecció indexada d’elements únics (no hi poden haver valors repetits).

### D. Map
- El Map és una col·lecció que emmagatzema parelles clau-valor. Les claus han de ser úniques.

### E. Queue, Stack, PriorityQueue
- El Queue és una col·lecció de valors que entren i surten en ordre (FIFO).
- L’Stack és una col·lecció de valors que entren i surten en ordre invers (LIFO).
- La PriorityQueue és una cua de valors que entren i surten segons la prioritat que hagis configurat.
- Mètodes - També els heretats de collection:

```kotlin
queue.poll() // retorna i elimina l'element que ha de sortir
queue.peek() // retorna l'element que ha de sortir i no elimina
```

---

## 5. GUI - JavaFX API