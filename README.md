# CASC Lang Documention  
## Introduction

CASC is a programming language focus on concise syntaxes and enchancing language features from Java. CASC features:
- Fully interoperability to Java
- Shorthand syntaxes
- Rust / Go Lang / V Lang inspired syntaxes
- Better class form

CASC uses .casc and .cas as source file extension name.

## Installation

For 0.1, please heads to CASC github repostory's release for more infomation.

## Compiler Usage

```cmd
java -jar CASC-0.1.jar (-cp "classpath1;" "classpath2") (-o "C:/outputDirectory/") <File / Folder to be compiled>
```

If the last argument is a CASC source file, then it'll only queue the only file and will not try to compile other related CASC source files, but if it is a folder, then compiler will queue all the files in subdirectories into compilation.

## Table Of Contents

<table>
    <tr><td width=33% valign=top>

* [Hello World](#hello-world)
    
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

Let's take a look at the code above: First, `class` keyword declares a public class in default, it's necessary for CASC 0.1 to compile into class file. Second, similar to Java's main function, you'll need to declare main function, to declare it a static function, put `comp` keyword in front of the `fn` keyword to do it. Since we still haven't implemet a easier way to decalre argument-less main function, you'll need to add `args: str[]` into arguments in order to let JRE identify it as the entry function.

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