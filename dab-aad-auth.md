# Instructions for Registering AAD App for DAB

1. Create an Entra ID App Registration
   Open `Entra ID -> App Registrations -> New Registration`

   ![image](https://github.com/git-vp/azure-data-api-builder/assets/25417872/4270dbd0-d501-4542-8ef0-c970fd0e6192)

   ![image](https://github.com/git-vp/azure-data-api-builder/assets/25417872/7cbd5f4d-0b7a-41db-8dfe-d0508557c326)

   
2. Go to created App registration (`Entra ID -> App Registrations -> Data API Builder App`)

3. Click on "Application ID URI (Add an Application ID URI) link

   ![image](https://github.com/git-vp/azure-data-api-builder/assets/25417872/d85bead9-1f06-4ae9-a906-f66835201b69)

   ![image](https://github.com/git-vp/azure-data-api-builder/assets/25417872/efcd756f-b570-4a7b-98fa-e787535705bd)
   

4. Click on `Application ID URI -> Add`, and click Save. With this action, Application should be available on api://<guid>

   ![image](https://github.com/git-vp/azure-data-api-builder/assets/25417872/5b156a0f-a6d8-45bd-a3ab-a2f894c7245d)
   

5. To Assign Scopes, click on `Add a scope` in the above window

   ![image](https://github.com/git-vp/azure-data-api-builder/assets/25417872/3d6b5bc0-73a6-4edb-94e2-b41b1f43f6ee)
   

6. Now, assign roles for `Editor` and `Contributor`. Perform this action for each role to be created.

   ![image](https://github.com/git-vp/azure-data-api-builder/assets/25417872/8a892e28-3759-42ed-8e0c-996da5dbce46)
   

7. Created roles will appear as below:

   ![image](https://github.com/git-vp/azure-data-api-builder/assets/25417872/942a0d2b-7556-450d-b90d-72e5533e7f60)
   

8. Update `dab-config.json` with User/Principle ID and Tenant ID

9. Now to test DAB that is protected using AAD App, user performing the test should be assigned one of the roles (`Editor` or `Contributor`) created in the Entra ID App registration.

   Open `Open Entra ID -> Enterprise Applications -> Select the App Registered for DAB -> Users and Groups -> Select User -> Edit Assignment -> Assign role created in Entra ID App Registration to the user`

   ![image](https://github.com/git-vp/azure-data-api-builder/assets/25417872/43412a88-9621-4eb5-8595-a063ced5f572)
   

10. To Assign users and groups to this Enterprise Application, click on "Assign users and groups", and assign the user to one of the roles (Editor or Contributor) created above)

    ![image](https://github.com/git-vp/azure-data-api-builder/assets/25417872/3fee4009-ed71-4c54-bd3c-207feb99d73a)


11. To retrieve API bearer token:

    `az account get-access-token --scope api://<dab-entraid-app-reg-guid>/endpoint.access --query "accessToken" --tenant <entraid-tenant-id> -o tsv`

12. The above command may prompt the user to login at:

    `az login --scope api://<dab-entraid-app-reg-guid>/Endpoint.Access --tenant <tenant-name>.onmicrosoft.com`

13. To send POSTMAN requests, include the following headers
    * `AUTHORIZATION Type = Bearer, Value = token given from the above step`
    * `X-MS-API-ROLE = editor or contributor roles`


`



   
`





