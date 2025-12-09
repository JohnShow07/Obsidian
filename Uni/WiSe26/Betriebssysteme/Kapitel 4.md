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
