# Object Creation in Go

### ✅ **Object Creation in Java**

- `Customer c = new Customer();` → creates an object and returns a reference.
- `c.addBalance(500);` → call method on object.
- Inline version: `new Customer().addBalance(500);` → creates and calls immediately.

In Go:

```go
customer := models.Customer{
    Id: 1, Name: "Alice", Balance: 1000.0, Age: 30,
}

```

- This **is not a constructor** in the Java sense because Go doesn’t have constructors built into structs.
- What you are doing is called a **struct literal**. It creates a new `Customer` struct and **initializes its fields** at the same time.
- It’s very similar to using a constructor in Java that takes parameters to set fields.

✅ Example comparison with Java:

```java
// Java constructor
Customer customer = new Customer(1, "Alice", 1000.0, 30);

```

```go
// Go struct literal
customer := Customer{Id: 1, Name: "Alice", Balance: 1000.0, Age: 30}

```

---

### Optional “constructor” style in Go

Go allows you to write a factory function, which is closer to a Java constructor:

```go
func NewCustomer(id int, name string, balance float64, age int) *Customer {
    return &Customer{Id: id, Name: name, Balance: balance, Age: age}
}

// Usage
customer := NewCustomer(1, "Alice", 1000.0, 30)

```

- This can include extra logic if needed, unlike a plain struct literal.
- Returns a pointer, which is often convenient for methods with pointer receivers.

---

So in short:

- `models.Customer{...}` = **struct literal**, shorthand for initializing fields.
- You **don’t need setters/getters** unless you want extra logic.

---

### ✅ **Object Creation in Go**

### 1. **Struct Literal (most common)**

```go
customer := models.Customer{
    Id: 1, Name: "Alice", Balance: 1000.0, Age: 30,
}
customer.AddBalance(500)

or

&models.Customer{ 
    Id: 1, Name: "Alice", Balance: 1000.0, Age: 30,
}.AddBalance(500)

```

- No `new` keyword needed.
- Creates and initializes in one step.
- Works with both pointer and value methods (Go handles it).

### 2. **Using `new()`**

```go
customer := new(models.Customer)
customer.Id = 1
customer.Balance = 1000.0
customer.AddBalance(500)

```

- Allocates memory and returns `Customer`.
- Fields are zero-initialized.

### 3. **Inline method call**

```go
new(models.Customer).AddBalance(500)

```

or

```go
(&models.Customer{Id: 1, Balance: 1000.0}).AddBalance(500)

```

- Creates and calls in one expression.

---

### ✅ **Pointer vs Value**

| Expression | Meaning | Use case |
| --- | --- | --- |
| `*Customer` | Pointer → refers to actual object in memory | Use to modify original object |
| `&customer` | Get address of object | Pass to function/method expecting pointer |
| `Customer` | Value copy of object | Used when no mutation needed |
| `new(Customer)` | Allocates memory and returns pointer | Equivalent to Java’s `new Customer()` |