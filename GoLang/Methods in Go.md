

In Go, methods are functions that are associated with a specific type. Unlike regular functions, methods have a **receiver**, which allows them to be called on instances of that type. This is similar to how methods work on classes in other languages like Java or Python.

---

#### 1. Defining a Method

A method is defined just like a function, but with an extra parameter called the **receiver**, placed before the function name.

```go
func (receiver Type) MethodName(params) returnType {
    // method body
}
```

- **receiver**: the type the method is attached to (can be a struct or any named type).
    
- **Type**: the receiver type.
    
- **MethodName**: name of the method.
    
- **params**: additional input parameters.
    
- **returnType**: return type of the method.
    

---

#### 2. Example with Struct

```go
package main

import "fmt"

// Define a struct
type User struct {
    Name  string
    Email string
}

// Method with value receiver (does not modify original struct)
func (u User) Greet() {
    fmt.Println("Hello,", u.Name)
}

// Method with pointer receiver (can modify original struct)
func (u *User) UpdateEmail(newEmail string) {
    u.Email = newEmail
}

func main() {
    u := User{Name: "Alice", Email: "old@email.com"}

    u.Greet()                         // calls value receiver method
    u.UpdateEmail("new@email.com")    // calls pointer receiver method
    fmt.Println("Updated Email:", u.Email)
}
```

---

#### 3. Value vs Pointer Receivers

- **Value Receiver** (`func (u User) ...`)  
    The method operates on a copy of the value. Changes inside the method do not affect the original instance.
    
- **Pointer Receiver** (`func (u *User) ...`)  
    The method operates on a pointer to the value. Changes inside the method affect the original instance.
    

---

#### 4. Analogy with Classes in Other Languages

- **Struct** = fields of a class.
    
- **Methods with receivers** = class methods.
    
- **Pointer receiver** = allows modification of the object (similar to `self` in Python or `this` in Java).
    

---

#### 5. Key Notes

- Methods in Go are syntactic sugar: `u.Greet()` is actually `User.Greet(u)`.
    
- Methods can be defined on any named type, not just structs.
    
- Go does not have classes, but by combining **structs** and **methods**, you can achieve object-oriented behavior.
    

---
