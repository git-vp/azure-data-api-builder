# Test APIs
APIs provided by Data API Builder (DAB) can be tested using several methods like:
* Browser
* curl
* POSTMAN
  
## Test APIs in Browser
1. Open a browser and enter `http://localhost:5000/api/Book`. This should show all the books.
2. Enter `http://localhost:5000/api/Author` in the browser. This should show all the authors.

## Test APIs in POSTMAN
1. If Entra ID is used for DAB Auth, follow these steps to retrieve the token:
   
   * To retrieve API bearer token:

    `az account get-access-token --scope api://<dab-entraid-app-reg-guid>/endpoint.access --query "accessToken" --tenant <entraid-tenant-id> -o tsv`

   * The above command may prompt the user to login at:

    `az login --scope api://<dab-entraid-app-reg-guid>/Endpoint.Access --tenant <tenant-name>.onmicrosoft.com`

   * Set the AUTHORIZATION Type to Bearer, and Value to the bearer token retrieved from above
   * Set X-MS-API-ROLE to either `editor` or `contributor` roles created. For details on how to create these roles see [DAB EntraID Auth](https://github.com/git-vp/azure-data-api-builder/edit/main/dab-aad-auth.md)

    
2. To retrieve all books in POSTMAN using REST API:
     Method: `GET`
     URL: `http://localhost:5000/api/Book`
   
3. To retrieve all authors in POSTMAN using REST API:
     Method: `GET`
     URL: `http://localhost:5000/api/Author`
   
4. To retrieve first 5 books using GraphQL:
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
