---
# Basic Note Frontmatter
title: Get-Set-GoLang
description: "Go or GoLang is a statically typed, Compiled, High-level Programming Language designed by Google, which finds immense use cases in the Cloud, DevOps and Server-Side spaces"
# Site Configurations
share: true
# Dataview Fetch Fields
content_type: MOC
content_tags:
 - Open-Source
 - Programming-Language
 - Cloud-and-DevOps
 - Compiled
 - Statically-Typed
 - Cross-Platform
---
## Introduction
**Go**, also referred to as **Golang** is a programming language developed by Google in 2007. It's designed for simplicity, performance, and concurrency. Go has a built-in concurrency model with goroutines and channels. It includes a garbage collector for automatic memory management. Go is popular for building high-performance systems and network applications. Go modules is the built-in package manager for dependency management. Go is a great option for developers who want to build fast, efficient, and scalable systems.

### GoLang History and Development
The development of Golang started in 2007. The language was first announced publicly in November 2009 and was officially released in March 2012.

Go was created by a team of three developers at Google, *Robert Griesemer*, *Rob Pike*, and *Ken Thompson*, with the goal of creating a new programming language that was more productive, efficient, and scalable than existing languages.

Go was designed to address some of the limitations of other programming languages, such as slow compilation times, verbose syntax, and lack of support for concurrency. The language was also designed to be *easy to learn and use*, with a *minimalist syntax* and a *focus on simplicity*.

Go's development was heavily influenced by other programming languages, particularly C, Pascal, and the programming language Alef, which was also developed by Rob Pike and others at Bell Labs in the 1980s.

Go's development has been *guided by an open source community*, with contributions from many developers around the world. The language has seen rapid adoption, particularly in the field of *cloud computing*,*distributed systems* and *DevOps*. Today, Go is used by many major companies, including Google, Uber, Dropbox, and Netflix, among others.

Go continues to evolve, with new releases and updates bringing new features and improvements to the language. The language has gained a reputation for being fast, efficient, and easy to use, and is increasingly seen as a viable alternative to other popular programming languages such as Python, Ruby, and Java.

### Language Syntax
The syntax of Golang is influenced by C, but it also includes features from other programming languages, such as Pascal and Modula-2. Here are some key features of the Golang syntax:

1.  **Simple and readable** - The syntax of Golang is designed to be simple and readable, with a minimal amount of keywords and syntax. This makes it easy for developers to understand and maintain code.
2.  **Statically typed** - Golang is a statically typed language, which means that variable types are declared at compile time. This helps to catch errors early in the development process.
3.  **Package-based** - Golang organizes code into packages, which are collections of functions, types, and variables. Packages can be imported and used by other packages, making it easy to reuse code and manage dependencies.
4.  **Pointers** - Golang includes pointers, which are variables that hold the memory address of another variable. Pointers are used to create more efficient code and to allow for direct manipulation of memory.
5.  **Concurrency support** - Golang includes built-in support for concurrency through goroutines and channels. Goroutines are lightweight threads that allow for parallel execution of code, while channels provide a safe and efficient way to share data between concurrent processes.
6.  **Garbage collection** - Golang includes a garbage collector that automatically manages memory, freeing developers from having to worry about manual memory management.
7.  **Error handling** - Golang includes a built-in error handling system, which makes it easy to handle and propagate errors throughout a program.

Overall, the syntax of Golang is designed to be simple, readable, and efficient, with a focus on concurrency and error handling. These features make Golang a great choice for building high-performance systems and network applications

Go is designed to be simple to understand, code and learn. With this in mind, a typical go development environment consists of a set of tools that make development in go a pleasurable experience.

### Basic Concepts
Before starting anything, the first thing to learn in any programming language  is to understand the help system. To access the built-in help system within Golang, the following command can be used.

=== "Input"

	```go
		go help
	```

