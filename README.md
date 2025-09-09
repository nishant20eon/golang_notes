# Go Programming Language Notes

A comprehensive guide to Go programming language concepts, syntax, and best practices.

## Table of Contents

1. [Introduction](#introduction)
2. [Basic Syntax](#basic-syntax)
3. [Variables and Data Types](#variables-and-data-types)
4. [Control Flow](#control-flow)
5. [Functions](#functions)
6. [Arrays and Slices](#arrays-and-slices)
7. [Maps](#maps)
8. [Structs](#structs)
9. [Methods and Interfaces](#methods-and-interfaces)
10. [Error Handling](#error-handling)
11. [Concurrency (Goroutines and Channels)](#concurrency)
12. [Packages and Modules](#packages-and-modules)
13. [Best Practices](#best-practices)

## Introduction

Go is a statically typed, compiled programming language designed at Google. It's known for its simplicity, efficiency, and excellent support for concurrent programming.

### Key Features:
- Fast compilation
- Garbage collection
- Built-in concurrency support
- Simple syntax
- Strong standard library
- Cross-platform support

## Basic Syntax

### Hello World Program

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

### Program Structure
- Every Go program starts with a `package` declaration
- `import` statements bring in external packages
- `main()` function is the entry point for executable programs
- Statements don't need semicolons (automatic insertion)

## Variables and Data Types

### Variable Declaration

```go
// Method 1: var keyword with type
var name string = "John"
var age int = 30

// Method 2: var keyword with type inference
var name = "John"
var age = 30

// Method 3: Short declaration (inside functions only)
name := "John"
age := 30

// Multiple variable declaration
var (
    name string = "John"
    age  int    = 30
)
```

### Basic Data Types

```go
// Numeric types
var a int = 42
var b int8 = 127
var c int16 = 32767
var d int32 = 2147483647
var e int64 = 9223372036854775807

var f uint = 42
var g uint8 = 255
var h uint16 = 65535
var i uint32 = 4294967295
var j uint64 = 18446744073709551615

var k float32 = 3.14
var l float64 = 3.141592653589793

// Boolean
var isActive bool = true

// String
var message string = "Hello, Go!"

// Character (rune is an alias for int32)
var char rune = 'A'

// Byte (uint8 alias)
var b byte = 255
```

### Constants

```go
const PI = 3.14159
const (
    Monday = iota
    Tuesday
    Wednesday
    Thursday
    Friday
    Saturday
    Sunday
)
```

## Control Flow

### If Statements

```go
age := 18

if age >= 18 {
    fmt.Println("Adult")
} else if age >= 13 {
    fmt.Println("Teenager")
} else {
    fmt.Println("Child")
}

// If with initialization
if x := getRandomNumber(); x > 5 {
    fmt.Println("Greater than 5")
}
```

### Switch Statements

```go
day := "Monday"

switch day {
case "Monday":
    fmt.Println("Start of work week")
case "Friday":
    fmt.Println("TGIF!")
case "Saturday", "Sunday":
    fmt.Println("Weekend!")
default:
    fmt.Println("Midweek")
}

// Switch without expression (like if-else chain)
switch {
case age < 13:
    fmt.Println("Child")
case age < 18:
    fmt.Println("Teenager")
default:
    fmt.Println("Adult")
}
```

### Loops

```go
// For loop (traditional)
for i := 0; i < 5; i++ {
    fmt.Println(i)
}

// For loop (while-like)
i := 0
for i < 5 {
    fmt.Println(i)
    i++
}

// Infinite loop
for {
    // Do something forever
    break // Use break to exit
}

// Range loop
numbers := []int{1, 2, 3, 4, 5}
for index, value := range numbers {
    fmt.Printf("Index: %d, Value: %d\n", index, value)
}

// Range loop (index only)
for index := range numbers {
    fmt.Println(index)
}

// Range loop (value only)
for _, value := range numbers {
    fmt.Println(value)
}
```

## Functions

### Basic Function Syntax

```go
func add(a int, b int) int {
    return a + b
}

// Same type parameters can be grouped
func add(a, b int) int {
    return a + b
}

// Function with multiple returns
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// Named return values
func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return // naked return
}
```

### Variadic Functions

```go
func sum(numbers ...int) int {
    total := 0
    for _, num := range numbers {
        total += num
    }
    return total
}

// Usage
result := sum(1, 2, 3, 4, 5)
```

### Function as Values

```go
func main() {
    // Assign function to variable
    add := func(a, b int) int {
        return a + b
    }
    
    result := add(3, 4)
}

// Higher-order functions
func applyOperation(a, b int, op func(int, int) int) int {
    return op(a, b)
}
```

### Defer Statements

```go
func main() {
    defer fmt.Println("This executes last")
    fmt.Println("This executes first")
    defer fmt.Println("This executes second to last")
}

// Common use case: cleanup
func readFile(filename string) {
    file, err := os.Open(filename)
    if err != nil {
        return
    }
    defer file.Close() // Ensures file is closed
    
    // Work with file...
}
```

## Arrays and Slices

### Arrays

```go
// Array declaration
var numbers [5]int
numbers[0] = 1
numbers[1] = 2

// Array initialization
var numbers = [5]int{1, 2, 3, 4, 5}
numbers := [5]int{1, 2, 3, 4, 5}
numbers := [...]int{1, 2, 3, 4, 5} // Let compiler count

// Get array length
fmt.Println(len(numbers))
```

### Slices

```go
// Slice declaration
var numbers []int
numbers = append(numbers, 1, 2, 3)

// Slice initialization
numbers := []int{1, 2, 3, 4, 5}
numbers := make([]int, 5)      // length 5, capacity 5
numbers := make([]int, 5, 10)  // length 5, capacity 10

// Slice operations
slice1 := numbers[1:4]  // Elements at index 1, 2, 3
slice2 := numbers[:3]   // Elements from start to index 2
slice3 := numbers[2:]   // Elements from index 2 to end
slice4 := numbers[:]    // All elements

// Append to slice
numbers = append(numbers, 6, 7, 8)

// Copy slices
destination := make([]int, len(numbers))
copy(destination, numbers)

// Length and capacity
fmt.Println("Length:", len(numbers))
fmt.Println("Capacity:", cap(numbers))
```

## Maps

### Map Operations

```go
// Map declaration and initialization
var ages map[string]int
ages = make(map[string]int)

// Direct initialization
ages := map[string]int{
    "John":  30,
    "Jane":  25,
    "Alice": 35,
}

// Add/Update values
ages["Bob"] = 40

// Get values
age := ages["John"]

// Check if key exists
age, exists := ages["John"]
if exists {
    fmt.Println("John's age:", age)
}

// Delete key
delete(ages, "John")

// Iterate over map
for name, age := range ages {
    fmt.Printf("%s is %d years old\n", name, age)
}
```

## Structs

### Struct Definition and Usage

```go
// Define a struct
type Person struct {
    Name string
    Age  int
    City string
}

// Create struct instances
var p1 Person
p1.Name = "John"
p1.Age = 30

p2 := Person{
    Name: "Jane",
    Age:  25,
    City: "New York",
}

p3 := Person{"Alice", 35, "London"} // Positional

// Struct pointers
p4 := &Person{Name: "Bob", Age: 40}
fmt.Println(p4.Name) // Go automatically dereferences
```

### Embedded Structs

```go
type Address struct {
    Street string
    City   string
    Country string
}

type Person struct {
    Name    string
    Age     int
    Address // Embedded struct
}

// Usage
p := Person{
    Name: "John",
    Age:  30,
    Address: Address{
        Street:  "123 Main St",
        City:    "New York",
        Country: "USA",
    },
}

fmt.Println(p.Street) // Direct access to embedded field
```

## Methods and Interfaces

### Methods

```go
type Circle struct {
    Radius float64
}

// Method with value receiver
func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

// Method with pointer receiver
func (c *Circle) SetRadius(radius float64) {
    c.Radius = radius
}

// Usage
circle := Circle{Radius: 5}
fmt.Println("Area:", circle.Area())
circle.SetRadius(10)
```

### Interfaces

```go
// Define interface
type Shape interface {
    Area() float64
    Perimeter() float64
}

// Implement interface for Circle
func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * math.Pi * c.Radius
}

// Implement interface for Rectangle
type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}

// Use interface
func PrintShapeInfo(s Shape) {
    fmt.Printf("Area: %.2f, Perimeter: %.2f\n", s.Area(), s.Perimeter())
}

// Empty interface
var value interface{} = "Hello"
value = 42
value = true

// Type assertion
if str, ok := value.(string); ok {
    fmt.Println("String value:", str)
}

// Type switch
switch v := value.(type) {
case string:
    fmt.Println("String:", v)
case int:
    fmt.Println("Integer:", v)
case bool:
    fmt.Println("Boolean:", v)
default:
    fmt.Println("Unknown type")
}
```

## Error Handling

### Basic Error Handling

```go
import (
    "errors"
    "fmt"
)

// Function that returns an error
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// Usage
func main() {
    result, err := divide(10, 0)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Println("Result:", result)
}
```

### Custom Error Types

```go
type ValidationError struct {
    Field string
    Value interface{}
    Msg   string
}

func (e ValidationError) Error() string {
    return fmt.Sprintf("validation failed for %s with value %v: %s", 
        e.Field, e.Value, e.Msg)
}

func validateAge(age int) error {
    if age < 0 {
        return ValidationError{
            Field: "age",
            Value: age,
            Msg:   "age cannot be negative",
        }
    }
    return nil
}
```

### Panic and Recover

```go
func main() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered from panic:", r)
        }
    }()
    
    panic("Something went wrong!")
    fmt.Println("This won't be printed")
}
```

## Concurrency

### Goroutines

```go
import (
    "fmt"
    "time"
)

func sayHello(name string) {
    for i := 0; i < 3; i++ {
        fmt.Printf("Hello, %s! (%d)\n", name, i)
        time.Sleep(time.Millisecond * 100)
    }
}

func main() {
    // Start goroutines
    go sayHello("Alice")
    go sayHello("Bob")
    
    // Wait for goroutines to complete
    time.Sleep(time.Second)
}
```

### Channels

```go
// Basic channel operations
func main() {
    // Create channel
    ch := make(chan int)
    
    // Send data to channel (in goroutine)
    go func() {
        ch <- 42
    }()
    
    // Receive data from channel
    value := <-ch
    fmt.Println("Received:", value)
}

// Buffered channels
func main() {
    ch := make(chan int, 3) // Buffer size 3
    
    ch <- 1
    ch <- 2
    ch <- 3
    
    fmt.Println(<-ch)
    fmt.Println(<-ch)
    fmt.Println(<-ch)
}

// Channel direction restrictions
func send(ch chan<- int) {
    ch <- 42
}

func receive(ch <-chan int) {
    value := <-ch
    fmt.Println("Received:", value)
}
```

### Select Statement

```go
func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    
    go func() {
        time.Sleep(time.Second)
        ch1 <- "Channel 1"
    }()
    
    go func() {
        time.Sleep(2 * time.Second)
        ch2 <- "Channel 2"
    }()
    
    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-ch1:
            fmt.Println(msg1)
        case msg2 := <-ch2:
            fmt.Println(msg2)
        case <-time.After(3 * time.Second):
            fmt.Println("Timeout")
        }
    }
}
```

### Worker Pool Pattern

```go
func worker(id int, jobs <-chan int, results chan<- int) {
    for job := range jobs {
        fmt.Printf("Worker %d processing job %d\n", id, job)
        time.Sleep(time.Second)
        results <- job * 2
    }
}

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)
    
    // Start workers
    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }
    
    // Send jobs
    for j := 1; j <= 5; j++ {
        jobs <- j
    }
    close(jobs)
    
    // Collect results
    for r := 1; r <= 5; r++ {
        <-results
    }
}
```

## Packages and Modules

### Package Structure

```go
// In file: math/calculator.go
package math

// Exported function (starts with capital letter)
func Add(a, b int) int {
    return a + b
}

// Unexported function (starts with lowercase letter)
func subtract(a, b int) int {
    return a - b
}
```

### Import Statements

```go
import (
    "fmt"                    // Standard library
    "net/http"              // Standard library
    
    "github.com/user/repo"  // External package
    custom "github.com/user/repo/v2" // Aliased import
    . "fmt"                 // Dot import (not recommended)
    _ "github.com/user/repo" // Blank import (for side effects)
)
```

### Go Modules

```bash
# Initialize module
go mod init github.com/username/project

# Add dependency
go get github.com/gorilla/mux

# Update dependencies
go mod tidy

# Vendor dependencies
go mod vendor
```

### go.mod file example

```go
module github.com/username/project

go 1.19

require (
    github.com/gorilla/mux v1.8.0
    github.com/stretchr/testify v1.7.0
)

require (
    github.com/davecgh/go-spew v1.1.0 // indirect
    github.com/pmezard/go-difflib v1.0.0 // indirect
    gopkg.in/yaml.v3 v3.0.0-20200313102051-9f266ea9e77c // indirect
)
```

## Best Practices

### Code Organization

1. **Use meaningful names**: Variables, functions, and packages should have descriptive names
2. **Keep functions small**: Functions should do one thing well
3. **Handle errors properly**: Always check and handle errors appropriately
4. **Use interfaces wisely**: Design interfaces to define behavior, not data

### Naming Conventions

```go
// Good naming examples
type UserService struct{}
func (us *UserService) GetUserByID(id int) (*User, error) {}

var (
    ErrUserNotFound = errors.New("user not found")
    DefaultTimeout  = 30 * time.Second
)

// Constants
const (
    MaxRetries = 3
    APIVersion = "v1"
)
```

### Error Handling Best Practices

```go
// Don't ignore errors
result, err := someFunction()
if err != nil {
    return fmt.Errorf("failed to call someFunction: %w", err)
}

// Use early returns
func processData(data []byte) error {
    if len(data) == 0 {
        return errors.New("data cannot be empty")
    }
    
    if !isValid(data) {
        return errors.New("invalid data format")
    }
    
    // Main logic here
    return nil
}
```

### Concurrency Best Practices

```go
// Use context for cancellation
func processWithTimeout(ctx context.Context, data string) error {
    done := make(chan error, 1)
    
    go func() {
        // Simulate work
        time.Sleep(2 * time.Second)
        done <- nil
    }()
    
    select {
    case err := <-done:
        return err
    case <-ctx.Done():
        return ctx.Err()
    }
}

// Proper channel closing
func producer(ch chan<- int) {
    defer close(ch) // Always close channels when done sending
    
    for i := 0; i < 10; i++ {
        ch <- i
    }
}
```

### Testing

```go
// Example test file: calculator_test.go
package math

import "testing"

func TestAdd(t *testing.T) {
    result := Add(2, 3)
    expected := 5
    
    if result != expected {
        t.Errorf("Add(2, 3) = %d; want %d", result, expected)
    }
}

func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(2, 3)
    }
}
```

### Performance Tips

1. **Use slices instead of arrays** when possible
2. **Pre-allocate slices** when you know the size: `make([]int, 0, capacity)`
3. **Use string.Builder** for string concatenation in loops
4. **Avoid premature optimization** - profile first
5. **Use sync.Pool** for object reuse in high-frequency scenarios

```go
// String building example
var builder strings.Builder
for i := 0; i < 1000; i++ {
    builder.WriteString("hello ")
}
result := builder.String()
```

---

This guide covers the essential concepts of Go programming. For more advanced topics, refer to the official Go documentation at [golang.org](https://golang.org).