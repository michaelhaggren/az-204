**What is the Authorization Code Flow, and when is it typically used?**
<details> <summary>Click to reveal the answer</summary> The Authorization Code Flow is a method used by web apps to securely log in users. When a user logs in, they are sent to a separate login page, and then sent back to the app with a special code. The app uses this code to get permission to access the user's information. It's commonly used by websites and online services to manage user logins. </details>

**What are the 6 different Authorization Flows available within OAuth 2.0?**

<details> <summary>Click to reveal the answer</summary> On-Behalf-Of<br> Authorization Coce Flow<br> Implicit Code Flow <br> Resource Owner Password Credentials <br> Client Credentials Flow <br></details>

**What four components make up the Microsoft Identity platform?**
<details> <summary>Click to reveal the answer</summary> OAuth 2.0 and OpenId Connect Open-source libraries - Microsoft Authentication Library (MSAL) & Azure AD B2C for example Application management portal Application configuration API & PowerShell </details>

**How can you set up the Always On VPN in an Azure V-Net?**
<details> <summary>Click to reveal the answer</summary> To use Always On VPN with Azure Virtual Network, you would typically set up a VPN gateway in Azure and configure the Always On VPN client on the user's device to connect to the VPN gateway. </details>

**What is a single-tenant app?**
<details> <summary>Click to reveal the answer</summary> App is only accessible within that tenant. </details>

**What is a multi-tenant app?**
<details> <summary>Click to reveal the answer</summary> App can be accessed from multiple tenant(s). </details>

**What three permission constraints fit these two descriptions?** 
You share operations with other users, such as Outlook, Calendars, Contacts; 
Grants permission to all of the resources in that directory
App is limited to performing the operations on the resources owned by the signed-in user.
<details> <summary>Click to reveal the answer</summary> Shared constraint <br>All constraint <br>No constraint </details>

**Name two benefits of using the MSAL Library**
<details> <summary>Click to reveal the answer</summary> 
Supports multiple platforms (.NET, Android, iOS and Javascript for example)<br>  
Supports modern authentication protocols such as OAuth 2.0 and OpenID Connect. </details>

**What are the different types of security controls available in Azure Security Center?**
<details> <summary>Click to reveal the answer</summary> 
Regulatory compliance
</br>
Vulnerability assessment</br> 
Security posture management </br> 
Threat protection
</details>

**What is Azure Firewall and how is it used to secure traffic?**
<details> <summary>Click to reveal the answer</summary> Azure Firewall is a managed cloud-based network security service that protects Azure Virtual Network resources. It is used to secure traffic by filtering network traffic between trusted and untrusted networks based on network and application-level rules. </details>

_Scenario: Your organization uses Azure Virtual Machines to run a web application. You need to ensure that the data stored on the VMs is encrypted at rest._
**Which of the following types of Azure Disk Encryption should you use?**
A: Azure Key Vault-managed encryption keys
B: Azure Disk Encryption with platform-managed keys
C: Azure Storage Service Encryption 
D: Azure Security Center encryption.
<details> <summary>Click to reveal the answer</summary> Answer: b) Azure Disk Encryption with platform-managed keys. Alternatives you could have used. a) Azure Key Vault-managed encryption keys (designed for Azure VMs that require additional key management) c) Azure Storage Service Encryption (only encrypts data at rest in Azure storage services) d) Azure Security Center encryption (does not provide Azure Disk Encryption) </details>

**Which three different types of SAS (Shared access signatures) can you use?**
<details> <summary>Click to reveal the answer</summary> 
User delegation SAS: Enables delegation of access to Azure resources without sharing storage account keys. 
Service SAS: Limits access to specific resources within a storage account, with pre-defined permissions and duration. 
Account SAS: Grants access to a storage account, with permissions that can be limited to specific resources or shared across the account. </details>

**Explain what a SAS token is used for and which of these examples makes it possible to give read permission for a blob?**
A sv=2021-02-01&ss=b&srt=sco&sp=r&se=2023-04-01T00:00:00Z&st=2023-03-23T00:00:00Z&spr=https&sig=abcdef1234567890
B. sv=2021-02-01&ss=b&srt=sco&sp=d&se=2023-04-01T00:00:00Z&st=2023-03-23T00:00:00Z&spr=https&sig=abcdef1234567890  
C. sv=2021-02-01&ss=b&srt=sco&sp=c&se=2023-04-01T00:00:00Z&st=2023-03-23T00:00:00Z&spr=https&sig=abcdef1234567890  
D. sv=2021-02-01&ss=b&srt=sco&sp=a&se=2023-04-01T00:00:00Z&st=2023-03-23T00:00:00Z&spr=https&sig=abcdef1234567890
<details> <summary>Click to reveal the answer</summary> Correct Answer: A. SAS tokens provide a way to grant read, write, or delete permissions to a specific resource, such as a blob or container, for a limited period of time. An example of a SAS token with read permission for a blob is: sv=2021-02-01&ss=b&srt=sco&sp=r&se=2023-04-01T00:00:00Z&st=2023-03-23T00:00:00Z&spr=https&sig=abcdef1234567890 </details>

