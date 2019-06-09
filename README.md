### Table of contents

* [Introduction](#introduction)
* [Requirements](#requirements)
* [Platform support](#platform-support)
* [Installation](#installation)
* [Getting started](#getting-started)
* [Resources](#resources)
* [Core principles](#core-principles)
* [Examples](#examples)
* [Syntax](#syntax)
* [Naming convention](#naming-convention)
* [Technologies](#technologies)
* [Workflow](#workflow)
* [Frequently asked questions](#frequently-asked-questions)

### Introduction

The Ion language project is a programming language currently undergoing active, early development. It was created mainly for research purposes, however it is fair to say that it has grown beyond that.

Ion is an umbrella project that borrows functionality from its various respositories to accomplish a fully functional, compiled programming language.

### Requirements

Lucky for you, there are not many requirements!

> [Microsoft Visual C++ Redistributable 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145) (**Windows only**)

### Platform support

| Platform / Architecture     | x86 | x86_64 | ARM |
|-----------------------------|-----|--------|------
| Windows 10                  | ✓   | ✓      | x   |
| Linux                       | ✓   | ✓      | ✓   |
| OSX (10.12 Sierra or later) | ✓   | ✓      | -   |

### Installation

Head over to the releases page on the CLI utility's repository to get your installer.

> [View releases](https://github.com/IonLanguage/Ion.CLI/releases)

#### Linux

Get started quickly and conviniently with the following one-liner.

```shell
$ curl -sSL https://raw.githubusercontent.com/IonLanguage/Ion.CLI/master/install.sh | sudo sh
```

If you wish to view the source code of the installer script, simply navigate to its [source URL](https://raw.githubusercontent.com/IonLanguage/Ion.CLI/master/install.sh):



### Getting started

Make sure you have Ion installed on your machine at this point.

#### Windows

Use the following instructions to initialize and run a project in Windows.

```shell
cmd> mkdir myproject
cmd> cd myproject
cmd> ion init
cmd> mkdir src
cmd> notepad src/main.ion
```

Paste the following source code:

```c#
extern int puts(string message);

void main() {
    puts("Hello world!");
}
```

Now, save your file and close the Notepad editor.

You're now ready to run your program! Use the following command to compile & run your program:

```shell
cmd> ion run
```

#### Linux

Use the following instructions to initialize and run a project in Linux.

```shell
$ mkdir myproject
$ cd myproject
$ ion init
$ mkdir src
$ nano src/main.ion
```

Paste the following source code:

```c#
extern int puts(string message);

void main() {
    puts("Hello world!");
}
```

Now, save your file by pressing `CTRL + X`, then pressing `Y` and finally `ENTER`.

You're now ready to run your program! Use the following command to compile & run your program:

```shell
$ ion run
```

### Resources

* Syntax highlighting
    * [Notepad++](resources/npp-syntax.xml)
    * [Visual Studio Code](https://github.com/IonLanguage/Ion.VSCode) (undergoing early development)

### Core principles

1. **Simplicity**. The language should be simple, or as powerful as the programmer wishes. This means that some symbols and patterns are optional and infered by the compiler.

2. **Fully managed**. The programmer should not be required to spend significant time installing (or building) the language, or its dependencies, when they could be coding their projects instead.

3. **Flexible**. The language should contain tools and shortcuts to make the programming experience smooth, not rigid. This means that the programmer is not required (but can) to be explicit with their code.

### Examples

Hello world

```cs
extern int puts(string message);

void main() {
    puts("Hello world!");
}
```

Fibonacci sequence

```cs
int fib(number) {
    if (number <= 1) {
        return 1;
    }

    return fib(number - 1) + fib(number - 2);
}
```

Formatted output

```cs
extern int printf(string format, ...);

void main() {
    printf("%d + 2 = 3", 1);
}
```

Struct

```cs
extern int printf(string format, ...);

struct Person {
    string name;
    int age;
}

void main() {
    Person joe = new Person {
        name: "Joe",
        age: 25
    };

    printf("Joe's age in 5 years will be: %d.", joe.age + 5);
}
```

### Syntax

Globals

```cs
int @counter = 0;

void main() {
    counter++;
    // ...
}
```

Functions

```cs
void join(char delimiter, ...) {
    // ...
}

void main() {
    // ...
}
```

Structs

```cs
struct Person {
    string name;
    int age;
}

void main() {
    // Create the struct.
    Person joe = new Person {
        name: "Joe",
        age: 25
    };

    // Access its properties.
    joe.name;
    joe.age;
}
```

Lambdas

```cs
delegate void WorkCallback(int status);

void work(WorkCallback callback) {
    int status;
    // ...
    callback(status);
}

void main() {
    // Pass lambda as parameter.
    work((int status) => {
        // ...
    });
}
```

Importing modules

```cs
// Import a module.
import my.module;

// Import a system/built-in module.
import <os>;

// Import a module with an alias.
import my.other.module as other;

// ...
```

### Naming convention

#### Functions

All function names should be in camelCase.

```rust
void main() {
    //
}
```

#### Classes

Class names should be in PascalCase, and members in camelCase.

```c#
extern int printf(string message, ...);

class Example {
    public string name = "John Doe";

    void sayHello() {
        printf("Hello, " + this.name);
    }
}
```

#### Attributes

Attributes are considered proxy functions, thus they should be treated as functions and be named in camelCase.

```c
[Transform(0)]
int main() {
    // Return value will be transformed to '0'.
    return 1;
}
```

### Technologies

The entire project has been implemented around Microsoft's C# language, using .NET Core.

### Workflow

In the future, the Ion.IR repository will be the central point connecting all other repositories.

Below is a diagram of the future workflow implementation of the project.

<br />
<img src="assets/workflow.png" />

Below is a diagram of the different stages that the compilation process undergoes before reaching final executable form.

<br />
<img src="assets/stages.png" />

#### libllvm

libllvm is the C LLVM library used by LLVMSharp.

#### LLVMSharp

LLVMSharp is an external NuGet package that provides the C# bindings of the C LLVM library to all projects that require it.

#### IonIR

The Ion.IR repository allows creation, manipulation and generation of Ion's intermediate representation language named `IonIR`.

The Ion.IR repository plays a vital role in the project. Ion.IR's simple API implementation allows for efficient and easy manipulation of IonIR code. Compared to the Ion language, IonIR code is much simpler, concise and condensed. This is key because it strips the need of handling Ion's complex, abstracted syntax.

The Ion.IR repository is the last step of program compilation. Code generation is handled here through the C# LLVM library bindings.

#### IonCLI

The Ion.CLI project serves multiple essential purposes:

1. LLVM toolchain wrapper
2. Ion code compilation CLI utility
3. Package management

Since LLVM's functionality of LLVM IR to Bitcode compilation is only available through its executable tools, releases are forced to package the compiled LLVM tools.

The Ion.CLI repository manages and oversees the execution of these tools.

### Frequently asked questions

**What is Ion?** Ion is a high-level (and low-level), strongly-typed language implemented using the LLVM project, currently undergoing early development.

**What is the purpose of this project?** A research project. The purpose of this language is to serve as the basis for learning, and experimenting with compiler design. However, the language is to be fully implemented and functional.

**Why should I use Ion?** If you're a fan of fast development, and simply want your development workflow to be smooth, yet rigid when necessary, Ion is a language for you.

**How can I learn compiler design?** We strongly recommend you start your journey into the mysterious and complex world of compiler design through [this book](http://www.informatik.uni-bremen.de/agbkb/lehre/ccfl/Material/ALSUdragonbook.pdf). Wikipedia is also your best friend.

**What is the technology stack used for this project?** The C# language is the main language used to implement both the core compiler library (this repository) and the CLI utility. LLVM (using C# bindings) is used for code generation. NUnit is used for both compiler and CLI tests, although a partial custom testing framework (based upon NUnit) is currently being implemented in order to satisfy the specific unit testing needs of the projects.
