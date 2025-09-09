# More Details

## 📌 Go Constants (`const`)

### 🔹 What is a Constant?

- A **constant** is a value that **cannot be changed** once defined.
- Declared using the `const` keyword.
- Can be of **numeric, string, or boolean** type.
- Constants are evaluated **at compile time**.
- **No `:=`** (only `=`)

---

### 🔹 Syntax

```go
const identifier type = value
```

Type is optional (Go can infer it).

### ✅ Examples

```go
const PI = 3.14        // untyped
const Msg string = "Hi" // typed
const A, B = 10, 20

// with expression
const Result = 2 * 5
```

### 🔄 iota (auto-increment)

### 🔹 Constant Groups & `iota`

- `iota` is a **counter** that auto-increments within a `const` block.
- Useful for enums.

```go
const (
  Sun = iota // 0
  Mon        // 1
  Tue        // 2
)
```

👉 Use for **enums, flags, constants**.

### 🔹 Key Points

- Constants **cannot be declared using `:=`**.
- Only **compile-time calculable values** are allowed.
- Arrays/slices/maps **cannot be constants**.