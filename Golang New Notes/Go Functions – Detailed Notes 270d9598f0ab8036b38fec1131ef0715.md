# Go Functions – Detailed Notes

# ✅ **Go Functions – Detailed Notes**

Each section below follows this structure:

1. **Syntax** – how the function is declared
2. **Function Code Example** – with real use cases and, where applicable, using a separate struct
3. **How to Call the Function** – examples of invoking the function

---

## ✅ **1. Function without argument and without return type**

### ✅ Syntax

```go
func functionName() {
    // implementation
}

```

### ✅ Function Code Example

```go
package main

import "fmt"

// Function without argument and without return type
func greet() {
    fmt.Println("Hello, this is a simple greeting!")
}

func main() {
    greet()
}

```

### ✅ How to Call the Function

```go
greet()

```

- Call it directly without any parameters or return handling.
- It performs its action (printing the message) when called.

---

## ✅ **2. Function without argument and with single return type**

### ✅ Syntax

```go
func functionName() returnType {
    // implementation
    return value
}

```

### ✅ Function Code Example

```go
package main

import "fmt"

// Function without argument and with single return type
func getMagicNumber() int {
    return 7
}

func main() {
    result := getMagicNumber()
    fmt.Println("Magic Number is:", result)
}

```

### ✅ How to Call the Function

```go
result := getMagicNumber()
fmt.Println("Magic Number is:", result)

```

- The return value is stored in `result` and printed.

---

## ✅ **3. Function without argument and with two return types**

### ✅ Syntax

```go
func functionName() (type1, type2) {
    // implementation
    return value1, value2
}

```

### ✅ Function Code Example

```go
package main

import "fmt"

// Function without argument and with two return types
func getServerStatus() (int, string) {
    return 200, "OK"
}

func main() {
    code, message := getServerStatus()
    fmt.Println("Status Code:", code)
    fmt.Println("Message:", message)
}

```

### ✅ How to Call the Function

```go
code, message := getServerStatus()
fmt.Println("Status Code:", code, "Message:", message)

```

- Multiple return values are captured using two variables.

---

## ✅ **4. Function with argument and without return type**

### ✅ Syntax

```go
func functionName(paramName paramType) {
    // implementation
}

```

### ✅ Function Code Example

```go
package main

import "fmt"

// Function with argument and without return type
func printGreeting(name string) {
    fmt.Println("Hello,", name)
}

func main() {
    printGreeting("Alice")
}

```

### ✅ How to Call the Function

```go
printGreeting("Alice")

```

- Pass the argument directly, and the function uses it without returning anything.

---

## ✅ **5. Function with argument and with single return type**

### ✅ Syntax

```go
func functionName(paramName paramType) returnType {
    // implementation
    return value
}

```

### ✅ Function Code Example

```go
package main

import "fmt"

// Function with argument and with single return type
func square(number int) int {
    return number * number
}

func main() {
    result := square(5)
    fmt.Println("Square of 5 is:", result)
}

```

### ✅ How to Call the Function

```go
result := square(5)
fmt.Println("Square of 5 is:", result)

```

- Argument passed is processed and the result is stored and printed.

---

## ✅ **6. Function with argument and with two return types**

### ✅ Syntax

```go
func functionName(paramName paramType) (returnType1, returnType2) {
    // implementation
    return value1, value2
}

```

### ✅ Function Code Example

```go
package main

import "fmt"

// Function with argument and with two return types
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

### ✅ How to Call the Function

```go
result, err := divide(10, 2)
if err != nil {
    fmt.Println("Error:", err)
} else {
    fmt.Println("Result:", result)
}

```

- The result and error are handled separately after calling the function.

---

## ✅ **7. Function with struct reference, argument, and single return type**

### ✅ Syntax

```go
func (s *StructType) functionName(paramName paramType) returnType {
    // implementation
    return value
}

```

### ✅ Function Code Example

```go
package main

import "fmt"

// Struct definition
type Person struct {
    Name string
    Age  int
}

// Function with struct reference, argument, and single return type
func (p *Person) Greet(prefix string) string {
    return prefix + " " + p.Name
}

func main() {
    person := Person{Name: "Bob", Age: 25}
    message := person.Greet("Hello")
    fmt.Println(message)
}

```

### ✅ How to Call the Function

```go
person := Person{Name: "Bob", Age: 25}
message := person.Greet("Hello")
fmt.Println(message)

```

- Create a struct instance and call the method using the dot notation.

---

## ✅ **8. Function with struct reference, argument, and two return types**

### ✅ Syntax

```go
func (s *StructType) functionName(paramName paramType) (returnType1, returnType2) {
    // implementation
    return value1, value2
}

```

### ✅ Function Code Example

```go
package main

import "fmt"

// Struct definition
type Person struct {
    Name string
    Age  int
}

// Function with struct reference, argument, and two return types
func (p *Person) UpdateAge(newAge int) (bool, error) {
    if newAge < 0 {
        return false, fmt.Errorf("invalid age")
    }
    p.Age = newAge
    return true, nil
}

func main() {
    person := Person{Name: "Alice", Age: 30}
    success, err := person.UpdateAge(35)
    if err != nil {
        fmt.Println("Error:", err)
    } else {
        fmt.Println("Age updated:", success, "New Age:", person.Age)
    }
}

```

### ✅ How to Call the Function

```go
person := Person{Name: "Alice", Age: 30}
success, err := person.UpdateAge(35)
if err != nil {
    fmt.Println("Error:", err)
} else {
    fmt.Println("Age updated:", success, "New Age:", person.Age)
}

