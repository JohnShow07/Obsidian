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
