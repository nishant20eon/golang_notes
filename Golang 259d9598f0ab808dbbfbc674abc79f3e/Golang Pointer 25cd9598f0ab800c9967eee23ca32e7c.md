# Golang Pointer

[M-1](Golang%20Pointer%2025cd9598f0ab800c9967eee23ca32e7c/M-1%2025cd9598f0ab809c9ab0c91571d812d7.md)

[M-2](Golang%20Pointer%2025cd9598f0ab800c9967eee23ca32e7c/M-2%2025cd9598f0ab804e891bc28ecca3bafc.md)

### 🔹 What is a Pointer?

- A pointer is a variable that **stores the memory address** of another variable.
- Declared with `Type` (e.g., `int`, `string`, `User`).
- Use `&` to get the **address of a variable**, and  to **dereference (access/change the value)**.

---

### 🔹 Syntax

```go
package main

import "fmt"

func main() {
    x := 10           // x is a variable with value 10, stored at some address, say 0x004dce
    var ptr *int      // ptr is a pointer that can hold the address of an int

    ptr = &x          // assign the address of x to ptr, so ptr now points to 0x004dce

    fmt.Println("Address of x:", &x) // print where x is stored
    fmt.Println("Address stored in ptr:", ptr) // print what address ptr holds
    fmt.Println("Value at address ptr points to:", *ptr) // dereference ptr → get value of x, which is 10
}

```

---

### 🔹 Example 1: Pointer with Basic Type

```go
package main
import "fmt"

func main() {
    x := 10
    ptr := &x               // pointer to x
    fmt.Println(*ptr)       // prints 10
    *ptr = 20               // modifies x
    fmt.Println(x)          // prints 20
}

```

---

### 🔹 Example 2: Pointer with Function

```go
package main
import "fmt"

func updateName(name *string) {
    *name = "Updated"
}

func main() {
    name := "Original"
    updateName(&name)
    fmt.Println(name) // Updated
}

```

---

### 🔹 Example 3: Pointer with Struct

```go
package main
import "fmt"

type User struct {
    Name  string
    Email string
}

func updateUser(u *User, name, email string) {
    u.Name = name
    u.Email = email
}

func main() {
    user := User{Name: "John", Email: "john@mail.com"}
    updateUser(&user, "Jane", "jane@mail.com")
    fmt.Println(user.Name) // Jane
}

```

---

### 🔹 Example 4: Efficient with Large Struct

```go
type Large struct {
    Data [1000]int
}

func modify(ls *Large) {
    ls.Data[0] = 999
}

func main() {
    ls := Large{}
    modify(&ls)
    fmt.Println(ls.Data[0]) // 999
}

```

---

### ✅ Key Points

- `&var` → get address of variable.
- `ptr` → dereference (get/set value).
- Use **pointers with functions** if you want to **modify original data**.
- Use **pointers for large structs** to avoid memory copy.
- **No pointer arithmetic in Go** (safe & simple).

# 🔹 Java vs Go → Pointers / References

### 1. **Primitives (int, float, boolean, etc.)**

- **Java** → Passed by value (copy of the value).
- **Go** → Same: passed by value. If you want to modify original → use pointer ().

**Java Example**

```java
public class Main {
    static void changeValue(int x) {
        x = 20;
    }
    public static void main(String[] args) {
        int a = 10;
        changeValue(a);
        System.out.println(a); // 10 (not changed)
    }
}

```

**Go Example**

```go
func changeValue(x int) {
    x = 20
}
func main() {
    a := 10
    changeValue(a)
    fmt.Println(a) // 10 (not changed)
}

```

✅ Same behavior. Both copy the value.

👉 In Go, if you want to modify, pass **pointer**:

```go
func changeValue(x *int) {
    *x = 20
}

```

---

### 2. **Objects / Structs**

- **Java** → Objects are always **references by default**.
- **Go** → Structs are **values by default** (copy). If you want reference-like behavior → use `Struct`.

**Java Example**

```java
class User {
    String name;
    User(String name) { this.name = name; }
}
class Main {
    static void update(User u) { u.name = "Updated"; }
    public static void main(String[] args) {
        User u = new User("Original");
        update(u);
        System.out.println(u.name); // Updated
    }
}

```

👉 In Java, we didn’t write `*` or `&`. But `u` is internally a **reference** (memory address).

**Go Example**

```go
type User struct { Name string }

func update(u *User) {
    u.Name = "Updated"
}

func main() {
    u := User{Name: "Original"}
    update(&u)
    fmt.Println(u.Name) // Updated
}

```

👉 In Go, we **explicitly** pass `&u` (address), and dereference with `*`.

---

### 3. **Large Structs**

- **Java** → Always references, no copy overhead.
- **Go** → By default struct is copied → inefficient if large → so we use pointers ().

---

### 🔑 Mental Mapping for Java Developers

| Concept | Java | Go |
| --- | --- | --- |
| Primitive vars | Passed by value | Passed by value |
| Objects / Arrays | Implicit references | By value (need `*` for reference) |
| Changing object inside function | Works directly | Need pointer (`*`) |
| Pointer syntax | Hidden | Explicit (`*`, `&`) |
| Pointer arithmetic | ❌ Not allowed | ❌ Not allowed |

---

✅ So:

- In **Java**, everything except primitives is **already a reference**.
- In **Go**, everything is **by value by default**. If you want reference behavior, you must **explicitly use pointers**.

## 🔹 Java Classes

- In Java, **everything is reference type** (except primitives).
- When you pass an object to a method, you are **always passing its reference (address)**.
- So changes made inside the method affect the original object.

Example:

```java
class Person {
    String name;
    int age;

    void changeName(String newName) {
        this.name = newName;  // modifies original object
    }
}

public class Main {
    public static void main(String[] args) {
        Person p = new Person();
        p.name = "Nishant";
        p.age = 28;

        p.changeName("Millo");   // modifies p
        System.out.println(p.name); // Output: Millo
    }
}
```

---

## 🔹 Go Structs

- In Go, a **struct is a value type**.
- When you pass a struct to a function **without a pointer**, Go makes a **copy** of it.
- Modifying the copy does **not** affect the original.

Example:

```go
package main
import "fmt"

type Person struct {
    Name string
    Age  int
}

// Value receiver (copy)
func (p Person) ChangeName(newName string) {
    p.Name = newName
}

// Pointer receiver (reference)
func (p *Person) ChangeNamePointer(newName string) {
    p.Name = newName
}

func main() {
    p := Person{Name: "Nishant", Age: 28}

    p.ChangeName("Millo")       // works on a copy
    fmt.Println(p.Name)         // Output: Nishant

    p.ChangeNamePointer("Millo") // works on actual object
    fmt.Println(p.Name)          // Output: Millo
}
```

---

## ✅ Quick Rule to Remember

- **Java → always reference** (changes affect original object).
- **Go → by default copy** (use `Person` if you want Java-like reference behavior).

---

👉 So in Go:

- `func (p Person)` = works on **copy**
- `func (p *Person)` = works on **original**