```

- Create a struct instance, call the method, and handle both return values.

---

## ✅ **9. Function with struct type argument (by value)**

### ✅ Syntax

```go
func functionName(s StructType) {
    // implementation
}

```

### ✅ Function Code Example

```go
package main

import "fmt"

// Struct definition
type Person struct {
    Name string
    Age  int
}

// Function with struct argument by value
func printPerson(p Person) {
    fmt.Println("Person Name:", p.Name)
    fmt.Println("Person Age:", p.Age)
}

func main() {
    person := Person{Name: "John", Age: 28}
    printPerson(person)
}

```

### ✅ How to Call the Function

```go
person := Person{Name: "John", Age: 28}
printPerson(person)

```

- The entire struct is passed by value to the function.
- The function accesses fields of the struct but cannot modify the original struct.

---

## ✅ **10. Function with struct type argument (by pointer)**

### ✅ Syntax

```go
func functionName(s *StructType) {
    // implementation
}

```

### ✅ Function Code Example

```go
package main

import "fmt"

// Struct definition
type Person struct {
    Name string
    Age  int
}

// Function with struct argument by pointer
func updateName(p *Person, newName string) {
    p.Name = newName
}

func main() {
    person := Person{Name: "John", Age: 28}
    fmt.Println("Before:", person.Name)
    updateName(&person, "Johnny")
    fmt.Println("After:", person.Name)
}

```

### ✅ How to Call the Function

```go
person := Person{Name: "John", Age: 28}
updateName(&person, "Johnny")

```

- A pointer to the struct is passed.
- The function modifies the original struct.

---

## ✅ **11. Struct method with struct reference and struct type argument**

### ✅ Syntax

```go
func (s *StructType) functionName(arg StructType) returnType {
    // implementation
}

```

### ✅ Function Code Example

```go
package main

import "fmt"

// Struct definitions
type Person struct {
    Name string
    Age  int
}

type Address struct {
    City    string
    ZipCode string
}

// Method with struct reference and struct type argument
func (p *Person) UpdateAddress(addr Address) string {
    return fmt.Sprintf("%s now lives in %s (%s)", p.Name, addr.City, addr.ZipCode)
}

func main() {
    person := Person{Name: "Alice", Age: 30}
    address := Address{City: "New York", ZipCode: "10001"}
    message := person.UpdateAddress(address)
    fmt.Println(message)
}

```

### ✅ How to Call the Function

```go
person := Person{Name: "Alice", Age: 30}
address := Address{City: "New York", ZipCode: "10001"}
message := person.UpdateAddress(address)
fmt.Println(message)

```

- The method receives a struct argument and returns a result.
- The method is called using the struct instance and dot notation.

---

## ✅ 12. Struct method with struct reference and pointer to another struct argument

### ✅ Syntax

```go
func (s *StructType) functionName(arg *StructType) returnType {
    // implementation
}

```

### ✅ Function Code Example

```go
package main

import "fmt"

// Struct definitions
type Person struct {
    Name string
    Age  int
}

type Address struct {
    City    string
    ZipCode string
}

// Method with struct reference and pointer to struct argument
func (p *Person) MoveTo(addr *Address) {
    fmt.Printf("%s is moving to %s (%s)\n", p.Name, addr.City, addr.ZipCode)
}

func main() {
    person := Person{Name: "Bob", Age: 40}
    address := Address{City: "Los Angeles", ZipCode: "90001"}
    person.MoveTo(&address)
}

```

### ✅ How to Call the Function

```go
person := Person{Name: "Bob", Age: 40}
address := Address{City: "Los Angeles", ZipCode: "90001"}
person.MoveTo(&address)

```

- The method receives a pointer to another struct.
- It can access or modify the pointed struct if needed.

## ✅ Summary

| Type | Syntax | Example Focus |
| --- | --- | --- |
| 1 | No argument, no return | Simple tasks like printing |
| 2 | No argument, single return | Static values or computations |
| 3 | No argument, two returns | Status reporting with code and message |
| 4 | Argument, no return | Using input without returning anything |
| 5 | Argument, single return | Simple computations like square or sum |
| 6 | Argument, two returns | Operations prone to failure like division |
| 7 | Struct method, argument, single return | Using struct data to compute something |
| 8 | Struct method, argument, two returns | Modifying struct data with validation |
|  |  |  |
|  |  |  |

| Struct argument (by value) | `func f(s StructType)` | Pass a struct and read fields |
| --- | --- | --- |
| Struct argument (by pointer) | `func f(s *StructType)` | Modify the original struct fields |
| Struct method with struct argument | `func (s *StructType) f(arg StructType)` | Use both struct data and another struct's data |
| Struct method with struct pointer argument | `func (s *StructType) f(arg *StructType)` | Access or modify another struct’s data |

### ✅ Why this matters

- Struct arguments allow passing complex data types into functions.
- Passing by pointer enables in-place modification and efficient memory usage.
- Methods on structs encapsulate behaviors and organize related functionality.
- Combining struct references and arguments creates powerful abstractions, useful in real-world applications like models, data handling, and service layers.

---

Now this set fully includes:

✔ Functions with struct arguments (both by value and by pointer)

✔ Struct methods that take other structs or pointers as arguments

✔ Complete examples, calling patterns, and explanations

Let me know if you want this extended into interface examples,