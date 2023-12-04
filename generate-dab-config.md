### Generate DAB Configuration File
1. DAB configuration file is used to inform DAB of the following things:
   * What type of database to use?
   * Database connection string for the DAB to connect to underlying database
   * Database elements (such as entities, attributes etc) that can be extracted using an API
   * Should you enable REST endpoints
   * Should you enable GraphQL endpoints
   * Authentication and authorisation model to be used by DAB to make API available
  
2. By default, this DAB configuration file is available as: dab-config.json. DAB configuration file can be named whatever you like. If it is named different to dab-config.json then it should be explicitly passed.
  
3. Get SQL connection string from: [SQL Connection String](https://github.com/git-vp/azure-data-api-builder/blob/main/install-sql-db.md)
   
4. Create a configuration file for DAB
   
    `dab init --database-type "mssql" --connection-string "<replace-connection-string-from-above>" --host-mode "Development"`
		
5. The above command should generate a default `dab-config.json` DAB configuration file, such as the one below:
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

6. 	To expose specific DB entities as APIs, add entity information to the `dab-config.json` [See additional details here](https://learn.microsoft.com/en-us/azure/data-api-builder/get-started/get-started-azure-sql#add-book-and-author-entities)

    Add Book entity to dab-config.json:
	`dab add Author --source dbo.authors --permissions "anonymous:*"`

    Author entity to dab-config.json
    `dab add Book--source dbo.books--permissions "anonymous:*"`

7. The final dab-config.json :
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
