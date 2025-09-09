# Go Operator

### 1️⃣ Arithmetic Operators

- `+` → Addition
- → Subtraction
- → Multiplication
- `/` → Division
- `%` → Modulus

```go
a, b := 10, 3
fmt.Println("a+b:", a+b)
fmt.Println("a-b:", a-b)
fmt.Println("a*b:", a*b)
fmt.Println("a/b:", a/b)
fmt.Println("a%b:", a%b)
```

**Output:**

```
a+b: 13
a-b: 7
a*b: 30
a/b: 3
a%b: 1
```

---

### 2️⃣ Relational Operators

- `==` → Equal
- `!=` → Not equal
- `<` → Less than
- `>` → Greater than
- `<=` → Less than or equal
- `>=` → Greater than or equal

```go
x, y := 5, 10
fmt.Println("x==y:", x==y)
fmt.Println("x!=y:", x!=y)
fmt.Println("x<y:", x<y)
fmt.Println("x>y:", x>y)
fmt.Println("x<=y:", x<=y)
fmt.Println("x>=y:", x>=y)
```

**Output:**

```
x==y: false
x!=y: true
x<y: true
x>y: false
x<=y: true
x>=y: false
```

---

### 3️⃣ Logical Operators

- `&&` → AND
- `||` → OR
- `!` → NOT

```go
p, q := true, false
fmt.Println("p && q:", p && q)
fmt.Println("p || q:", p || q)
fmt.Println("!p:", !p)
```

**Output:**

```
p && q: false
p || q: true
!p: false
```

---

### 4️⃣ Assignment Operators

- `=` → Assign
- `+=` → Add and assign
- `=` → Subtract and assign
- `=` → Multiply and assign
- `/=` → Divide and assign
- `%=` → Modulus and assign

```go
n := 10
n += 5
fmt.Println("n += 5:", n)
n *= 2
fmt.Println("n *= 2:", n)
```

**Output:**

```
n += 5: 15
n *= 2: 30
```

---

### 5️⃣ Misc Operators

- `&` → Bitwise AND
- `|` → Bitwise OR
- `^` → Bitwise XOR
- `<<` → Left shift
- `>>` → Right shift

```go
x := 5  // 0101
y := 3  // 0011
fmt.Println("x & y:", x & y)
fmt.Println("x | y:", x | y)
fmt.Println("x ^ y:", x ^ y)
fmt.Println("x << 1:", x << 1)
fmt.Println("x >> 1:", x >> 1)
```

**Output:**

```
x & y: 1
x | y: 7
x ^ y: 6
x << 1: 10
x >> 1: 2
```