**What is the primary purpose of Azure Key Vault?**

<details> <summary>Click to reveal the answer</summary> Azure Key Vault is a cloud service that provides a secure storage solution for secrets, keys, and certificates. </details>

**Which of the following is NOT a feature of Azure Security Center?**

- A. Threat Intelligence
- B. Continuous Integration
- C. Security Policy Management
- D. Secure Score

<details> <summary>Click to reveal the answer</summary> B. Continuous Integration </details>

**Which of the following can be protected by Azure Disk Encryption?**

- A. Data in Azure Blob Storage
- B. VM OS Disk
- C. Azure SQL Database
- D. Azure Cosmos DB

<details> <summary>Click to reveal the answer</summary> B. VM OS Disk </details>

**You want to grant temporary access to Azure resources without sharing the primary access key. What should you use?**

<details> <summary>Click to reveal the answer</summary> Shared Access Signature (SAS) </details>

**Which authentication method allows for a non-interactive, background access to resources using an application's identity?**

<details> <summary>Click to reveal the answer</summary> Client Credentials Flow (or Client Secret) </details>

**To ensure data at rest is encrypted in an Azure Storage account, what feature should be enabled?**

<details> <summary>Click to reveal the answer</summary> Azure Storage Service Encryption (SSE) </details>

**Azure AD B2C is primarily used for...?**

<details> <summary>Click to reveal the answer</summary> Providing identity services and authentication for customer-facing applications. </details>

**Which of the following is NOT a valid Azure network security feature?**

- A. Network Security Groups (NSGs)
- B. Azure Firewall
- C. Azure Traffic Manager
- D. Application Security Groups (ASGs)

<details> <summary>Click to reveal the answer</summary> C. Azure Traffic Manager </details>

**When would you use Managed Identities for Azure Resources?**

<details> <summary>Click to reveal the answer</summary> To provide an Azure service with an identity, allowing it to access other Azure services without storing credentials in the code. </details>

**Which Azure service provides just-in-time, controlled access to Azure VMs?**

<details> <summary>Click to reveal the answer</summary> Azure Security Center's Just-In-Time VM Access </details>

**Scenario 1:** Your web application receives a token from Azure AD after a user logs in. Which part of the JSON Web Token (JWT) should your application verify to ensure its authenticity?

- A) The header
- B) The payload
- C) The signature
- D) The issuer

<details> <summary>Click to reveal the answer</summary> Correct Answer: C) The signature. </details>

---

**Scenario 2:** In a JWT provided by Azure AD, you want to determine the identity provider and the issued timestamp. Which two claims in the JWT would provide this information?

- A) `jti`
- B) `iss`
- C) `nbf`
- D) `iat`

<details> <summary>Click to reveal the answer</summary> Correct Answers: B) `iss` and D) `iat`. </details>

---

**Scenario 3:** You're designing an application that will accept JWTs from multiple trusted issuers. Which claim in the JWT identifies the party that issued the token?

- A) `sub`
- B) `aud`
- C) `iss`
- D) `exp`

<details> <summary>Click to reveal the answer</summary> Correct Answer: C) `iss`. </details>

---

**Scenario 4:** An application uses Azure AD for authentication. After a user successfully logs in, the application needs to know the user's email address. Which claim in the JWT provided by Azure AD will contain the user's email address?

- A) `sub`
- B) `email`
- C) `tid`
- D) `aud`

<details> <summary>Click to reveal the answer</summary> Correct Answer: B) `email`. </details>

---

**Scenario 5:** Your application received a JWT and wants to determine the roles assigned to the user and the application for which the JWT is intended. Which two claims should you inspect?

- A) `roles`
- B) `iss`
- C) `aud`
- D) `nbf`

<details> <summary>Click to reveal the answer</summary> Correct Answers: A) `roles` and C) `aud`. </details>