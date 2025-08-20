
## What is Gorilla/mux?

- Gorilla **mux** is a powerful HTTP router and URL matcher for Go.
    
- Extends Go’s `net/http` with:
    
    - Path parameters (`/users/{id}`)
        
    - Route matching by HTTP method (GET, POST, etc.)
        
    - Subrouters for grouping routes
        
    - Middleware support
        

---

## Installation

```bash
go get -u github.com/gorilla/mux
```

---

## Example: Basic Server

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"github.com/gorilla/mux"
)

func main() {
	r := mux.NewRouter()
	r.HandleFunc("/", HomeHandler).Methods("GET")
	r.HandleFunc("/users/{id}", UserHandler).Methods("GET")

	fmt.Println("Server running at http://localhost:8080")
	log.Fatal(http.ListenAndServe(":8080", r))
}

func HomeHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "Welcome to Home!")
}

func UserHandler(w http.ResponseWriter, r *http.Request) {
	vars := mux.Vars(r)
	id := vars["id"]
	fmt.Fprintf(w, "User ID: %s\n", id)
}
```

---

## Features

### Path Variables

```go
r.HandleFunc("/products/{category}/{id}", func(w http.ResponseWriter, r *http.Request) {
	vars := mux.Vars(r)
	fmt.Fprintf(w, "Category: %s, ID: %s", vars["category"], vars["id"])
})
```

### Query Parameters

```go
r.HandleFunc("/search", func(w http.ResponseWriter, r *http.Request) {
	query := r.URL.Query().Get("q")
	fmt.Fprintf(w, "You searched for: %s", query)
})
```

### Method Restriction

```go
r.HandleFunc("/submit", SubmitHandler).Methods("POST")
```

### Subrouters

```go
api := r.PathPrefix("/api").Subrouter()
api.HandleFunc("/users", UsersHandler).Methods("GET")
api.HandleFunc("/posts", PostsHandler).Methods("GET")
```

---

## When to Use

- Need **path parameters**
    
- Want **grouped routes** with subrouters
    
- Need more than Go’s default `http.ServeMux`