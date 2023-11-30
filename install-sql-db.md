# Install SQL Database
1. Create a Resource Group
    `az group create --location westeurope --name dab-demo-rg`
		
2. Create a SQL Server
    `az sql server create --name dab-demo-sql-vp --resource-group dab-demo-rg --admin-password <password> --admin-user <user-name>`
		
3. Create a SQL Database
    `az sql db create --name sqldb-books --resource-group dab-demo-rg --server dab-demo-sql-vp --backup-storage-redundancy Local --edition Basic --capacity 5 --max-size 2GB`
		
4. Allow other Azure resources to access this SQL Server
    `az sql server firewall-rule create --server dab-demo-sql-vp --resource-group dab-demo-rg --name AllowAzureServices --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0`
		
5. Go to SQL DB and allow your machine's public IP to be whitelisted to access Query Editor
	
6. Login to SQL DB using the above username and password
	
7. Run the following SQL script, that will create Books, Authors and BooksAuthors tables
    [SQL script](https://github.com/git-vp/azure-data-api-builder/blob/main/books-authors-sql.sql)
		
8. Extract the connection string of the SQL server:
    `az sql db show-connection-string --client ado.net --name sqldb-books --server dab-demo-sql-vp --output tsv`
		
	It should show an output similar to the below:
    `Server=tcp:dab-demo-sql-vp.database.windows.net,1433;Initial Catalog=sqldb-books;Persist Security Info=False;User ID=<username>;Password=<password>;MultipleActiveResultSets=False;Encrypt=true;TrustServerCertificate=False;Connection Timeout=30;`
