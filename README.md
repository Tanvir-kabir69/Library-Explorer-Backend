# 📖 Project Name: Library Explorer Backend

## 🎯 Objective

Develop a **Library Management System** using **Express**, **TypeScript**, and **MongoDB (via Mongoose)**.

* * *

## 🔧 Core technologies:

*   Language and framework : **Jaascript** and **Typescript**   
*   Rruntime and frameworks:  **node.js** using **express.js**
*   Database and frameworks: **MongoDB** using **Mongoose**

Projects Features:

*   Proper schema validation
*   Business logic enforcement (e.g., availability control on borrow)
*   Use of aggregation pipeline
*   Use of **Mongoose static or instance method**
*   Use of **Mongoose middleware**
*   Filtering and pagination features


### **Project Setup**

1. Download or clone repository
2. Open the project with a code editor 
3. Install the dependencies in package.json file
4. Open terminal and run "npm run dev"
5. Export postman.json to the postman for API testing
6. Now you are ready to go.
7. Test API

* * *
# API and Project Details :
### Book Model Fields & Validation

*   **title** (string) — Mandatory field. The book’s title.
*   **author** (string) — Mandatory field. The book’s author.
*   **genre** (string) — Mandatory field. One of: `FICTION`, `NON_FICTION`, `SCIENCE`, `HISTORY`, `BIOGRAPHY`, `FANTASY`.
*   **isbn** (string) — Mandatory and unique. The book’s International Standard Book Number.
*   **description** (string) — Optional. A brief summary or description of the book.
*   **copies** (number) — Mandatory. Non-negative integer representing total copies available.
*   **available** (boolean) — Defaults to `true`. Indicates if the book is currently available for borrowing.

* * *

### Borrow Model Fields & Validation

*   **book** (objectId) — Mandatory. References the borrowed book’s ID.
*   **quantity** (number) — Mandatory. Positive integer representing the number of copies borrowed.
*   **dueDate** (date) — Mandatory. The date by which the book must be returned.

* * *

* * *

## ✨ API Section

### 1\. Create Book

**POST** `/api/books`

#### Request:

```json
{
"title": "The Theory of Everything",
"author": "Stephen Hawking",
"genre": "SCIENCE",
"isbn": "9780553380163",
"description": "An overview of cosmology and black holes.",
"copies": 5,
"available": true
}
```

#### Response:

```json
{
"success": true,
"message": "Book created successfully",
"data": {
"_id": "64f123abc4567890def12345",
"title": "The Theory of Everything",
"author": "Stephen Hawking",
"genre": "SCIENCE",
"isbn": "9780553380163",
"description": "An overview of cosmology and black holes.",
"copies": 5,
"available": true,
"createdAt": "2024-11-19T10:23:45.123Z",
"updatedAt": "2024-11-19T10:23:45.123Z"
}
}
```

* * *

### 2\. Get All Books(Query Supported)

**GET** `/api/books`

Supports filtering, and sorting, limiting.

#### Query Forms:

`/api/books?filter=FANTASY&sortBy=createdAt&sort=desc&limit=5`

#### Query Parameters:

*   `filter`: Filter by genre
*   `sort`: `asc` or `desc`
*   `limit`: Number of results (default: 10)

#### Response:

```json
{
"success": true,
"message": "Books retrieved successfully",
"data": [
{
"_id": "64f123abc4567890def12345",
"title": "The Theory of Everything",
"author": "Stephen Hawking",
"genre": "SCIENCE",
"isbn": "9780553380163",
"description": "An overview of cosmology and black holes.",
"copies": 5,
"available": true,
"createdAt": "2024-11-19T10:23:45.123Z",
"updatedAt": "2024-11-19T10:23:45.123Z"
}
{...}
]
}
```

* * *

### 3\. Get Book by ID

**GET** `/api/books/:bookId`

#### Response:

```json
{
"success": true,
"message": "Book retrieved successfully",
"data": {
"_id": "64f123abc4567890def12345",
"title": "The Theory of Everything",
"author": "Stephen Hawking",
"genre": "SCIENCE",
"isbn": "9780553380163",
"description": "An overview of cosmology and black holes.",
"copies": 5,
"available": true,
"createdAt": "2024-11-19T10:23:45.123Z",
"updatedAt": "2024-11-19T10:23:45.123Z"
}
}
```

* * *

### 4\. Update Book

**PUT** `/api/books/:bookId`

#### Request:

```json
{
"copies": 50
}
```

#### Response:

```json
{
"success": true,
"message": "Book updated successfully",
"data": {
"_id": "64f123abc4567890def12345",
"title": "The Theory of Everything",
"author": "Stephen Hawking",
"genre": "SCIENCE",
"isbn": "9780553380163",
"description": "An overview of cosmology and black holes.",
"copies": 50,
"available": true,
"createdAt": "2024-11-19T10:23:45.123Z",
"updatedAt": "2024-11-20T08:30:00.000Z"
}
}
```

* * *

### 5\. Delete Book

**DELETE** `/api/books/:bookId`

#### Response:

```json
{
"success": true,
"message": "Book deleted successfully",
"data": null
}
```

* * *

### 6\. Borrow a Book

**POST** `/api/borrow`

#### Business Logic:

*   Before Borrowing, verifies the book has enough available copies.
*   After Borrowing, Deducts the requested quantity from the book’s copies and add it to the  borrow quantity.
*   After Borrowing, if copies become 0, update with `available` to `false` (using mongoose static method).

#### Request:

```json
{
"book": "64ab3f9e2a4b5c6d7e8f9012",
"quantity": 2,
"dueDate": "2025-07-18T00:00:00.000Z"
}
```

#### Response:

```json
{
"success": true,
"message": "Book borrowed successfully",
"data": {
"_id": "64bc4a0f9e1c2d3f4b5a6789",
"book": "64ab3f9e2a4b5c6d7e8f9012",
"quantity": 2,
"dueDate": "2025-07-18T00:00:00.000Z",
"createdAt": "2025-06-18T07:12:15.123Z",
"updatedAt": "2025-06-18T07:12:15.123Z"
}
}
```

* * *

### 7\. Borrowed Books Summary

`GET /api/borrow`

**Working Principle :**

Returns a summary of borrowed books, including:

*  Total borrowed quantity per book (`totalQuantity`)
*    Book details: `title` and `isbn`

**Response:**

```json
{
"success": true,
"message": "Borrowed books summary retrieved successfully",
"data": [
{
"book": {
"title": "The Theory of Everything",
"isbn": "9780553380163"
},
"totalQuantity": 5
},
{
"book": {
"title": "1984",
"isbn": "9780451524935"
},
"totalQuantity": 3
}
]
}
```
### Generic Error Response

Handles Error very efficient way. Error Response incluses:  

1. **`message`**: A brief error message explaining what went wrong.
2. **`success`**: Set to `false` for error responses.
3. **`error`**: The error message or error object returned by the application 

```json
{
"message": "Validation failed",
"success": false,
"error": {
"name": "ValidationError",
"errors": {
"copies": {
"message": "Copies must be a positive number",
"name": "ValidatorError",
"properties": {
"message": "Copies must be a positive number",
"type": "min",
"min": 0
},
"kind": "min",
"path": "copies",
"value": -5
}
}
}
}
```

* * *

## ✨ MAINTAINING:

*   **Code Quality:** Clean, readable code with meaningful names.
*   **API Structure:** Follows mentioned endpoints and response formats exactly.
*   **Error Handling:** Handles invalid input, 404s, and validation errors clearly.
*   **Documentation:** Well-written [README.md](http://readme.md/) with setup and API details.