=== "Output"

	```txt
		Go is a tool for managing Go source code.

	Usage:

        go <command> [arguments]

	The commands are:

        bug         start a bug report
        build       compile packages and dependencies
        clean       remove object files and cached files
        doc         show documentation for package or symbol
        env         print Go environment information
        fix         update packages to use new APIs
        fmt         gofmt (reformat) package sources
        generate    generate Go files by processing source
        get         add dependencies to current module and install them
        install     compile and install packages and dependencies
        list        list packages or modules
        mod         module maintenance
        work        workspace maintenance
        run         compile and run Go program
        test        test packages
        tool        run specified go tool
        version     print Go version
        vet         report likely mistakes in packages

	Use "go help <command>" for more information about a command.

	Additional help topics:

        buildconstraint build constraints
        buildmode       build modes
        c               calling between Go and C
        cache           build and test caching
        environment     environment variables
        filetype        file types
        go.mod          the go.mod file
        gopath          GOPATH environment variable
        gopath-get      legacy GOPATH go get
        goproxy         module proxy protocol
        importpath      import path syntax
        modules         modules, module versions, and more
        module-get      module-aware go get
        module-auth     module authentication using go.sum
        packages        package lists and patterns
        private         configuration for downloading non-public code
        testflag        testing flags
        testfunc        testing functions
        vcs             controlling version control with GOVCS

	Use "go help <topic>" for more information about that topic.
	```

Most Golang projects start with a `go mod init` command being run at the terminal. It goes something like this

```bash title="Initializing a Go Project"
# General Syntax
go mod init <some-name/repo-name>

# Example 1: Initialize a Hello World project
go mod init hello-world

# Example 2: Initialize a Standalone Go Project
go mod init github.com/GoPal/Hello-World
```

=== "Input"

	```go
		go mod init hello-world
	```

=== "Output"

	```txt
		go: creating new go.mod: module hello-world
	```

The result of this operation is the creation of a `go.mod` file in the directory where the command is run.

**Why `go mod init`?**

`go.mod` is a file that is used to track dependencies for the current go project. the following are some of the reasons why initializing a go module is considered best practice.

1. **Dependency Management** - `go mod init` is essential for managing dependencies in a Golang project. The `go.mod` file created by `go mod init` command tracks the versions of dependencies used in the project and ensures that they are compatible with each other.
2. **Versioning** - `go.mod` file assists in versioning of the code and its dependencies. This assists in easy sharing and reuse of the same code across teams and projects.
3. **Portability** - `go.mod` file is a self-contained used, meaning that it can be moved to different environments and build systems.
4. **Compatibility** - `go mod init` is designed to be backwards-compatible, meaning that it can be run on older Golang projects and it would still work without any issues.

By convention, the first thing to do in a programming language is try and print `Hello World!`. That is exactly the goal of this section. In Golang, the code is organized into packages. Every functionality of the language has to be imported to be used. Consider the following example.

```go title="Go - Hello World"
package main

import "fmt"

func main() {

    fmt.Println("Hello World!")
    
}
```

**What is `package main` ?**

Here, the `package main` is a declaration usually found in files named `main.go`. These are declarations that the go compiler uses to compile the files to an executable. It tells the compiler that this is intended to be an executable program and not a package that is intended to be imported into other programs.

`package main` must be the first line of the code. and the file must contain a `main` function. this function is the entry point for code execution, where the code/program logic begins evaluation.

Summarizing, the `package main` is a declaration on `main.go` files indicates the go compiler to build an executable program out of it and that the program contains a main function to act as the entry point for code execution.

**What are we importing?**

In Golang, the import statement is used to include code from other packages in the current program. When a package is imported, all the functions, types and variables exposed by the package will be available to use by the importing program.

To import a package, use the following syntax

```go title="Importing Go packages"
# Importing a single package
import "fmt"

# Importing multiple packages
import {
	"fmt"
	"math"
	"bufio"
}
```

As a best practice, do not import packages that are not used by the program. Import what is really needed to run the program.

## Data Types and Variables

### Data Types
In GoLang, there are several built-in data types that assist in creating variables and assigning values to them, so that they can be manipulated in programs. The following is the list of the data types available in Golang.

#### Boolean
- The `bool` data type represents a true or false value.

#### Numeric
-   *`int` and `uint`* - Signed and unsigned integers of various sizes (int8, int16, int32, int64, uint8, uint16, uint32, uint64).
-   *`float32` and `float64`* - Floating-point numbers of varying precisions.
-   *`complex64` and `complex128`* - Complex numbers with floating-point real and imaginary parts.

The following table captures the permitted values along with the memory allocation for each of the numeric data types

