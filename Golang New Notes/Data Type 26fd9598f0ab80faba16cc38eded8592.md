# Data Type

```markdown
** Basic Data Type **
bool          // true, false
string        // text
int, int8, int16, int32, int64   // signed integers
uint, uint8, uint16, uint32, uint64, uintptr // unsigned
byte          // alias for uint8
rune          // alias for int32 (represents Unicode char)
float32, float64
complex64, complex128

Example
var flag bool = true
var name string = "GoLang"

var a int = 42
var b uint = 100
var c byte = 'A'       // ASCII 65
var d rune = '❤'       // Unicode char

var pi float64 = 3.1415
var z complex64 = 2 + 3i

Derived Types
Array   // fixed-size collection
Slice   // dynamic array
Map     // key-value pairs
Struct  // custom data type
Pointer // memory address
Function// function type
Interface // abstract behavior
Channel // for concurrency

Code Example how to use

package main

import "fmt"

func main() {
    // Boolean
    var flag bool = true
    fmt.Println("bool:", flag)

    // String
    var name string = "GoLang"
    fmt.Println("string:", name)

    // Integer
    var age int = 25
    fmt.Println("int:", age)

    // Unsigned Integer
    var u uint = 100
    fmt.Println("uint:", u)

    // Byte (alias for uint8)
    var b byte = 'A'
    fmt.Println("byte (char):", b, "=>", string(b))

    // Rune (alias for int32, Unicode char)
    var r rune = '❤'
    fmt.Println("rune (char):", r, "=>", string(r))

    // Float
    var pi float64 = 3.1415
    fmt.Println("float64:", pi)

    // Complex
    var z complex64 = 2 + 3i
    fmt.Println("complex64:", z)
}

Output:
bool: true
string: GoLang
int: 25
uint: 100
byte (char): 65 => A
rune (char): 10084 => ❤
float64: 3.1415
complex64: (2+3i)
```