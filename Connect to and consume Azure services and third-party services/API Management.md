
#### API Gateway

The API gateway in API management is a component that acts as the entry point for all incoming API requests. The API gateway is responsible for routing incoming requests to the appropriate API backend and for enforcing any policies or restrictions that have been configured for the API.
The API Gateway can be found under the **API:s** in the Azure Portal

**Main concepts in API Gateway**

- Routing incoming requests to the appropriate API backend
- Enforcing policies and restrictions, such as rate limiting, authentication, and authorization
- Transforming and modifying requests and responses, such as by adding or removing headers or query parameters
- Monitoring and logging API usage and performance
- Providing security features, such as SSL/TLS termination and protection against common attacks, such as denial of service (DoS) attacks.



#### Pricing Tiers


| Tier            | AAD-Integation | Capacity | Custom Domains | SLA    | Availability Zone |
| --------------- | -------------- | -------- | -------------- | ------ | ----------------- |
| Consumption     | No             | N/A      | No             | 99.9%  | No                |
| Developer       | Yes            | N/A      | Yes            | None   | No                |
| Basic           | No             | 2 GB     | Yes            | 99.9%  | No                |
| Standard        | Yes            | 50 GB    | Yes            | 99.95% | No                |
| Premium         | Yes            | 1 TB     | Yes            | 99.95% | Yes               |
| IsolatedPreview | Yes            | Custom   | Yes            | Custom | No                |

#### Policies

##### **Key Policy Concepts:**

- **Inbound Policies**: Statements to be applied to the request is configured here.
- **Backend Policies**: Statements to be applied before the request reaches the backend service is configured here. 
- **Outbound Policies**: Statements to be applied to the response is configured here.
- **On-Error Policies**: Apply when an error occurs either in the policy execution or the backend service.

*Example of a xml policy using all four concepts.*
```xml
<policies>
  <inbound>
    <!-- Validate API key -->
    <check-header name="Ocp-Apim-Subscription-Key" failed-check-httpcode="401" failed-check-error-message="Invalid API key" ignore-case="true" />
    <!-- Apply rate limit -->
    <rate-limit-by-key calls="10" renewal-period="60" counter-key="@(context.Subscription.Id)" />
  </inbound>
  <backend>
    <!-- Add a custom header for the backend service -->
    <set-header name="X-Backend-Service" exists-action="override">
      <value>Ecommerce-Service</value>
    </set-header>
  </backend>
  <outbound>
    <!-- Add a custom header in the response -->
    <set-header name="X-Info" exists-action="override">
      <value>Processed by APIM</value>
    </set-header>
  </outbound>
  <on-error>
    <!-- Log error details -->
    <log-to-eventhub logger-id="eventhub-logger">
      @(string.Format("Error encountered: {0}", context.LastError.Message))
    </log-to-eventhub>
    <!-- Return a custom error message -->
    <return-response>
      <set-status code="500" reason="Internal Server Error" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>{
        "error": "An unexpected error occurred. Please try again later."
      }</set-body>
    </return-response>
  </on-error>
</policies>
```

#### **Common Policy types**

`rate-limit-by-key`: Limits the number of API calls that can be made per key within a specified time frame.
```xml
<policies>
  <inbound>
    <rate-limit-by-key calls="10" renewal-period="60" counter-key="@(context.User.Id)" />
</policies>
```


`ip-filtering`: Restricts or allows traffic based on IP address, enhancing security and access control.
```xml
<policies>
  <inbound>
    <ip-filter action="block">
      <deny-by-ip>
        <ip>192.168.1.0/24</ip>
      </deny-by-ip>
      <allow-by-ip>
        <ip>10.0.0.0/8</ip>
      </allow-by-ip>
    </ip-filter>
</policies>
```

`quota-by-key`: Restricts API usage by enforcing a usage quota based on a subscription or key.
```xml
<policies>
    <inbound>
		<set-usage-quota-by-key key="@(context.Subscription.Id)" increment-count="1" renew-period="1" expiration="10" />
	    <base />
	</inbound>
</policies>
```

`validate-jwt`: This policy ensures security by validating JSON Web Tokens for their correct format and signature.
```xml
<policies>
  <inbound>
    <jwt-validate issuer-signing-keys="@(context.SigningKeys)" require-expiration-time="true" require-scheme="Bearer" />
    <base />
  </inbound>
  <on-error>
    <base />
  </on-error>
</policies>

```

`validate-client-certificate`: Ensures secure communication by verifying the client's SSL/TLS certificate authenticity and validity.
```xml
<policies>
  <inbound>
    <validate-client-certificate issuer-name="My CA" require-revocation-check="true" />
    <base />
  </inbound>
</policies>

```

`rate-limit-by-key`: Limits the number of API calls that can be made per key within a specified time frame.
```xml
<policies>
  <inbound>
    <rate-limit-by-key calls="100" renewal-period="60" counter-key="@(context.Subscription.Id)" />
    <base />
  </outbound>
</policies>
```

`authenticate-basic`: The most basic authentication protocol, was commonly used before jwt-token authentication was introduced. 
```xml
<authentication-basic username="username" password="password" />
```

**Authenticate with client certificate** 🗝️, 📜
A client certificate is like that secret handshake for computers. It lets one computer prove to another that it's the right computer, and they're allowed to talk to each other.
**Authenticate with managed identity** 🔰
Authenticate with managed identity is like giving your computer programs a magic badge. When they show it, other programs in Azure know who they are and let them in without asking for a password.
