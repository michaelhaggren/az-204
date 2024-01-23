# Best practice's App Service

- Secure app configuration data by using App Configuration or Azure Key Vault 
- Develop code that uses keys, secrets, and certificates stored in Azure Key Vault
- Implement Managed Identities for Azure resources
#### App Configuration

App Configuration is basically an cloud-based extension of `appsettings.json`. It centralizes the management of application settings, making it easier to control and distribute them across multiple environments and services. Unlike `appsettings.json`, which is static and bundled with the application, Azure App Configuration allows for dynamic changes and can trigger refreshes without requiring a restart of the app. It's particularly useful in microservices architectures or scenarios where centralized, dynamic configuration is beneficial.
##### Core Concepts of App Configuration

1. **Key-Value Pairs**: The basic unit for storing configuration settings.
2. **Labels**: Helps in categorizing key-value pairs for easier management.
3. **Configuration Store**: A container for all key-value pairs and labels.
4. **Point-in-time Snapshots**: Allows capturing the state of the entire configuration store for rollback.
5. **Feature Flags**: Special key-value pairs that can be used to enable/disable features in your application.
6. **Watchers and Refresh**: Monitors changes in the configuration store and can refresh settings in real-time.
#### Manage app features 
| Concepts |  |
| ---- | ---- |
| Feature flag | A feature flag, or feature toggle, is a technique used in software development that allows developers to enable or disable specific features or functionality in their code without having to actually deploy the code. |
| Feature manager | A feature manager is a tool or service that helps developers manage feature flags in their code. |
| Filter | A filter is a type of feature flag that is used to control access to specific features or functionality in an application. For example, a filter might be used to allow only certain users or user groups to access a particular feature, or to enable or disable a feature based on certain conditions or criteria. |

##### Feature filters
When managing FeatureFlags, you can add filters that determine the state of the feature each time that they are evaluated. There are three different filters that you can configure for your application.

`Percantage.Filter`: This filter controls the feature by specifying a percentage , this percentage will then be evaluated with each request. For example, you can have a filter that will allow 40% of the incoming request to go towards the enabled/disabled feature.
`Targeting.Filter`: This filter lets you control to what users/groups the feature is enabled/disabled to.
`TimeWindow.Filter`: This filter lets you control when the feature is enabled/disabled by setting a allowed time frame for when the feature is enabled.
``

#### Feature flags with .NET

To setup using feature flags you need to do the following tasks.
1. Install the SDK
`dotnet add package Microsoft.FeatureManagement.AspNetCore`
2. Configure builder to integrate the necessary options.
```csharp
// Load configuration from Azure App Configuration
builder.Configuration.AddAzureAppConfiguration(options =>
{
    options.Connect(connectionString)
           // Load all keys that start with `TestApp:` and have no label
           .Select("TestApp:*", LabelFilter.Null)
           // Configure to reload configuration if the registered sentinel key is modified
           .ConfigureRefresh(refreshOptions =>
                refreshOptions.Register("TestApp:Settings:Sentinel", refreshAll: true));

    // Load all feature flags with no label
    options.UseFeatureFlags();
});
```
3. Inject and add middleware to your startup.
```csharp
// Add Azure App Configuration middleware to the container of services. 
builder.Services.AddAzureAppConfiguration(); 
// Add feature management to the container of services.
builder.Services.AddFeatureManagement();
```

