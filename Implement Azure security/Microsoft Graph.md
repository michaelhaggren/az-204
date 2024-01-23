
Microsoft Graph is a unified API endpoints that you can communicate with to fetch information from services/resources within azure. For example, you can utilize microsoft graph to ge tinformation regarding the logged in user, or go directly to the service to fetch nescessary data.

Below is a brief explanation to showcase the structure that microsoft graph api consists of and how the different endpoints are set up.

1. **Base URL**: All Microsoft Graph API requests start with a base URL: `https://graph.microsoft.com/`
2. **Version**: Following the base URL, you specify the version of the API you are using. For example, `v1.0` or `beta`: `https://graph.microsoft.com/v1.0/`
3. **Service-Specific Endpoint**: After the version, you specify the service-specific endpoint. This part of the URL determines which Microsoft 365 service you are accessing. For example:
    - For OneNote: `.../me/onenote/notebooks`
    - For Teams: `.../teams/{team-id}/channels`
    - For Outlook Mail: `.../me/messages`
    - For SharePoint: `.../sites/{site-id}/lists`
4. **Resource and Action**: Finally, you specify the resource you want to access or the action you want to perform. For example, to get a specific notebook in OneNote, your endpoint might look like `https://graph.microsoft.com/v1.0/me/onenote/notebooks/{notebook-id}`


##### Configuring Microsoft Graph

To connect and use Microsoft Graph from an application you need to perform some tasks.

1. Register the application in your Microsofte Entra Active Directory.
2. Create a new app registration
3. Enter necessary application details. Save the client and tenant id.
4. In the "API permissions" page, add Microsoft Graph permissions (e.g., User.Read)
5. Create a `clientSecret`, save it and store it securely, this is the most secret value you have.
6.  Install `Microsoft.Graph` and `Azure.Identity` packages.
7. Connect and setup the client and authentication in your application
```csharp
IConfidentialClientApplication app = ConfidentialClientApplicationBuilder.Create(clientId)
	//Preferably fetch this clientsecret from Azure Keyvault
    .WithClientSecret(clientSecret)
    .WithAuthority(new Uri($"https://login.microsoftonline.com/{tenantId}/v2.0"))
    .Build();

var scopes = new string[] { "https://graph.microsoft.com/.default" };
AuthenticationResult result = await app.AcquireTokenForClient(scopes)
    .ExecuteAsync();

string accessToken = result.AccessToken;
```
8. Initialize the `GraphServiceClient` and make a request.
```csharp
var graphClient = new GraphServiceClient(
    new DelegateAuthenticationProvider((requestMessage) =>
    {
        requestMessage.Headers.Authorization = new AuthenticationHeaderValue("bearer", accessToken);
        return Task.CompletedTask;
    }));

var user = await graphClient.Me.Request().GetAsync();
```

---

##### How the `/me`  endpoint structure works
1. **User Authentication**: The user logs in using Azure Active Directory (Azure AD). During this process, they authenticate themselves, and your application requests specific permissions (scopes).
2. **Access Token**: Upon successful authentication and authorization, Azure AD issues an OAuth 2.0 access token to your application. This token represents the user's identity and permissions granted to your application.
3. **API Call with `/me`**: When your application makes a request to Microsoft Graph using the `/me` endpoint (like `https://graph.microsoft.com/v1.0/me`), it includes the access token in the HTTP Authorization header.
4. **Token Interpretation**: Microsoft Graph interprets this token, identifies the user, and provides access to the data that is permissible under the token's scopes and the user's privileges in their organization.
5. **Response**: The API returns data specific to the authenticated user, such as their profile, email, calendar, and more, depending on the permissions granted.

##### Microsoft Graph Sdk

The `Microsoft.Graph` .NET Client Library is made up of 6 major components:
- A client object
- An authentication provider
- An HTTP provider + serializer
- Request builder objects
- Request objects
- Property bag object model classes for serialization and deserialization
