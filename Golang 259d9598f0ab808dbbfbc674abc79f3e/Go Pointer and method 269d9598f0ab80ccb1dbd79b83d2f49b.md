# Go Pointer and method

### ✅ **Go Pointers & Methods – Quick Reference Summary**

---

### 📌 **Pointer Basics:  and `&`**

1. **`Type` (e.g. `Customer`)**
    - Means “pointer to a `Customer`”.
    - Allows functions or methods to access or modify the original object’s data (not a copy).
    - Example:
        
        ```go
        func (c *Customer) AddBalance(amount float64) { c.Balance += amount }
        
        You can call it in two main ways:
        
        1️⃣ Using a value variable (Go takes the address automatically)
        customer := models.Customer{
            Id: 1, Name: "Alice", Balance: 1000.0, Age: 30,
        }
        
        customer.AddBalance(500) // Go interprets it as (&customer).AddBalance(500)
        
        fmt.Println(customer.Balance) // 1500
        
        Here customer is a value, but Go automatically converts it to a pointer to call the method.
        
        2️⃣ Using a pointer variable
        customer := &models.Customer{
            Id: 1, Name: "Alice", Balance: 1000.0, Age: 30,
        }
        
        customer.AddBalance(500) // Works directly since customer is already a pointer
        
        fmt.Println(customer.Balance) // 1500
        ```
        
2. **`&variable` (e.g. `&customer`)**
    - Gives the address of the variable.
    - Example:
        
        ```go
        cust := Customer{}
        custPtr := &cust
        ```
        
3. **`Type` (e.g. `Customer`)**
    - Means working on the actual object or a copy of it.
    - Any changes are local and do not affect the original object.
    - Example:
        
        ```go
        func UpdateBalanceCopy(c Customer, amount float64) { c.Balance += amount }
        ```
        

---

### 📌 **Pointer vs Value in Function Arguments**

| Type | Passing by value | Passing by pointer |
| --- | --- | --- |
| Struct (`Customer`) | Copies the data → changes don't affect original | Passes address → changes affect original |
| Interface (`context.Context`) | Already reference-like → passing by value is efficient | No need to pass as pointer |
| Large objects | Inefficient to copy | Efficient to reference and modify |

---

### 📌 **Methods vs Functions**

1. **Method receiver `(c *Customer)`**
    - Associates the method with a type.
    - Pointer receiver (`Customer`) allows modifying the original object.
    - Can be called using either value or pointer → Go converts automatically if needed.
2. **Method argument (e.g. `amount float64`)**
    - Regular parameter passed when calling the method.
3. **Return type**
    - If nothing specified → the method returns nothing (`void` equivalent).

---

### 📌 **Calling Methods**

✔ Works with both value and pointer:

```go
cust := Customer{Balance: 100}
cust.AddBalance(50) // Go converts to (&cust).AddBalance(50)

custPtr := &Customer{Balance: 100}
custPtr.AddBalance(50) // Works directly
```

---

### 📌 **Context (`context.Context`) in gRPC**

✔ Passed by value but safe because it's an interface → holds references internally.

✔ No need to use `*context.Context`.

✔ Efficient for sharing cancellation, deadlines, and request-scoped values.

---

### ✅ **Key Concepts to Remember**

✔ `*Type` → expects an address → can modify original data

✔ `&variable` → gives the address of a variable

✔ `Type` → works with a copy → changes don’t affect original

✔ Methods with pointer receivers can modify objects; methods with value receivers cannot

✔ Interfaces like `context.Context` are already reference-like → pass by value

✔ Go automatically converts values to pointers when calling pointer-receiver methods