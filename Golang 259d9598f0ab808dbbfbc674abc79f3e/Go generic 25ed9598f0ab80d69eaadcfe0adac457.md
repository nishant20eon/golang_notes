# Go generic

# 🟢 Go Generics (introduced in Go 1.18+)

Generics = write functions or types with **type parameters**.

Similar to Java `<T>`.

### Generic Function

```go
package main

import "fmt"

// generic function
func PrintSlice[T any](s []T) {
    for _, v := range s {
        fmt.Println(v)
    }
}

func main() {
    PrintSlice([]int{1, 2, 3})
    PrintSlice([]string{"a", "b", "c"})
}

```

✅ `[T any]` → T is a type parameter, can be any type.

✅ `any` = alias for `interface{}`.

---

### Generic Constraint

Restrict type parameter with an interface.

```go
type Number interface {
    int | float64
}

func Sum[T Number](a, b T) T {
    return a + b
}

func main() {
    fmt.Println(Sum(3, 4))       // int
    fmt.Println(Sum(2.5, 3.1))   // float64
}

```

✅ Only `int` and `float64` allowed.

✅ Similar to Java bounded generics (`<T extends Number>`).

---

# 🎯 Quick Summary

- **Generics** → introduced in Go 1.18.
    - `[T any]` for generic type.
    - Use **constraints** to limit types (`int | float64`).
    - Works for functions, structs, and methods.

👉 Example: **Reusable function to find Max**

```go
package main

import "fmt"

// Define constraint
type Number interface {
    int | float64
}

// Generic function to get max
func Max[T Number](a, b T) T {
    if a > b {
        return a
    }
    return b
}

func main() {
    fmt.Println("Max int:", Max(10, 20))       // works with int
    fmt.Println("Max float:", Max(4.5, 2.3))  // works with float
}

```

✅ Same function works for **int** and **float64**.

✅ No need to write duplicate code.

---

## 🟢 Combining Both (Enums + Generics Example)

👉 Example: **Process Payments with different transaction types**

```go
package main

import "fmt"

// Enum for transaction type
type TxType int

const (
    Credit TxType = iota
    Debit
)

// Generic function for transaction logging
func LogTransaction[T int | float64](amount T, tx TxType) {
    if tx == Credit {
        fmt.Println("Credited:", amount)
    } else {
        fmt.Println("Debited:", amount)
    }
}

func main() {
    LogTransaction(100, Credit)   // int
    LogTransaction(250.75, Debit) // float
}

```

✅ Handles **enum values** (Credit/Debit).

✅ Supports both **int and float amounts** via generics.

✅ Looks close to real fintech/banking code.

---

⚡ **Final Recap**

- **Enums** (via `iota`) → represent states like `Pending, Delivered, Cancelled`.
- **Generics** → write one reusable function for multiple types (`int`, `float64`, etc).
- ✅ Together → clean, type-safe, and reusable code.

## 🟢 Example: Generic `Box[T]` Struct

👉 Use case: Storing **different typed values** (like a wrapper around DB values, API responses, etc.)

```go
package main

import "fmt"

// Generic struct Box that can hold any type T
type Box[T any] struct {
    value T
}

// Method to get the value
func (b Box[T]) Get() T {
    return b.value
}

// Method to set/update the value
func (b *Box[T]) Set(v T) {
    b.value = v
}

func main() {
    // Box of int
    intBox := Box[int]{value: 100}
    fmt.Println("Int Box:", intBox.Get()) // 100
    intBox.Set(200)
    fmt.Println("Updated Int Box:", intBox.Get()) // 200

    // Box of string
    strBox := Box[string]{value: "Hello"}
    fmt.Println("String Box:", strBox.Get()) // Hello
    strBox.Set("World")
    fmt.Println("Updated String Box:", strBox.Get()) // World
}

```

✅ Works like Java’s `Box<T>` or `Optional<T>`.

✅ Same struct can store **int, string, float, custom struct, etc.**

✅ No need to write separate `IntBox`, `StringBox`, etc.

---

## 🟢 Example: Generic `Response[T]` (Closer to Real API)

👉 Use case: Wrapping API responses in fintech apps.

```go
package main

import "fmt"

// Generic API Response wrapper
type Response[T any] struct {
    Status  string
    Data    T
    Message string
}

func main() {
    // API returns user details
    userResp := Response[map[string]string]{
        Status:  "success",
        Data:    map[string]string{"id": "101", "name": "Nishant"},
        Message: "User fetched successfully",
    }

    fmt.Println("User API Response:", userResp)

    // API returns account balance
    balanceResp := Response[float64]{
        Status:  "success",
        Data:    1520.75,
        Message: "Balance fetched successfully",
    }

    fmt.Println("Balance API Response:", balanceResp)
}

```

✅ Very common in fintech/backend → APIs always return some **generic wrapper**.

✅ Here `Response[T]` works with both `map` (user data) and `float64` (account balance).

---

⚡ **Final Recap:**

- `Box[T]` → a **container** struct for any type (like Java `Box<T>`).
- `Response[T]` → real-world **API response wrapper** that works for any data type.