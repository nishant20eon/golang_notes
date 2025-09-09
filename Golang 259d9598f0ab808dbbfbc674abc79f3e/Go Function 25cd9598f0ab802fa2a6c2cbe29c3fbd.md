# Go Function

## ⚡ Go Functions – Quick Look

### 🔹 1️⃣ What is a Function?

- A **function** is a block of code that performs a task.
- Helps **reusability, modularity, and readability**.

---

### 🔹 2️⃣ Function Syntax

```go
func functionName(parameters) returnType {
    // code
    return value
}
```

- `parameters` → comma-separated `name type`
- `returnType` → optional (can return multiple values)

---

### 🔹 3️⃣ Types of Functions

### 1. **Simple Function (no parameters, no return)**

```go
func greet() {
    fmt.Println("Hello Go")
}

func main() {
    greet()
}
```

**Output:**

```
Hello Go
```

**When to use:** for simple tasks, like printing logs.

---

### 2. **Function with Parameters**

```go
func add(a int, b int) {
    fmt.Println("Sum:", a+b)
}

func main() {
    add(10, 20)
}
```

**Output:**

```
Sum: 30
```

**When to use:** when you need to pass input values.

---

### 3. **Function with Return Value**

```go
func multiply(a, b int) int {
    return a * b
}

func main() {
    result := multiply(5, 6)
    fmt.Println("Product:", result)
}
```

**bnOutput:**

```
Product: 30
```

**When to use:** when the function needs to **return a value**.

---

### 4. **Function with Multiple Return Values**

```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("cannot divide by zero")
    }
    return a / b, nil
}

func main() {
    result, err := divide(10, 2)
    if err != nil {
        fmt.Println("Error:", err)
    } else {
        fmt.Println("Result:", result)
    }
}
```

**Output:**

```
Result: 5
```

**When to use:** Go’s idiomatic **error handling**.

---

### 5. **Function with Named Return Values**

```go
func square(n int) (result int) {
    result = n * n
    return
}

func main() {
    fmt.Println("Square:", square(7))
}
```

**Output:**

```
Square: 49
```

**When to use:** when return value can be **named** for clarity.

---

### 6. **Anonymous Function**

```go
func() {
    fmt.Println("I am anonymous")
}()
```

**Output:**

```
I am anonymous
```

**When to use:** for quick inline code or **goroutine** execution.

---

### 7. **Function as Value / Higher-Order Function**

```go
func calc(a, b int, op func(int,int) int) int {
    return op(a, b)
}

func main() {
    sum := calc(5, 3, func(x, y int) int { return x + y })
    fmt.Println("Sum:", sum)
}
```

**Output:**

```
Sum: 8
```

**When to use:** functional programming, passing behavior as argument.

---

### 🔹 Key Points

- Go supports **multiple return values** → commonly used for `value + error`.
- Functions can be **first-class citizens** → assign to variables, pass as arguments.
- Use **anonymous functions** for short-lived logic or goroutines.
- Prefer **named return values** only for readability; don’t overuse.

---

This gives you a **complete function reference** in Go: simple, param, return, multiple return, named, anonymous, and higher-order.

---

**variadic functions** (`func sum(nums ...int)`):

- **What it is:** A function that can take **variable number of arguments**.
- **Syntax example:**

```go
func sum(nums ...int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}

func main() {
    fmt.Println(sum(1,2,3))    // 6
    fmt.Println(sum(5,10,15,20)) // 50
}

```

- **When to use:** When you don’t know how many inputs will come, e.g., logging, adding numbers, building strings.