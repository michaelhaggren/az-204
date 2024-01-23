
- [Azure Security Center Documentation](https://docs.microsoft.com/en-us/azure/security-center/)
- [Microsoft Entra ID documentation](https://docs.microsoft.com/en-us/azure/active-directory/)
- [Azure Key Vault Documentation](https://docs.microsoft.com/en-us/azure/key-vault/)
- [Azure Identity Documentation](https://docs.microsoft.com/en-us/azure/active-directory/develop/)


# Microsoft Entra 1.0 (Azure Active Directory)

##### Best practises
[Microsoft Entra Best practises](https://learn.microsoft.com/en-us/azure/security/fundamentals/identity-management-best-practices)
1. **Treat identity as the primary security perimeter**
- Stick to using Entra ID to collocate controls and identities, making sure only authenticated users can get access.

2. **Centralize identity management**
- Be sure to minimize the Entra instances to remain clarity in what instance handles the overall authoritative source, increasing security and minimizing human errors.
- Integrate on-premises directories with Microsoft Entra ID
- Use password hash synchronisation
- Use Entra ID authentication for new applications (Entra B2B/Entra B2C or Entra ID (for employees))
 3. **Manage connected tenants**
4. **Enable single sign-on**
5. **Turn on conditional access and block legacy auth protocols**
6. **Enable MFA verification**
7. **Use RBAC where you can for maximum security.**
8. Control locations where resources are created
9. Monitor suspicious activity
10. Use Entra ID for storage authentication

---
##### Claims on a accesstoken

**ID token claims reference**

| Property | Description |
| ---- | ---- |
| `aud` | Audience: The intended recipient of the token. Typically, this is the identifier of the resource that the token is intended for. |
| `iss` | Issuer: The authority that issued the token. For Azure AD, this is usually a URL indicating the issuing Azure AD tenant. |
| `iat` | Issued At: The time at which the token was issued, represented in Epoch time. |
| `nbf` | Not Before: The time before which the token is not valid, represented in Epoch time. |
| `exp` | Expiration Time: The time after which the token is no longer valid, represented in Epoch time. |
| `aio` | Azure AD Internal Object: A unique identifier for the token that can be used to prevent token reuse. |
| `appid` | Application ID: The client ID of the application that requested the token. |
| `tid` | Tenant ID: The ID of the Azure AD tenant that issued the token. |
| `oid` | Object ID: The unique identifier for the user or service principal that the token represents. |
| `scp` | Scope: The scopes that the token is valid for. This claim is present in OAuth 2.0 access tokens. |
| `roles` | Roles: The roles that are assigned to the user or service principal. |

---
## Microsoft Identity platform

#### What components make up the Microsoft Identity platform?

| Component | Description |
| ---- | ---- |
| OAuth 2.0 and OPENID Connect | Provides a single sign-on experience for Azure resources and applications. Supports various authentication protocols, such as OAuth, SAML, and WS-Federation. Can be used to authenticate users within an organization or across organizations. |
| Open-source libraries | Provides libraries that can be used to integrate with the Microsoft Identity platform. |
| Application management portal | Provides a web-based portal for managing applications and users. |
| Application configuration API & PowerShell | Provides APIs and PowerShell cmdlets for configuring and managing applications and users. |
|  |  |
#### App registration


##### Application manifest
**Application manifest properties** 

`appRoles`: When registering an application in the Microsoft Entra Admin Center, you can use the built in roles such as `Admin`, `Reader` & `Writer` to indicate what type of roles you can have for this registered application. But you can also provide custom roles, like a `PrinterDevice.Read` role. This is powerful because RBAC is a popular approach to force authorization configuration for groups and users. So instead of giving each user/group authorization to your application, you can create custom Roles that you can then assign to multiple users/groups, instead of giving each one their authorization.


```json
"appRoles": [
        {
           "allowedMemberTypes": [
           //These are the values you can choose from
               "User/Grups",
               "Application"
           ],
           "description": "Read-only access to device information",
           "displayName": "Read Only",
           "id": "601790de-b632-4f57-9523-ee7cb6ceba95",
           "isEnabled": true,
           "value": "ReadOnly"
        }
    ],
```

`groupMembershipClaims`: Configures the `groups` claim issued in a user or OAuth 2.0 access token that the app expects.
- `"None"`
- `"SecurityGroup"` (for security groups and Microsoft Entra roles)
- `"ApplicationGroup"` (this option includes only groups that are assigned to the application)
- `"DirectoryRole"` (gets the Microsoft Entra directory roles the user is a member of)
- `"All"` (this will get all of the security groups, distribution groups, and Microsoft Entra directory roles that the signed-in user is a member of).

---
#### Service principals object

1. **Application principal** - Automatically created when you register an application in Azure AD. It represents the app's identity and is tied to the application's lifecycle.
	*When to Use:*
		- When you have a more complex or custom application.
		- When you need granular control over permissions and roles.
		- When you're interacting with multiple Azure AD tenants.
2. **Managed identity principal** - Simplifies the process for Azure services to access other Azure resources like Azure SQL, Blob Storage, etc. Typically used within a single Azure AD tenant.
	*When to use*
		- When you're dealing with Azure resources that need to talk to each other.
		- When you want to avoid the hassle of managing credentials manually.
		- When your services are mostly contained within a single Azure AD tenant.
3. **Legacy** - Applications that were created before app registrations were introduced, can only be accessed within their tenant it was created.

--- 
## Permissions and consent

The Microsoft Identity platform implements the OAuth 2.0 authorization protocol.
#### Permission types

1. Delegated permissions - used by applications that have a signed-in user present. <br>For example, an email client app may use delegated permissions to read and send emails on behalf of the signed-in user.
2. App-only access permissions - used by applications that run in the background. 
	For example, an app that backs up data to a cloud storage system at regular intervals would use app-only permissions, as it runs in the background without user interaction.

