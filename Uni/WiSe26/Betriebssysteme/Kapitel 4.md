#BSysteme #Kapitel4
# Rust (Part1)
### Primitive types


| signed         | **i8, i16, i32, i64, i128**                                  |
| -------------- | ------------------------------------------------------------ |
| unsigned       | u8, u16, u32, u64, u128                                      |
| Floating point | f32(einfach genau), f64(standart, sehr genau)                |
| Boolean        |                                                              |
| Char           |                                                              |
| String-Literal | kein String, sondern ein Zeiger auf Text im Programmspeicher |

```rust
let s1: &str = "Hallo";             // String Slice
let s2: String = "Hallo".to_string(); // richtiger String
```

### Compound types

>**Tupel**
```rust
let my_tuple: (i32, char, bool) = (162, 'X', true);

//Zugriff:
my_tuple.0
my_tuple.1
my_tuple.2

```
>**Array**
```rust
let nums: [i32; 5] = [1, 2, 3, 4, 5];
let second = nums[1];

```
- IMMER feste Größe
>**Structs**
- Klasse ohne Methoden
	- lasst mehrere Daten unter einem Namen zusammen
```rust
struct Coordinate {
    x: i32,
    y: i32,
}

let point = Coordinate { x: 5, y: 3 };

//Ein struct beenutzen:
let point = Coordinate { x: 5, y: 3 };

println!("{}", point.x);
println!("{}", point.y);

```

### Functions

>**Fnc mit Rückgabewert**
```rust
fn square(x: i32) -> i32 {
    return x * x;
}

//Auch als "implizites return" -> Keine Semicolon am ende der Rückgabewert:

fn square(x: i32) -> i32 {
    x * x   // kein Semikolon → Rückgabewert
}

```

>**Fnc ohne Rückgabewert**
```rust
fn add_one(x: &mut i32) {
    *x += 1;
}

```
- Rückgabetyp ist = () (spricht man: „Unit“, _Null_ in Java). _> Oft nicht geschrieben_
- `&mut i32` bedeutet: Referenz auf eine veränderbare i32

#### Hello World & Macros

```rust
fn main() {
	println!("Hello World!");
}
```
- `println!` ist kein Funktionsaufruf, sondern ein Macro
- Rust-Macros(Compile-Time Code-Generator->erzeugt zeitgleich ausführbarer Code) generieren zur Compile-Zeit zusätzlichen Code

### If Statements
- Keine Klammern
```rust
let x = 4;

if x > 5 {
    println!("Greater than 5");
} else if x > 3 {
    println!("Greater than 3");
} else {
    println!("x was not big enough");
}
```
- Kann Werte erzeugen
```rust
let x = 4;

let message = if x > 5 {
    "Greater than 5"
} else if x > 3 {
    "Greater than 3"
} else {
    "x was not big enough"
};
```

### Loop
- Undendliche Schleife
```rust
loop {
    println!("stuck in a loop");
}
// end with "break"
loop {
    break;
}
// Wert Rückgabe möglich
let mut count = 0;

let three = loop {
    count += 1;
    if count >= 3 {
        break count;  // <--- DAS GIBT DIESES ERGEBNIS ZURÜCK!
    }
};
```

### While

```rust
let mut count = 5;

while count > 0 {
    count -= 1;
}
```

**Unterschiede zu loop**:

|loop|while|
|---|---|
|läuft unendlich|läuft, solange Bedingung true ist|
|braucht `break`|stoppt automatisch|
|kann Wert zurückgeben|gibt keinen Wert zurück|

### `for` - Schleifen
- Iteriert immer über eine `Collection` oder `Range`.
```rust
let nums = [0, 1, 2, 3, 4];

for n in nums {
    println!("{}", n); // Ausgabe: `0 1 2 3 4` 
}
```
- Range(Zahlenbereich)
```rust
for n in 0..5 {
    println!("{}", n); // Ausgabe: `0 1 2 3 4` 
}
```

### usize / isize

| Typ           | Signed?   | Größe hängt von Architektur ab?            | Typische Verwendung                         | Beispiel                    | Bemerkung                            |
| ------------- | --------- | ------------------------------------------ | ------------------------------------------- | --------------------------- | ------------------------------------ |
| **usize**     | ❌ Nein    | ✔️ Ja (32-bit → 32 bits, 64-bit → 64 bits) | **Array-Indizes**, Speichergrößen (`len()`) | `let n: usize = arr.len();` | kann keine negativen Werte speichern |
| **isize**     | ✔️ Ja     | ✔️ Ja                                      | Zeigerarithmetik, selten gebraucht          | `let x: isize = -5;`        | signed-Version von usize             |
| **u32 / i32** | teilweise | ❌ Nein (immer 32 bit)                      | normale Zahlen                              | `let a: i32 = -3;`          | Architektur-unabhängig               |
| **u64 / i64** | teilweise | ❌ Nein (immer 64 bit)                      | sehr große Zahlen                           | `let b: u64 = 5000;`        | Architektur-unabhängig               |
- Warum so wichtig?

| Situation                    | Erwarteter Typ |
| ---------------------------- | -------------- |
| Array-Index                  | **usize**      |
| Array-Länge (`arr.len()`)    | **usize**      |
| Range (z. B. `0..arr.len()`) | **usize**      |
| Speichergröße                | **usize**      |

- Bsp:
```rust
let arr = [10, 20, 30];

for i in 0..arr.len() {   // arr.len() ist usize
    println!("{}", arr[i]);  
}
```
**Mini-Merksatz:**
- Alles, was mit Array-Längen und Speicher zu tun hat, ist `usize`.  
- `isize` ist das gleiche, aber signed.

### Enums
- erlauben, einen Typ zu definieren, der mehrere mögliche Varianten haben ka