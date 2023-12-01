# Test APIs
## Test APIs in Browser
1. Open a browser and enter `http://localhost:5000/api/Book`. This should show all the books.
2. Enter `http://localhost:5000/api/Author` in the browser. This should show all the authors.

## Test APIs in POSTMAN
1. To retrieve all books in POSTMAN using REST API:
     Method: `GET`
     URL: `http://localhost:5000/api/Book`
2. To retrieve all authors in POSTMAN using REST API:
     Method: `GET`
     URL: `http://localhost:5000/api/Author`
3. To retrieve first 5 books using GraphQL:
     URL: `http://localhost:5000/api/Author`
     Query:
     ```
      {
        books(first: 5, orderBy: { title: DESC }) {
          items {
            id
            title
          }
        }
      }
     ```
