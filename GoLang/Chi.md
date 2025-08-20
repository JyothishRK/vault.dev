# Chi – Beginner Guide  

## What is Chi?  
* Chi is a lightweight, idiomatic HTTP router for Go.  
* Focused on simplicity, speed, and composability.  
* Fully compatible with Go’s `net/http`.  
* Great for building REST APIs with middleware support.  

---

## Installation  

```bash
go get github.com/go-chi/chi/v5
```

---

## Example: Basic Server  

```go
package main

import (
	"fmt"
	"net/http"
	"github.com/go-chi/chi/v5"
)

func main() {
	r := chi.NewRouter()

	r.Get("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintln(w, "Welcome Home!")
	})

	r.Get("/users/{id}", func(w http.ResponseWriter, r *http.Request) {
		id := chi.URLParam(r, "id")
		fmt.Fprintf(w, "User ID: %s", id)
	})

	http.ListenAndServe(":8080", r)
}
```

---

## Features  

### Path Parameters  

```go
r.Get("/articles/{category}/{id}", func(w http.ResponseWriter, r *http.Request) {
	category := chi.URLParam(r, "category")
	id := chi.URLParam(r, "id")
	fmt.Fprintf(w, "Category: %s, ID: %s", category, id)
})
```

### Query Parameters  

```go
r.Get("/search", func(w http.ResponseWriter, r *http.Request) {
	query := r.URL.Query().Get("q")
	fmt.Fprintf(w, "Searching for: %s", query)
})
```

### Restrict Methods  

```go
r.Post("/submit", func(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintln(w, "Form Submitted!")
})
```

### Route Groups / Subroutes  

```go
r.Route("/api", func(r chi.Router) {
	r.Get("/users", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintln(w, "List of users")
	})
	r.Get("/posts", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintln(w, "List of posts")
	})
})
```

### Middleware Example  

```go
r.Use(loggingMiddleware)

func loggingMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		fmt.Println("Request:", r.Method, r.URL.Path)
		next.ServeHTTP(w, r)
	})
}
```

---

## When to Use  

* Building REST APIs in Go  
* Need a fast, minimal router  
* Want middleware chaining and route grouping  
* Prefer idiomatic Go style  