<<<<<<< HEAD
# Library Management System API Design

## PART 1: Understand the Scenario

The Library Management System API manages members, books, loans, and categories.

The system supports:
a
* Registering new members
* Managing books
* Borrowing and returning books
* Managing book categories
* Viewing active and overdue loans
## PART 2: Identify Resources

### Resources

1. Members
2. Books
3. Loans
4. Categories

### Members Properties

* id
* first_name
* last_name
* email
* phone
* created_at
* updated_at

### Books Properties

* id
* title
* author
* isbn
* published_year
* category_id
* available_copies
* created_at
* updated_at

### Loans Properties

* id
* member_id
* book_id
* borrowed_at
* due_date
* returned_at
* status
* created_at
* updated_at

### Categories Properties

* id
* name
* description
* created_at
* updated_at
## PART 3: Design Endpoints

### Members

* GET /api/members → Get all members
* GET /api/members/:id → Get one member by ID
* POST /api/members → Create a new member
* PUT /api/members/:id → Update member
* PATCH /api/members/:id → Partially update member
* DELETE /api/members/:id → Delete member

### Books

* GET /api/books → Get all books
* GET /api/books/:id → Get one book by ID
* POST /api/books → Create a new book
* PUT /api/books/:id → Update book
* PATCH /api/books/:id → Partially update book
* DELETE /api/books/:id → Delete book

### Loans

* GET /api/loans → Get all loans
* GET /api/loans/:id → Get one loan by ID
* POST /api/loans → Create a new loan
* PUT /api/loans/:id → Update loan
* PATCH /api/loans/:id → Partially update loan
* DELETE /api/loans/:id → Delete loan

### Categories

* GET /api/categories → Get all categories
* GET /api/categories/:id → Get one category by ID
* POST /api/categories → Create a new category
* PUT /api/categories/:id → Update category
* PATCH /api/categories/:id → Partially update category
* DELETE /api/categories/:id → Delete category

### Nested Resources

* GET /api/members/:id/loans → Get all loans for a member
* POST /api/members/:id/loans → Create a loan for a member
* GET /api/categories/:id/books → Get all books in a category
* GET /api/books/:id/loans → Get all loans for a book
## PART 4: Define Request & Response Formats

### POST /api/members

*Request Body:*

{
  "first_name": "Juan",
  "last_name": "Dela Cruz",
  "email": "juan@example.com",
  "phone": "09171234567"
}

### POST /api/loans

*Request Body:*

{
  "member_id": 1,
  "book_id": 101,
  "due_date": "2026-09-24"
}

### PATCH /api/books/:id

*Request Body:*

{
  "available_copies": 4
}

### GET /api/members/:id

*Success Response (200 OK):*

{
  "id": 1,
  "first_name": "tony",
  "last_name": "Dela Cruz",
  "email": "tony.delacruz@example.com",
  "phone": "09171234567"
}
*Error Response (404 Not Found):*

{
  "error": {
    "code": "MEMBER_NOT_FOUND",
    "message": "Member with ID 1 does not exist"
  }
}

### POST /api/loans

*Success Response (201 Created):*

{
  "id": 1001,
  "member_id": 1,
  "book_id": 101,
  "borrowed_at": "2026-09-10T10:30:00Z",
  "due_date": "2026-09-24",
  "status": "active"
}

*Error Response (409 Conflict):*

{
  "error": {
    "code": "BOOK_NOT_AVAILABLE",
    "message": "This book has no available copies for borrowing"
  }
}
### DELETE /api/books/:id

*Success Response (204 No Content):*

No response body. The book is successfully deleted.

## PART 5: Choose Status Codes

### GET /api/books/:id

* Success: 200 OK
* Bad Request: 400 Bad Request
* Not Found: 404 Not Found
* Server Error: 500 Internal Server Error

### POST /api/members

* Created: 201 Created
* Bad Request: 400 Bad Request
* Conflict: 409 Conflict
* Server Error: 500 Internal Server Error

### POST /api/loans

* Created: 201 Created
* Bad Request: 400 Bad Request
* Not Found: 404 Not Found
* Conflict: 409 Conflict
* Server Error: 500 Internal Server Error
### DELETE /api/books/:id

* Success: 204 No Content
* Bad Request: 400 Bad Request
* Not Found: 404 Not Found
* Conflict: 409 Conflict
* Server Error: 500 Internal Server Error

### Special Scenarios

| Scenario                                 | Status Code | Error Code              | Error Message                                            |
| ---------------------------------------- | ----------: | ----------------------- | -------------------------------------------------------- |
| Borrowing an unavailable book            |         409 | BOOK_NOT_AVAILABLE    | This book has no available copies for borrowing.         |
| Returning an already-returned loan       |         409 | LOAN_ALREADY_RETURNED | This loan has already been returned.                     |
| Deleting a book with active loans        |         409 | BOOK_HAS_ACTIVE_LOANS | This book cannot be deleted because it has active loans. |
| Creating a member with a duplicate email |         409 | EMAIL_ALREADY_EXISTS  | A member with this email already exists.                 |
## PART 6: Advanced Features

### Book Search, Filtering, Sorting, and Pagination

GET /api/books

* ?search=programming → Search by title, author, or ISBN
* ?category_id=3 → Filter by category
* ?sort=published_year → Sort by publication year
* ?order=desc → Sort order
* ?page=1&limit=10 → Pagination

*Example:*

text
GET /api/books?search=programming&category_id=3&sort=published_year&order=desc&page=1&limit=10

### Loan Filtering and Pagination

GET /api/loans

* ?status=active → Filter active loans
* ?status=returned → Filter returned loans
* ?status=overdue → Filter overdue loans
* ?member_id=1 → Filter by member
* ?page=1&limit=10 → Pagination

*Example:*

text
GET /api/loans?status=active&member_id=1&page=1&limit=10
### API Versioning

* Current Version: v1
* New Version: v2

*Breaking Change:* Replace available_copies with total_copies and borrowed_copies.

*Migration Strategy:* Support both v1 and v2 for 6 months.

*Deprecation Timeline:* Remove v1 after 6 months.

## PART 7: Final Documentation

### 1. Overview

The Library Management System API is a RESTful API used to manage members, books, loans, and categories.

### 2. Base URL

*Production:*

text
https://api.library.com/v1

*Development:*

text
http://localhost:3000/api

### 3. Resources

The main resources are:

* Members
* Books
* Loans
* Categories

### 4. Endpoints

The API uses:

* GET for retrieving resources
* POST for creating resources
* PUT for replacing resources
* PATCH for partially updating resources
* DELETE for deleting resources

*Main endpoints:*

text
/api/members
/api/books
/api/loans
/api/categories### 5. Authentication

The API will use JWT (JSON Web Tokens) for authentication.

text
Authorization: Bearer <JWT_TOKEN>

Authentication will be implemented in the Final period.

### 6. Error Handling

The API uses standard HTTP status codes and JSON error responses.

{
  "error": {
    "code": "BOOK_NOT_FOUND",
    "message": "The requested book was not found."
  }
}

### 7. Special Scenarios

* Unavailable book → 409 BOOK_NOT_AVAILABLE
* Already returned loan → 409 LOAN_ALREADY_RETURNED
* Book with active loans → 409 BOOK_HAS_ACTIVE_LOANS
* Duplicate member email → 409 EMAIL_ALREADY_EXISTS

### 8. Future Enhancements

* Book reservation system
* Email notifications
* Fine calculation
* Admin and member roles
* Advanced search
* Reports and statistics
* API version 2
=======

>>>>>> f6beac45749ff025ff619c0d93ab79d1bb37e8c1


