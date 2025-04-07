# 🌐 Simple Go Web Server

This is a basic web server built with Go (`net/http`) that demonstrates handling different HTTP requests, serving static files, and processing form submissions.

## 🚀 Features

- Serves static files from the `./static` directory
- Handles GET requests on `/hello`
- Handles POST form submissions on `/form`
- Basic error handling for invalid methods or routes

## 🛠️ Tech Stack

- **Language:** Go (Golang)
- **Standard Library:** `net/http`, `fmt`, `log`

## 📁 Project Structure

```
.
├── main.go         // Entry point of the server
└── static/         // Directory for static files (HTML, CSS, JS, etc.)
```

## 🧪 Endpoints

### GET `/hello`

Returns a simple greeting message.

**Response:**

```
Hello Jahid!!
```

### POST `/form`

Accepts form data (name and email), logs it to the console.

**Expected Form Fields:**

- `name`
- `email`

### Static Files

All other routes serve files from the `static/` directory.

## 🏃 How to Run

1. Make sure you have [Go installed](https://golang.org/doc/install)
2. Clone the repo and navigate to the project folder
3. Add some HTML files to the `static/` folder (like `index.html`, `form.html`)
4. Run the server:

```bash
go run main.go
```

5. Visit: [http://localhost:8000](http://localhost:8000)

## 📂 Source Code

```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func helloHandler(w http.ResponseWriter, r *http.Request) {
	if r.URL.Path != "/hello" {
		http.Error(w, "404 not found", http.StatusNotFound)
		return
	}

	if r.Method != "GET" {
		http.Error(w, "Method isn't Supported", http.StatusNotFound)
		return
	}
	fmt.Fprintln(w, "Hello Jahid!!")
}

func formHandler(w http.ResponseWriter, r *http.Request) {
	if err := r.ParseForm(); err != nil {
		fmt.Fprintf(w, "ParseForm() error: %v", err)
		return
	}
	fmt.Println("POST Request Successfully Submitted!")
	name := r.FormValue("name")
	email := r.FormValue("email")

	fmt.Println(name)
	fmt.Println(email)
}

func main() {
	fileServer := http.FileServer(http.Dir("./static"))

	http.Handle("/", fileServer)
	http.HandleFunc("/hello", helloHandler)
	http.HandleFunc("/form", formHandler)

	fmt.Println("Server is running at :8000")
	if err := http.ListenAndServe(":8000", nil); err != nil {
		log.Fatal(err)
	}
}
```

## 📬 Example HTML Form

Put this in `static/form.html`:

```html
<form action="/form" method="POST">
  <input type="text" name="name" placeholder="Your name" />
  <input type="email" name="email" placeholder="Your email" />
  <button type="submit">Submit</button>
</form>
```

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