| Numeric Type | Allocated Size | Permitted Values                                      |
| ------------ | -------------- | ----------------------------------------------------- |
| uint8        | 8-bits         | 0 - 255                                               |
| uint16       | 16-bits        | 0 - 65535                                             |
| uint32       | 32-bits        | 0 - 4294967295                                        |
| uint64       | 64-bits        | 0 - 18446744073709551615                              |
| int8         | 8-bits         | -128 - 127                                            |
| int16        | 16-bits        | -32768 - 32767                                        |
| int32        | 32-bits        | -2147483648 - 2147483647                              |
| int64        | 64-bits        | -9223372036854775808 to 9223372036854775807           |
| float32      | 32-bits        | IEEE-754 floating point numbers                       |
| float64      | 64-bits        | IEEE-754 floating point numbers                       |
| complex64    | 64-bits        | Complex numbers with float32 real and imaginary parts |
| complex128   | 128-bits       | Complex numbers with float32 real and imaginary parts |

#### String
- The `string` data type represents a sequence of Unicode characters.
- Strings are immutable, meaning once they are created, they cannot be changed.
- A string's length is the number of bytes it occupies. It could be 0 for an empty string, but never negative. The length can be computed with the built-in function, `len`.
- Individual elements of the string can be accessed using the index of the character such as `string[0]` to `string[len(string) - 1]`. 
- However, it is not allowed to take the address of an individual elements of a string.

#### Array
- An `array` is a fixed-size collection of elements of the same type.

#### Slice
- A `slice` is a dynamic array that can grow or shrink as needed.

#### Map
- A `map` is a collection of key-value pairs.

#### Struct
- A `struct` is a composite data type that allows developers to group related values of different data ty

#### Interface
- An `interface` is a type that defines a set of methods that a type must implement to be considered an instance of that interface.

#### Pointer
- A `pointer` is a variable that holds the memory address of another variable.

In addition to these built-in data types, Golang also supports user-defined types, which can be created using the `type` keyword. Overall, the data types available in Golang provide developers with a flexible and powerful set of tools for creating and manipulating data in their programs.

### Variables
Now that the data types are known, the next logical step is to learn how to use them. There are several ways to declare a variable in Golang, the methods are stated below.

**Method 1: Using the `var` keyword and explicit data type declaration**

```go
// Variable declaration with explicit data type
var username string = "John Doe"
```

Here, a variable username was created with string data type, and declared a value of "John Doe"

*Possible errors:* If the explicit data type does not match the value being supplied to the variable during initialization, the go compiler throws an error. The following code throws an error.

```go
// Variable declaration with wrong explicit data type
var username int = "John Doe"
```

**Method 2: Using the var keyword and implicit data type declaration**

```go
// Variable declaration with implicit data type
var user_id = "EM001"
```

This follows a syntax to create a variable and initialize a value to it, but here the go compiler implicitly selects the data type for the value supplied.

**Method 3: Using the short variable declaration (Walrus Operator)**

```go
// Variable declaration with short variable declaration
age := 45
```

`:=` is the *short variable declaration operator*, commonly referred to as the *walrus operator* (due to appearance resembling a walrus with tusks)  popularized by Russ Cox, one of the original developers of the language. The following are some key points to remember when it comes to the walrus operator.
- It can be used only for variable declaration. To assign values to existing variables, the `=` operator is used. 
- The scope of a variable declared with the walrus operator is within the function, and not the package level. To declare variables at the package level, the `=` operator must be used.

*Possible Errors:* For short form variable declarations, the var keyword must not be used. If used, the go compiler throws an error.

```go
// Illegal short form variable declaration
var age:= 45
```

*Note:* In Golang, it is not possible to initialize a variable with a value before declaration. Thus a declaration needs to be made either explicitly or implicitly. The 3 methods stated above all declare the variable and supply an initialization value, and are thus considered valid syntax. However, assigning a value to a variable, without declaration will make the go compiler throw an error as in go, the `=` operator is considered as an assignment operator. The following code produces an error.

```go
// Variable value assignment without initialization
age = 45
```

**Method 4: Declaring multiple variables at once**

```go
// Multiple variables - same data types (explicit declaration)
var email, domain string = "john@doecompany.com", "JDC-AP"

// Multiple variables - same type (:= operator)
age, height := 25, 180
```

*Possible Errors:* When declaring multiple variables, all the variables must be of the same data type. Declaring multiple variables with multiple data types will lead to a `type mismatch` error.

```go
// Illegal multiple variable declaration
var name, height = "John Doe", 180

// Illegal multiple variable declaration
var name, email string, age int = "John Doe", "john.doe@example.com", 25
```

**Method 5: Declaring variables without initial value**

