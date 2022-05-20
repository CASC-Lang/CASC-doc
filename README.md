# CASC Lang Document  
## Introduction

> Notice that this documentation is a very brief introduction to CASC programming language's syntax and several 
> surface-level language \ specification, which expects that you have some experience with Rust's, Go's, V's,
> Java's, or Kotlin's syntax.

CASC is a programming language focus on concise syntaxes and enhancing language features from Java. It is mostly inspired by Rust / Go Lang / V Lang, and influenced by Kotlin. CASC features:
- Fully interoperability to Java
- Shorthand syntaxes
- Rust / Go Lang / V Lang inspired syntaxes
- Better class form organization

CASC uses .casc and .cas as source file extension name.

## Installation

For 0.1, please heads to CASC github repository's release for more infomation.

## Compiler Usage

- Compile
```cmd
java -jar <path to CASC compiler jar file> [-o "C:/outputDirectory/"] <source file / project source folder> [-t]
```
> Available parameters \
> Enable timing: -t \
> Shows all compiler unit's progressing total time
- Compile and Run
```cmd
java -jar <path to CASC compiler jar file> run [-o <output directory>] <source file> [-t]
```
> Available parameters \
> Enable timing: -t \
> Shows all compiler unit's progressing total time

## Table Of Contents

<table style="width: 100%">
    <colgroup>
      <col style="width: 33%" />
      <col style="width: 33%" />
      <col style="width: 33%" />
    </colgroup>
<tr width=100%><td display=table-cell 
text-align=center>

