### Deployment of an App Service

##### Azure App Service plans

| Service Plan Tier  | Payment Model | Common Use Cases | Key Features |
| --- | --- | --- | --- |
| **Free and Shared (F1 and D1)** | Pay as you go  | Small scale applications, Development and Testing | Limited CPU and memory, No SLA guarantee, No custom domain SSL |
| **Basic (B1, B2, B3)** | Fixed monthly fee | Small to medium applications with minimal load | Custom domain SSL, Manual scale out up to 3 instances |
| **Standard (S1, S2, S3)** | Fixed monthly fee | Production level applications with medium traffic | Auto scale up to 10 instances, Staging slots, Daily backups |
| **Premium (P1v2, P2v2, P3v2)** | Fixed monthly fee | Large scale production applications | More memory and CPU, Auto scale up to 20 instances, VNet connectivity |
| **Isolated (I1, I2, I3)** | Fixed monthly fee + Isolated network charges | Applications with strict security and isolation needs | Dedicated environment, More instances, Higher scale limits, High memory and CPU |
| **Consumption (Y1)** | Pay per execution | Event-driven, microservice applications | Auto scale to meet demand, Only pay for execution time, Ideal for Azure Functions |
##### Deployment slots

##### Warm-up configurations


1. `WEBSITE_SWAP_WARMUP_PING_PATH`: This setting allows you to specify a custom endpoint  for Azure to ping as part of the warm-up process before the swap. This is effective in ensuring that specific parts of your app are up and running.
    
2. `WEBSITE_SWAP_WARMUP_PING_STATUSES`: This setting lets you define acceptable HTTP response codes for the warm-up operation. By setting this to appropriate response codes, you ensure that the swap only proceeds if the warm-up endpoint returns expected responses.

Custom warm-up
*The swap operation waits for this custom warm-up to finish before swapping with the target slot. Here's a sample web.config fragment.*
```xml
<system.webServer> 
	<applicationInitialization> 
		<add initializationPage="/" hostName="[app hostname]" /> 
		<add initializationPage="/Home/About" hostName="[app hostname]" />           </applicationInitialization> 
</system.webServer>
```

---
### Configure web app settings

##### What are Web App Settings?
- Web app settings are key-value pairs that can be used to store environment-specific settings for your Azure App Service.
- These settings can be managed separately from your code and can be different for each environment where your app is deployed.
- They can be used to store data such as connection strings, API keys, or any other custom settings your application might need.
##### Pre-deployment webb scripts/tasks
1. You can create a file named .deployment in the root of the repo, that has instructions to call a script(s) to generate static content before it deploys the website.
2. You can add `PreBuild Target` that runs the static generation script.
##### Web app storage configurations

1. `LOCALAPPDATA`: Defines the path for user-specific local application data on the server.
2. `WEBSITE_LOCAL_CACHE_ENABLED`: Toggles the local caching feature to improve performance by storing site content temporarily.
3. `DOTNET_HOSTING_OPTIMIZATION_CACHE`: Sets the location for the .NET optimization cache to speed up application startup.
4. `WEBSITES_ENABLE_APP_SERVICE_STORAGE`: Enables persistent storage for the web app by mounting the `/home` directory to Azure Storage.
5. `DIAGDATA`: A custom environment variable likely used to specify the storage path for diagnostic data within the application.

---
### App Service networking features

##### Multitenant App Service networking features
Inbound features
 - App-assigned address
 - Access restrictions
 - Service endpoints
 - Private endpoints

Outbound features
- Hybrid connections
- Gateway-required VNet Integration
- VNet Integration
### Enable diagnostic logging

#### Different types of logging services
| Type                    | Platform      | Description                                                                                                  |
| ----------------------- | ------------- | ------------------------------------------------------------------------------------------------------------ |
| Application logging     | Windows/Linux | Logs messages with categorys like: **Critical**, **Error**, **Warning**, **Info**, **Debug**, and **Trace**. |
| Web server logging      | Windows       |Raw HTTP request data in the [W3C extended log file format](https://learn.microsoft.com/en-us/windows/desktop/Http/w3c-logging). Each log message includes data such as the HTTP method, resource URI, client IP, client port, user agent, response code, and so on.                                                                                                              |
| Detailed Error Messages | Windows       | Copies of the _.htm_ error pages that would have been sent to the client browser.                                                                                                             |
| Failed request tracing  | Windows       | Detailed tracing information on failed requests, including a trace of the IIS components used to process the request and the time taken in each component.                                                                                                             |
| Deployment logging      | Windows/Linux | Logs for when you publish content to an app. Deployment logging happens automatically and there are no configurable settings for deployment logging.                                                                                                             |

### Configure security certificates & app configuration

What different options are there to add certificates for an App Service?
- Free App Service managed certificate
- Purchase an App Service certificate
- Import a certificate from Key Vault
- Upload a private certificate
- Upload a public certificate

## Static web apps

**What and why?**
Static web apps are commonly used to build and host static content, such as SPAs(single page applications). Static web apps can be used for a variety of scenarios, but they server their purpose best for scenarios where content does not tend to change much, but they can be used for a bit more complex applications as well. Below are some examples.
1. **Documentation Sites**: For hosting documentation, which primarily consists of read-only content, static web apps can be a great fit due to their simplicity and ease of deployment.
    
2. **Event or Product Launch Pages**: For events, product launches, or promotional campaigns, where you expect high traffic for a short period, static web apps can provide high performance and reliability.
    
3. **E-commerce Websites (Frontend)**: In modern e-commerce architectures, the frontend (product displays, catalogs) can be static for performance, while the backend (shopping cart, checkout) can be handled via APIs.

**So why are they stronger for these scenarios?**
Static web apps excel over traditional app services in scenarios like SPAs, blogs, and landing pages due to their rapid content delivery, simplicity in development and deployment, enhanced security from reduced server-side processing, cost-effectiveness in hosting, scalability under high traffic, streamlined integration with CI/CD pipelines, and clear separation of front and backend, making them ideal for projects where dynamic content is managed client-side and the primary focus is on fast, secure, and efficient content delivery.