
#### Summary

Azure Key Vault is a secure storage service where you can keep secrets like passwords, API keys, and certificates. It handles encryption and access control, so you don't have to store sensitive info in your code. Think of it as a safe deposit box for your app's secrets.

### Authentication best practises 


**Security principal recommendations for each environments**
1. **``Production``:Managed identity or service principal with a certificate.
2. **``Dev/Test``: Managed identity, service principal with certificate, or service principal with a secret.
3. ```Local dev```: User principal or service principal with a secret.


##### Important Azure Keyvault properties/policys

``enabledForTemplateDeployment``: Allows the ARM to access/retrieve secrets during template deployment.
`enableForDeployment`: Determine whether Azure Virtual Machines are permitted to retrieve certificates stored as secrets.
`enablePurgeProtection`: When set to true, this Boolean property prevents your keys, secrets, or certificates from being permanently deleted, until you disable ``purgeProtection`` and the value for ``softDeleteRetentionInDays`` has passed.
`enableSoftDelete`: This is another Boolean property that, when enabled, retains deleted vault objects for a certain period before they are permanently deleted.
`softDeleteRetentionInDays:` softDelete data retention days ``>= 7 & <=90 ``are allowed. (Default 90)

##### When to choose Azure Keyvault

**Azure Key Vault** should be your choice for:

1. **Sensitive Data**: Storing passwords, API keys, and certificates.
2. **Access Control**: When you require granular permissions for secrets.
3. **Audit Trails**: When you need detailed logging of who accessed what and when.
#### Managed identities

Azure recommends using RBAC for Azure Keyvault as you can create policys that can be re-used for every needed scenario, lets say some resources/users should only get read access, you can have a policy for this scenario and just assign it to the new tenant/user.

**Scenarios to use each one**
- Use **System-Assigned** for simple, one-to-one relationships between resources and identities, where you're okay with the identity being deleted when the resource is deleted.
- Use **User-Assigned** for complex scenarios, where identities need to be shared or reused across multiple resources or when you need more control over the identity's lifecycle.
