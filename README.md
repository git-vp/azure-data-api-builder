# Steps for Running Azure Data API Builder (DAB)
[Azure Data API Builder](https://learn.microsoft.com/en-us/azure/data-api-builder/overview-to-data-api-builder?tabs=azure-sql) provides an ability to auto-generate APIs to expose data in database like MS SQL.

This repo provides step-by-step instructions to run Azure Data API Builder in various environments, like running on a VM, Azure Container Instance, Docker etc

## Run DAB in a Local Windows Machine
### Install DAB
1. Data API Builder requires .NET 6. Install .NET 6
    - To install .NET 6 download MSI from: [DotNet](https://dotnet.microsoft.com/en-us/download/dotnet) and execute the MSI
2. Install DAB
    `dotnet tool install --global Microsoft.DataApiBuilder`
3. Update DAB
	`dotnet tool update --global Microsoft.DataApiBuilder`
4. Verify DAB installation
    `dab --version`

### Install SQL Database
* [Instructions for installing Azure SQL Database](https://github.com/git-vp/azure-data-api-builder/blob/main/install-sql-db.md)

### Generate DAB Configuration File
1. Create a configuration file for DAB
    `dab init --database-type "mssql" --connection-string "<replace-connection-string-from-above>" --host-mode "Development"`
		
2. The above command should generate a DAB configuration file such as below. The default configuration file name would be: `dab-config.json`
<details>
  <summary>Click here</summary>
  
  ```json
  		{
  		  "$schema": "dab.draft-01.schema.json",
  		  "data-source": {
  		    "database-type": "mssql",
  		    "connection-string": "Server=tcp:dab-demo-sql-vp.database.windows.net,1433;Initial Catalog=sqldb-books;Persist Security Info=False;User ID=<username>;Password=<password>;MultipleActiveResultSets=False;Encrypt=true;TrustServerCertificate=False;Connection Timeout=30;"
  		  },
  		  "mssql": {
  		    "set-session-context": true
  		  },
  		  "runtime": {
  		    "rest": {
  		      "enabled": true,
  		      "path": "/api"
  		    },
  		    "graphql": {
  		      "allow-introspection": true,
  		      "enabled": true,
  		      "path": "/graphql"
  		    },
  		    "host": {
  		      "mode": "development",
  		      "cors": {
  		        "origins": [],
  		        "allow-credentials": false
  		      },
  		      "authentication": {
  		        "provider": "Simulator"
  		      }
  		    }
  		  },
  		  "entities": {}
  		}
  ```
</details>

`Note:` When DAB is run locally, authentication can be set to either StaticWebApps or Simulator mode

3. 	To expose specific DB entities as APIs, add entity information to the `dab-config.json` [See additional details here](https://learn.microsoft.com/en-us/azure/data-api-builder/get-started/get-started-azure-sql#add-book-and-author-entities)

    Add Book entity to dab-config.json:
	`dab add Author --source dbo.authors --permissions "anonymous:*"`

    Author entity to dab-config.json
    `dab add Book--source dbo.books--permissions "anonymous:*"`

4. The final dab-config.json :
<details>
  <summary>Click here</summary>
  
  ```json		
    {
      "$schema": "https://github.com/Azure/data-api-builder/releases/download/v0.9.7/dab.draft.schema.json",
      "data-source": {
        "database-type": "mssql",
        "connection-string": "Server=tcp:dab-demo-sql-vp.database.windows.net,1433;Initial Catalog=sqldb-books;Persist Security Info=False;User ID=<username>;Password=<Password>;MultipleActiveResultSets=False;Encrypt=true;TrustServerCertificate=True;Connection Timeout=30;",
        "options": {
          "set-session-context": false
        }
      },
      "runtime": {
        "rest": {
          "enabled": true,
          "path": "/api",
          "request-body-strict": true
        },
        "graphql": {
          "enabled": true,
          "path": "/graphql",
          "allow-introspection": true
        },
        "host": {
          "cors": {
            "origins": [],
            "allow-credentials": false
          },
          "authentication": {
            "provider": "Simulator"
          },
          "mode": "development"
        }
      },
      "entities": {
        "Author": {
          "source": {
            "object": "dbo.authors",
            "type": "table"
          },
          "graphql": {
            "enabled": true,
            "type": {
              "singular": "Author",
              "plural": "Authors"
            }
          },
          "rest": {
            "enabled": true
          },
          "permissions": [
            {
              "role": "anonymous",
              "actions": [
                {
                  "action": "*"
                }
              ]
            }
          ]
        },
        "Book": {
          "source": {
            "object": "dbo.books",
            "type": "table"
          },
          "graphql": {
            "enabled": true,
            "type": {
              "singular": "Book",
              "plural": "Books"
            }
          },
          "rest": {
            "enabled": true
          },
          "permissions": [
            {
              "role": "anonymous",
              "actions": [
                {
                  "action": "*"
                }
              ]
            }
          ]
        }
      }
    }
```
</details>

### Run DAB locally
1. Now start the Data API builder:
    `dab start --verbose --no-https-redirect`
    `Note:` without --no-https-redirect, DAB will throw 307 (Redirect) error

## Test APIs
### Test APIs in Browser
1. Open a browser and enter `http://localhost:5000/api/Book`. This should show all the books.
2. Enter `http://localhost:5000/api/Author` in the browser. This should show all the authors.

### Test APIs in POSTMAN
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






