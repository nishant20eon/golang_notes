# Var

```markdown
**  ??  **
var age int = 25
**  ??  **
var name = "Alice"  // compiler infers string
**  ??  **
count := 10
**  ??  **
var x, y, z int = 1, 2, 3a, b := "hello", "world"
**  ??  **
var 
(    
id   = 101    
name = "Alice"    
pi   = 3.14
a int = 10
)
**  ??  **
var n int       // 0
var s string    // ""
var f float64   // 0
var flag bool   // false

// 6. Global variables (outside function)
var Version = "1.0.0"
**  ??  **
package mypkg

var GlobalVar = 42   // Exported
var localVar = 99    // Not exported

package main

import (
    "fmt"
    "mypkg"
)

func main() {
    fmt.Println(mypkg.GlobalVar) // ✅ accessible
    // fmt.Println(mypkg.localVar) // ❌ compile error (unexported)
}

GlobalVar (capital G) → exported (public), can be accessed from other packages.

localVar (lowercase l) → unexported (private), visible only inside the same package.
**  Function  **
func main() {    
x := 10    
fmt.Println(x)
}
**  Block Level **
if true {    
y := 20    
fmt.Println(y)}// fmt.Println(y) // ❌ not visible here
}

```