In Golang, when a variable is just declared and not assigned a value, it takes a default value. The following code showcases the defaults.

```go
var i int
var f float32
var b bool
var s string
var ptr *int
var m map[string]int
var sl []int
var ch chan int

fmt.Println(i)    // Output: 0
fmt.Println(f)    // Output: 0
fmt.Println(b)    // Output: false
fmt.Println(s)    // Output: ""
fmt.Println(ptr)  // Output: nil
fmt.Println(m)    // Output: map[]
fmt.Println(sl)   // Output: []
fmt.Println(ch)   // Output: nil
```

**Method 6: Declaration of constants**

```go
const pi float64 3.14159
```

*Possible Errors:* Once a constant variable is declared, its value cannot be changed during the program's execution.

### Operators
Operators in any programming language facilitate arithmetic, logical and string operations on variables.

Following are the different types of operators in Golang and a short description of what they do.

#### Arithmetic operators
These operators are used to perform basic arithmetic operations such as addition, subtraction, multiplication, division, and modulus.

The following are arithmetic operators in Go

```txt
+    addition
-    subtraction
*    multiplication
/    division
%    modulus
```

#### Comparison Operators
These operators are used to compare two values and return a Boolean value indicating the result of the comparison.

The following are comparison operators in Go

```txt
==    equal to
!=    not equal to
<     less than
<=    less than or equal to
>     greater than
>=    greater than or equal to
```

#### Bitwise Operators
These operators are used to perform bitwise operations on integer values.

The following are bitwise operators in Go

```txt
&    bitwise AND
|    bitwise OR
^    bitwise XOR
<<   left shift
>>   right shift
&^   bitwise AND NOT
```


#### Assignment Operators
These operators are used to assign values to variables.

The following are assignment operators in Go

```txt
=     simple assignment
+=    addition assignment
-=    subtraction assignment
*=    multiplication assignment
/=    division assignment
%=    modulus assignment
<<=   left shift assignment
>>=   right shift assignment
&=    bitwise AND assignment
|=    bitwise OR assignment
^=    bitwise XOR assignment
&^=   bitwise AND NOT assignment
```

#### Other Miscellaneous Operators
A few other operators exist in Go, to facilitate specific operations. 

```txt
&    address of operator
*    pointer dereference operator
<-   channel send/receive operator
:=   walrus operator (short variable declaration)*
```

*\*Note:* The walrus operator is not a separate type of operator, but just a shorthand representation to declare and initialize a variable.

### Type Conversions
Type conversions are actions done on values housed inside variables to convert them from one data type to another. Go supports the following types of type conversions.

#### Implicit Type Conversions
Implicit conversions are type conversions that are carried out automatically by  Golang for data types that are compatible with one another.

One example of this conversion is conversion of values from integer to float when assigning to variables of float data type (as given below)

```go
var num int = 10
var fnum float64 = num // Implicit conversion (int - float64)
fmt.Println(fnum) // Output: 10.0
```

#### Explicit Type Conversions
Explicit conversions are made when go is specifically instructed to convert a value from one data type to another, by means of a type casting expression.

In Golang explicit type conversions can be done in the following ways

##### Using Type Casting
This type of operation is applicable only to easily interchangeable data types such as conversions within the numerical data types. The following code snippet showcases an implementation of such a type casting in Golang.

```go
var fnum float64 = 10.5
var inum int = int(fnum) // Type casting (float64 - int)
fmt.Println(inum) // Output: 10
```

##### Using Conversion Functions
Golang provides specific conversions functions that can be used to convert data from one data type to another. 

The [`strconv`](https://pkg.go.dev/strconv) package is one of the most commonly used type conversions packages used in Golang. 

```go
var str string = "10"
var num int = strconv.Atoi(str) // String - int
fmt.Println(num) // Output: 10
```

Some other functions in the `strconv` go package are as follows.
1.  `Atoi()` - converts a string to an integer (`int`)
2.  `ParseInt()` - converts a string to an integer (`int64`)
3.  `ParseUint()` - converts a string to an unsigned integer (`uint64`)
4.  `ParseFloat()` - converts a string to a floating-point number (`float64`)
5.  `FormatInt()` - converts an integer (`int64`) to a string
6.  `FormatFloat()` - converts a floating-point number (`float64`) to a string

Golang, out of the box provides conversion function for commonly used data formats such as binary, json, xml, csv, color codes and so much more. 