* [Hello World](#hello-world)
* [Types](#types)
* [Function](#function)
* [Access Modifiers](#access-modifiers)
* [Variable Declaration](#variable-declaration)
* [Promotion](#promotion)
* [Assignment](#assignment)
* [Array](#array)
* [Package](#package)
* [Use](#use)

</td>
<td display=table-cell text-align=center>
  
* [Statements](#statements)
  * [If-Else Statement](#if-else-statement)
  * [Java-Style For Loop](#java-like-for-loop-statement)
* [Expressions](#expressions)
  * [String Literal / Char Literal](#string-literal--char-literal)
    * [Escapable Character](#escapable-characters)
  * [Integer Literal / Float Literal](#integer-literal--float-literal)
  * [Bool Literal](#bool-literal)
  * [Null Literal](#null-literal)
  * [Operators](#operators)
  * [If-Else Expression](#if-else-expression)
  * [Assignment Expression](#assignment-expression)
  * [Constructor expression](#constructor-expression)
  
</td>
<td display=table-cell text-align=center>

* [Class](#class)
  * [Accessors & Mutability](#accessors--mutability)
  * [Fields](#fields)
  * [Inheritance](#inheritance)
* [Impl](#impl)
  * [Function Declaration](#function-declaration)
  * [Constructor Declaration](#constructor-declaration)
  * [Inheritance (Class)](#inheritance-class)

</td>
 
</tr>
</table>

## Hello World

> This is 0.1.0 version's example of Hello World.

To get start with any languages, hello world is the best demonstration to show a language's features. The following code is CASC's basic Hello World application:

```rust
class CASC

impl CASC {
  fn main(args: [str]) {
    java::lang::System.out.println("Hello World!")
  }
}
```

Let's take a look at the code above: First, `class` keyword declares a public class in default, 
it's necessary for CASC 0.1.0 to compile into class file. Second, similar to Java's main function, 
you'll need to declare main function. Since we still haven't implemented an easier way to declare argument-less 
main function, you'll need to add `args: [str]` into arguments in order to let JRE identify it as the entry function.

### Types

| CASC Primitive Types  | Java Primitive Types  |
|-----------------------|-----------------------|
| bool                  | boolean               |
| char                  | char                  |
| i8                    | byte                  |
| i16                   | short                 |
| i32                   | int                   |
| i64                   | long                  |
| f32                   | float                 |
| f64                   | double                |
| str                   | java.lang.String      |
| unit                  | void                  |
| null                  | null                  |

Notice that `null` is not an actual type in CASC, it's just a placeholder for typing, which means you cannot declare `null`
as a real type.

## Function

> To actually declare functions, you'll have to declare it in [Impl](#impl) block.

Similar to Kotlin's (or V Lang) function, you declare a function like below:

```rust
fn add(a: i32, b: i32): i32 {
    return a + b
}
```

To declare it as a companion function (same as static function in Java), simply declare like below:

```rust
fn add(a: i32, b: i32): i32 {
    return a + b
}
```

To declare it as a member function, you'll need to add keyword `self` before all the other parameters:

```rust
fn add(self, a: i32, b: i32): i32 {
    return self.manipulateAddition(a, b)
}
```

In addition, if you have type to return, **CASC doesn't allow you to return last expression without having `return` keyword**:

```rust
fn add(a: i32, b: i32): i32 {
    return a + b
}
```

To declare a void function, you can simply change return type to `unit` or just don't add return type after a function declaration:

```rust
fn sayHi(): unit {
    println("Hi")
}
```

```rust
fn sayHi() {
    println("Hi")
}
```

## Access Modifiers

Access modifiers could be applied on classes, fields, and functions.

| CASC Access Modifiers | Java Access Modifiers     |
|-----------------------|---------------------------|
| pub                   | public                    |
| intl                  | (default access modifier) |
| prot                  | protected                 |
| priv                  | private                   |

In default, every class, fields, and functions without access modifiers are public.

## Variable Declaration

Variable declaration is very straightforward in CASC, unlike Java, CASC doesn't need you to annotate variable's type
while declaring:

```v
a := 1 // Varible a stores i32 data
```

To make it mutable, add `mut` keyword before the variable name:

```v
mut a := 1 // Variable a is now able to be assigned with other i32 data
```

Notice that CASC doesn't allow you to declare a variable with annotating a type, in other word, CASC always infers the
right-hand expression's type, this can be done through a set of promotion rules, see [Promotion](#promotion)

## Promotion

(See [Types](#types) for more information)

Primitive number types can be promoted into another primitive number type **only if they won't lose precision of their 
current data**:

```rust
              i8 -> i16 -> i32 -> i64 -> f32 -> f64
              ⇕     ⇑      ⇑      ⇑      ⇑      ⇑
java::lang::Byte    ⇓      ║      ║      ║      ║
      java::lang::Short    ⇓      ║      ║      ║
            java::lang::Integer   ⇓      ║      ║
                      java::lang::Long   ⇓      ║
                            java::lang::Float   ⇓
                                  java::lang::Double
```

Promotion is automatically done by compiler, if your losing precision is expected behavior, see [Casting (WIP)](#casting).

## Assignment

Assignment expression allows you to assign to various variable within a single expression:

```rust
a = b = 1 // Ok
```

Assignment expression will retain the latest value if necessary.

However, you should notice that assignment will also trigger [Promotion](#promotion) when left-hand expression is narrower
data type than right-hand's:

```rust
mut a := 1I // a stores i32 data: 1
mut b := 2F // b stores f32 data: 2.0

// Err, cannot assign a wider data type to narrow data type, (f32 -> i32)
a = b = 1
// the execution order is (b = 1) then (a = b), at first assignment, 1 has been promoted as f32, so at second assginment
// it would fail.

// Ok
b = a = 1
```

## Array

Array in CASC has different similarity aspects compared to Rust, C# or Java.

Array type is very similar to Rust's, however since JVM doesn't force parameter array to have specific size, therefore,
it's still kinda different from Rust's:

```rust
[str]     -> 1D string array
[[str]]   -> 2D string array
// and so on...
```

But array declaration / initialization has mixed style with Rust's, C#'s and Java's:

```rust
[i32; 10]{}       // Array declaration
[i32; cap]{}      // Array declaration with varaible-determined size
[;]{ 1, 2, 3 }    // Array initialization
[i32;]{ 1, 2, 3 } // Array initialization with sepcifying array type
```

Notice that you must have the top dimension declare with a size, otherwise, CASC couldn't know how many array elements 
allocate.

```rust
[i32;]{} // Err, missing dimension at the top dimension declaration
```

Furthermore, you can skip declaring other sub-dimensional arrays with size, which means this array will be a jagged array.

```rust
[[i32;];10]{}
// Equivalent to Java's:
// new int[10][];
```

## Package

There's nothing special about package, it just works like Java:

```java
package your::package::here;
```

## Use

`use` keyword is very similar to Java's import, but its syntax is much similar to Rust's:

```rust
use other::package::path
use other::{ package, package::path }
use other::pacakge::path as p
```

Notice that CASC doesn't support `companion use` at this moment, similar to Java's `static import`.

## Statements

### If-Else Statement

(Notice that CASC also has [if-else expression](#if-else-expression))

If-else statement allows you conditionally execute code through multiple branches:

```rust
if i > 1 {

} else if i > 2 {

} else {

}
```

### Java-like For Loop Statement

CASC supports Java-favored for loop, though the limitation is that you can only have 1 statement in pre- / post-init 
section:

```v
for mut i := 0; i < 10; i++ {

}
```

## Expressions

### String Literal / Char Literal

You can declare string literal with `""`:

```rust
"This is a string!"
```

Same for character literal (`''`):

```rust
'a'
```

To escape a character, put `\` before the character you want to escape:

```rust
"\t\r"
```

#### Escapable characters

The following list shows the possible escaping characters:

| Escaped character | Name                     |
|-------------------|--------------------------|
| \t                | Tab                      |
| \\\\              | Back slash               |
| \r                | Carriage return          |
| \n                | Newline                  |
| \b                | Backward                 |
| \u09AF            | 16-bit unicode character |

### Integer Literal / Float Literal

```rust
1       // Integer Literal
2.0     // Float Literal
```

Besides, the normal literal, CASC also supports type annotated integer / float literal:

```rust
1B      // Integer Literal (i8)
1S      // Integer Literal (i16)
1I      // Integer Literal (i32)
1L      // Integer Literal (i64)
1F      // Float Literal (f32)
1D      // Float Literal (f64) 
```

### Bool Literal

```rust
true
false
```

### Null Literal

```java
null
```

### Operators

Operators in CASC is as same as Java's, including their precedence, see 
[Java's operator tutorial page](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/operators.html) for more information.

> Yeah, I'm quite lazy here to list all operators :P, I'll list it later

### If-Else Expression

If-else expression is very similar to Java's and C#'s conditional expression, or Rust's and V's if-else expression.
If-Else expression works mostly like [If-Else Statement](#if-else-statement), except in most scenarios, it expects having
an expression returns (or retain) value:

```rust
a := if i > 1 {
    1
} else {
    2
}
```

If-else expression will also perform [promotion](#promotion) if the value types at the end of branches is different.

### Assignment expression

See [Assignment](#assignment).

### Constructor expression

> aka constructor invocation

Same as Java's constructor expression (or invocation):

```java
new ClassName(/* parameters */)
```

## Class

Class is just like struct from various languages, it's meant to hold data and various function. But unlike struct, class in 
modern OOP languages have more extended functionalities, such as member functions.  
To declare a class in CASC, you have two ways:

- No scope
```rust
class ClassName
```
- With scope
```rust
class ClassName {

}
```

Notice that `no scope` cannot be used while there's some field declarations, to declare fields, you'll have to use 
`with scope`.

### Accessors & Mutability

Class could be applied with [access modifiers](#access-modifiers) and `mut` keyword:

```rust
pub mut class ClassName
```

`mut` keyword is used for inheritance situation: in CASC, all declarations are defaulted as non-mutable, in order to make
child class able to [override](#override) class, you'll have to add `mut` keyword before `class` keyword.

### Fields

CASC introduces a simple (or more neat) way to declare fields, inspired by V's (and Go's) field syntax:

```rust
class ClassName {
// start with empty accessors or `pub:`
    pubFinalField: i32
mut:
// or `pub mut:`
    pubMutableField: i32
prot:
    protFinalField: i32
prot mut:
    protMutableField: i32
intl:
    intlFinalField: i32
intl mut:
    intlMutableField: i32
priv:
    privFinalField: i32
priv mut:
    privMutableField: i32
}
```

To declare a set of companion fields, wrap it in a `comp` block:

```rust
class ClassName {
    comp {
        // Your fields here
    }
}
```

Notice that you cannot wrap it more than one times or have more than one `comp` blocks, otherwise, compiler will panic about this.

### Inheritance

See [Inheritance (Class)](#inheritance-class)

## Impl

Impl, or implementation, is a workaround for Java's, or Kotlin's function organization issue, its syntax is inspired by Rust's,
`impl` keyword can split owner functions and implemented interface function implementations, so you can straightforwardly know which functions
belong to which interface. This also overhauls the tidiness of a class implementations.

Same as [Class](#class), you have `no scope` and `with scope` options to declare an implementation block, the limitations are same.
But implementation block is actually optional, you can declare a class without it.

```rust
impl ClassName {
  
}
```

### Function Declaration

See [Function](#function) for more basics.

Function declaration also accept [access modifiers](#access-modifiers) and `mut` keywords.

### Constructor Declaration

CASC introduce a concise way to declare a constructor:

```rust
new(/* parameters */) {

}
```

The above example declares a constructor, notice that if your class inherit other class, you'll have to declare a constructor 
with a `self` keyword to call parent class' constructor, and all the other constructors in same context should have `super` 
keyword to call constructor with `self` keyword:

```rust
impl ClassName: ParentClass {
    // This calls parent class' `new()`
    new(): self() {

    }

    // This calls `new(): self()`
    new(i: i32): super() {

    }
}
```

> See [Inheritance (Class)](#inheritance-class) for more information.

### Inheritance (Class)

Like other OOP languages, CASC has inheritance!

```rust
impl ClassName: ParentClass {

}
```
