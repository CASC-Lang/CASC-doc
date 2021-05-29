# CASC Lang Document  
## Introduction

CASC is a programming language focus on concise syntaxes and enhancing language features from Java. It is mostly inspired by Rust / Go Lang / V Lang, and influenced by Kotlin. CASC features:
- Fully interoperability to Java
- Shorthand syntaxes
- Rust / Go Lang / V Lang inspired syntaxes
- Better class form

CASC uses .casc and .cas as source file extension name.

## Installation

For 0.1, please heads to CASC github repository's release for more infomation.

## Compiler Usage

```cmd
java -jar CASC-0.1.jar (-cp "classpath1;" "classpath2") (-o "C:/outputDirectory/") <File / Folder to be compiled>
```

If the last argument is a CASC source file, then it'll only queue the only file and will not try to compile other related CASC source files, but if it is a folder, then compiler will queue all the files in subdirectories into compilation.

## Table Of Contents

<table>
    <tr><td valign=top>

* [Hello World](#hello-world)
* [Types](#types)
* [Function](#function)
* [Access Modifiers](#access-modifiers)
* [Variable Declaration](#variable-declaration)
* [Assignment](#assignment)
* [Array](#array)
* [List](#list)
* [Module](#module)
* [Module Import](#module-import)
    
    </td><td valign=top>

* [Statements](#statements)
    * [If Else](#if-else)
    * [For Loop](#for-loop)
        * [Detailed For Loop](#detailed-for-loop)
        * [Ranged For Loop](#ranged-for-loop)
* [Class](#class)
    * [Default Constructor](#class-with-default-constructor)
    * [Primary Constructor](#class-with-primary-constructor)
    * [Primary & Secondary Constructor](#class-with-both-primary--secondary-constructor)
* [Field](#field)
    * [Field declaration block](#field-declaration-block)
    * [Companion field](#companion-field)
* [Variable property access](#variable-property-access)

    </td></tr>
</table>

## Hello World

> This is 0.1 version's example of Hello World.

To get start with any languages, hello world is the best demonstration to show a language's features. The following code is CASC's basic Hello World application:

```v
class CASC {
    comp fn main(args: str[]) {
        println("Hello World!")
    }
}
```

Let's take a look at the code above: First, `class` keyword declares a public class in default, it's necessary for CASC 0.1 to compile into class file. Second, similar to Java's main function, you'll need to declare main function, to declare it a static function, put `comp` keyword in front of the `fn` keyword to do it. Since we still haven't implemented a easier way to declare argument-less main function, you'll need to add `args: str[]` into arguments in order to let JRE identify it as the entry function.

For CASC 0.1, we have `print` and `println` built-in functions, you can directly use it without any problem.

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

## Function

Similar to Kotlin's (or V Lang) function, you declares a function like below:

```v
fn add(a: i32, b: i32): i32 {
    return a + b
}
```

To declare it as a companion function (same as static function in Java), you will need to put `comp` keyword before `fn` keyword:

```v
comp fn add(a: i32, b: i32): i32 {
    return a + b
}
```

In addition, if you have type to return, you can optionally discard `return` keyword:

```v
fn add(a: i32, b: i32): i32 {
    a + b // OK
}
```

To declare a void function, you can simply change return type to `unit` or just don't add return type after a function declaration:

```v
fn sayHi(): unit {
    println("Hi")
}
```

```
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

In default, every classes, fields, and functions without access modifiers are public.

> In CASC 0.1, accessability checking is still not implemented.

## Variable Declaration

All variables in default are declared as immutable variable:

```v
a := 1
```

To declare mutable variables, add `mut` keyword at the beginning of the variable declaration:

```v
mut b := 1
```

At this moment, we don't support variable declaration, to declare a i64 and f64 (since integer values are i32 and float values are f32 in default,) you can add `L` (or `l`) after integer values and `D` (or `d`) after float values:

```v
c := 1L

d := 1.0D
```

> This section is about CASC 0.1 limitation

As previous mentioned, CASC does not have type declaration, so types are automatically detected by CASC compiler, so for most object cases, for instance, declares a variable with type `java::lang::Integer` would just create a variable with type `java::lang::Integer`, and for this moment, no casting is available, and since compiler would not perform auto casting even it's possible to cast, which means if a function accepts `Comparator<T>`, you are not able to pass `java::lang::Integer` in.

## Assignment

You can only assign objects to mutable variables or fields:

```v
mut a := 1
a = 2 // OK
```

```v
b := 1
b = 2 // Error
```

Interesting aspect, you can reassign a variable (except for field) with another type as long as it is mutable:

```v
mut c := 1
c = java::lang::Integer(2) // Ok, now variable c's type is java::lang::Integer
```

## Array

Array in CASC is same as Java's, you can create an array by either initializing or allocating:

```v
a := i32:[10] // Allocate an i32 array holds up to 10 i32 values.
b := { 1 } // Dynamically initialize an i32 array holds up 1 i32 value, and assign 1 to its index 0
```

To declare a multidimensional array, you would like to do like below:

```v
c := i32:[10][10]
d := { { 1, 2 }, i32:[10] }
```

you can simply reference a value in an array by indexing array:

```v
e := c[2] // i32[]
f := c[3][0] // i32
g := (i32:[10])[0] // i32, though it's meaningless consider it is a constant value before assign with anotehr value.
```

## List

> In CASC 0.1, not implemented a DSL for initializing array.

## Module

CASC has same functionality as Java's package system calls module system, though it does not have significant differences. To organize files with module system, you would add `mod` keyword and come up with a qualified module path at the beginning of CASC source file than any other things:

```rs
mod website::template

class Slide {
    // ...
}
```

## Module Import

We do have same functionality  as Java's import, but in CASC, we import modules with `use` keyword:

```rust
use java::lang::Integer

class Math {
    fn add(a: Integer, b: Integer): i32 {
        a.intValue() + b.intValue()
    }
}
```

> In CASC 0.1, we haven't implemented wildcard import.

> In future CASC versions, we will as well provides inner-function module import functionality.

## Statements
### If Else

```v
if a > b {
    // ...
} else if a == b {
    // ...
} else {
    // ...
}
```

You can add parentheses before and after the expression as well, just for readability enhancement for group of users.

### For Loop
#### Detailed For Loop

This for loop is same as Java's original loop:

```rs
for mut i := 1; i < 20; i = i + 1 {
    // ...
}
```

> In CASC 0.1, we still don't have increment statement.

If you want to declare a infinite for loop, you can either do:

```rs
for ;; {
    // ...
}
```

Or just discard semicolons:

```rs
for {

}
```

#### Ranged For Loop

CASC does provide a concise way to declare a for loop inside a range.

```v
for i : 1 -> 10 {
    println(i)
}
```

It will print out:

```json
1
2
3
4
5
6
7
8
9
10
```

We call the for loop above as `to-forward for loop`.

If you don't want for loop iterate to 10, you would want to replace `->` to `|>`:

```v
for i : 1 |> 10 {
    println(i)
}
```

It will print out:

```json
1
2
3
4
5
6
7
8
9
```

We call the for loop above as `until-forward for loop`.

To reverse it, you need to put large number on the other side and vice versa. In addition, replace `->` with `<-`, or `|>` with `<|`.

```v
for i : 1 <- 10 {
    println(i)
}
```

```v
for i : 1 <| 10 {
    println(i)
}
```

It's worth noting that forward for loop takes the left side expression to increase, and reverse for loop takes the right side expression to decrease.

> In future CASC versions, we will provide step functionality if you want to increase or decrease number with specified number.

## Class

```kt
class Person {

}
```

Class itself can be applied with access modifiers:

```kt
priv class Person {

}
```

### Class with Default Constructor

The class in the following code will generate a constructor without parameters.

```kt
class Person {
    // ...
}
```

### Class with Primary Constructor

All classes should have a primary constructor than having secondary constructors only, also, constructor parameters may place `param` keyword before a parameter:

```kt
class Person(param age: i32, param name: str) {
    ctor() {
        self(18, "ChAoS")
    }
}
```

The above example will emit two-parameters constructor, if your constructor want to store parameters as class fields, you would like to declare it as below:

```kt
class Person(priv mut age: i32, name: str, param parents: Person[]) {

} 
```

The above example will emit three-parameters constructor, and will store `age` and `name` as class fields.

### Class with both Primary & Secondary Constructor

Secondary (or auxiliary) constructor should call either primary constructor or a secondary constructor that also calls primary constructor as well, which means in CASC we don't support constructor overloading. To declare a secondary constructor, put `ctor` keyword before a set of arguments:

> The following example will replace in future CASC versions.

```kt
class Person {
    ctor(age: i32, name: str) { // Error, no primary constructor 
        
    }
}
```

Need to mention is that you cannot have field parameter in secondary constructors.

Constructor can as well apply access modifiers before it:

```kt
class Person priv (priv mut age: i32, name: str) {
    prot ctor() {
        super(18, "ChAoS")
    }
}
```

## Field

```kt
class Person {
    priv mut age: i32
    name: str
}
```

### Field declaration block

If you have multiple fields with same access modifiers and mutability, you can declare them in a single field declaration block:

```kt
class Person {
    name: str
    priv mut:
        age: i32
        // other fields...
}
```

Due to compiler's limitation, the field declaration block does not recognize the indent, so consider the code:

```kt
class Person {
    priv mut:
        age: i32
    name: str
}
```

Both field `age` and `name` are private and mutable.

#### Companion field

```kt
class Person {
    comp:
        genericName: str
}
```

> In CASC 0.1, we haven't implement field assignment.

### Variable property access

```v
a := { 1 }
a[0] = 1
```

If it's a object and it has a property calls `b`, you can access it like below:

```v
obj.b = 10
```

