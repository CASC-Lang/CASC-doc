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
    <tr><td width=33% valign=top>

* [Hello World](#hello-world)
* [Types](#types)
* [Function](#function)
* [Access Modifiers](#access-modifiers)
* [Variable Declaration](#variable-declaration)
* [Assignment](#assignment)
    
    </td></tr>
</table>

## Hello World

> This is 0.1 version's example of Hello World.

To get start with any languages, hello world is the best demonstration to show a language's features. The following code is CASC's basic Hello World application:

```casc
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

```casc
fn add(a: i32, b: i32): i32 {
    return a + b
}
```

To declare it as a companion function (same as static function in Java), you will need to put `comp` keyword before `fn` keyword:

```casc
comp fn add(a: i32, b: i32): i32 {
    return a + b
}
```

In addition, if you have type to return, you can optionally discard `return` keyword:

```casc
fn add(a: i32, b: i32): i32 {
    a + b // OK
}
```

To declare a void function, you can simply change return type to `unit` or just don't add return type after a function declaration:

```casc
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

```casc
a := 1
```

To declare mutable variables, add `mut` keyword at the beginning of the variable declaration:

```casc
mut b := 1
```

At this moment, we don't support variable declaration, to declare a i64 and f64 (since integer values are i32 and float values are f32 in default,) you can add `L` (or `l`) after integer values and `D` (or `d`) after float values:

```casc
c := 1L

d := 1.0D
```

> This section is about CASC 0.1 limitation

As previous mentioned, CASC does not have type declaration, so types are automatically detected by CASC compiler, so for most object cases, for instance, declares a variable with type `java::lang::Integer` would just create a variable with type `java::lang::Integer`, and for this moment, no casting is available, and since compiler would not perform auto casting even it's possible to cast, which means if a function accepts `Comparator<T>`, you are not able to pass `java::lang::Integer` in.

## Assignment

You can only assign objects to mutable variables or fields:

```casc
mut a := 1
a = 2 // OK
```

```casc
b := 1
b = 2 // Error
```

Interesting aspect, you can reassign a variable (except for field) with another type as long as it is mutable:

```casc
mut c := 1
c = java::lang::Integer(2) // Ok, now variable c's type is java::lang::Integer
```

