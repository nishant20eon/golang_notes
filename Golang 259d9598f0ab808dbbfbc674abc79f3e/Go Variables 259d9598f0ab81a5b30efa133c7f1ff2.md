# Go Variables

[Quick Look](Go%20Variables%20259d9598f0ab81a5b30efa133c7f1ff2/Quick%20Look%2025cd9598f0ab8092a7a3e23a02914f9d.md)

# 🐹 Go Variables Quick Reference

A handy guide for declaring and using variables in Go.

---

## 📌 Ways to Declare Variables

### 1. Explicit with type

```go
var age int = 25
```

### 2. Inferred type (var without type)

```go
var name = "Alice"  // compiler infers string
```

### 3. Short declaration (inside functions only)

```go
count := 10
```

### 4. Multiple variables

```go
var x, y, z int = 1, 2, 3a, b := "hello", "world"
```

### 5. Block declaration

```go
var (    id   = 101    name = "Alice"    pi   = 3.14)
```

### 6. Zero values (uninitialized)

```go
var total int      // 0var price float64  // 0.0var active bool    // falsevar msg string     // ""
```

---

## 📌 Variable Scope Levels

### 1. Package-level

- Declared **outside any function**
- Visible to all functions in the same package
- Exported if the name starts with **Capital Letter**

```go
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
```

### 2. Function-level (local)

- Declared **inside functions**

```go
func main() {    x := 10    fmt.Println(x)}
```

### 3. Block-level

- Inside `{ }` blocks (`if`, `for`, etc.)

```go
if true {    y := 20    fmt.Println(y)}// fmt.Println(y) // ❌ not visible here
```

---

## 📌 Key Rules

1. Use `:=` for local variables (idiomatic Go).
2. Use `var` at package-level or when explicit type is needed.
3. Every variable **must be used** — otherwise compiler error.
4. Zero values are auto-assigned when uninitialized.

---

✅ Keep this handy when coding in Go!