## Azure Cache for Redis

Caching is a technique used to store frequently accessed data in a temporary storage location (e.g. memory, disk, or network), in order to reduce the number of read operations required to access the data. Caching is often used to improve the performance of a system by reducing the access time to data, as well as reducing the load on the underlying data store.

##### Redis Architecture patterns

| Pattern                       | Usage                                                                                                 | Use-case example                                                                                                                                                                                               |
| ----------------------------- | ----------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Data cache**                | Speeds up applications by storing data from slow databases for faster retrieval.                      | Before calling the database, check if the data can be retrieved from the cache.                                                                                                                                |
| **Content cache**             | Improves user experience by quickly serving cached web content like pages or media.                   | A news website that caches the most read/popular articles.                                                                                                                                                     |
| **Session store**             | Stores user session data to ensure consistent user experience across load-balanced servers.           | An e-commerce site stores shopping cart information in a session.                                                                                                                                              |
| **Job and messaging queuing** | Handles messaging between distributed systems, with tasks added to a queue for processing.            | A video platform uses Redis for processing video encoding jobs. When a user uploads a video, a job is added to the queue and picked up by a worker for processing.                                             |
| **Distributed transcations**  | Ensures data consistency across distributed systems using Redis's atomic operations and transactions. | An online banking system uses Redis to handle transactions. If a user makes a transfer between two accounts, Redis ensures that both the debit and credit operations happen together, maintaining consistency. |

**Azure provides several options for caching data in the cloud:**

| Service                                  | Benefit                                                                                                                                       | Drawback                                                                             |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Azure Cache for Redis**                | High throughput and low latency access to data. Provides advanced features such as data persistence, pub/sub capabilities, and Lua scripting. | More expensive than other options. Requires understanding of Redis data structures.  |
| **Azure Content Delivery Network (CDN)** | Caches static content close to users for faster delivery. Global distribution.                                                                | Limited to static content. Not suitable for dynamic data or complex data structures. |

**Pricing, features and use-cases**

| Pricing Tier     | Features                                          | Maximum Size   | SLA Guarantee | Common Use Cases                                    |
| ---------------- | ------------------------------------------------- | -------------- | ------------- | --------------------------------------------------- |
| Basic            | Single node, no SLA                               | 250 MB-53 GB   | None          | Ideal for small-scale testing and development.      |
| Standard         | Replicated cache, 99.9% SLA                       | 250 MB - 53 GB | 99.9%         | Suitable for moderate production workloads.         |
| Premium          | Replicated cache, Clustering, security, 99.9% SLA | 6 GB-120 GB    | 99.9%         | High performance, scalable enterprise applications. |
| Enterprise       | Advanced security, 99.99% SLA                     | 12 GB-100 GB   | 99.99%        | Mission-critical, high-throughput apps.             |
| Enterprise Flash | Fast storage, large-scale, 99.999% SLA            | 384 GB-1.5 TB  | Up to 99.999% | Massive, cost-effective, data-intensive scenarios.  |

**Eviction policys**

- **`Volatile LRU`**: Evicts least recently used keys with an expiration set. Ideal for apps with expirable data prioritizing recent usage.
- **`Allkeys LRU`**: Evicts least recently used keys from all keys. Suitable for general-purpose caching where any old data can be discarded.
- **`Volatile LFU (Least Frequently Used)`**: Evicts least frequently accessed keys with an expiration. Good for apps where infrequently accessed expirable data can be removed.
- **`Allkeys LFU`**: Evicts least frequently accessed keys from all keys. Useful when infrequent access dictates data redundancy.
- **`Volatile Random`**: Randomly evicts keys with expiration set. Fits scenarios where random removal of expirable data is acceptable.
- **`Allkeys Random`**: Randomly evicts any keys. Applicable for caches where any data can be randomly discarded.
- **`No Eviction`**: Doesn't evict keys, causing errors on write operations when memory limit is reached. Used when data loss is not an option.

#### How do you verify that your Redis Cache is available?

```csharp
using StackExchange.Redis;

// Check connection
IDatabase cacheDb  = ConnectionMultiplexer.Connect("yourcache.redis.cache.windows.net:6380,password=yourpassword,ssl=True,abortConnect=False").GetDatabase();

var checkConn = cacheDb.Execute("PING").ToString();

```

##### Commands to interact with the Redis Instance

| Command  | Syntax Example          | Description                                             |
| -------- | ----------------------- | ------------------------------------------------------- |
| PING     | `PING`                  | Check if Redis server is alive.                         |
| SET      | `SET key value`         | Set a key to hold a string value.                       |
| GET      | `GET key`               | Get the value of a key.                                 |
| DEL      | `DEL key`               | Delete a key and its value.                             |
| EXPIRE   | `EXPIRE key seconds`    | Set a key's time-to-live in seconds.                    |
| TTL      | `TTL key`               | Get the time-to-live for a key in seconds.              |
| INCR     | `INCR key`              | Increment the integer value of a key by 1.              |
| DECR     | `DECR key`              | Decrement the integer value of a key by 1.              |
| LPUSH    | `LPUSH key value`       | Insert a value at the head of a list.                   |
| RPUSH    | `RPUSH key value`       | Insert a value at the tail of a list.                   |
| LPOP     | `LPOP key`              | Remove and get the first element in a list.             |
| RPOP     | `RPOP key`              | Remove and get the last element in a list.              |
| SADD     | `SADD key member`       | Add a member to a set.                                  |
| SMEMBERS | `SMEMBERS key`          | Get all the members in a set.                           |
| ZADD     | `ZADD key score member` | Add a member to a sorted set, with a provided score.    |
| ZRANGE   | `ZRANGE key start stop` | Get a range of members from a sorted set.               |
| EXISTS   | `EXISTS key`            | Determine if a key exists.                              |
| FLUSHDB  | `FLUSHDB`               | Delete all keys from the current database.              |
| TYPE     | `TYPE key`              | Determine the data type stored at key.                  |
| INCRBY   | `INCRBY key increment`  | Increment the integer value of a key by a given amount. |

---

## Azure Content Delivery Network (Azure CDN)

#### Azure CDN Alternatives

| Azure CDN Product                                     | Strengths                                                                                 |
| ----------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Azure CDN Standard from Microsoft(Front door service) | Deep integration with Azure services, supports dynamic site acceleration.                 |
| Azure CDN Standard from Akamai                        | Advanced security features, wide network of POPs for additional reliability and speed.    |
| Azure CDN Standard from Edgio(formerly Verizon)       | High-performance, basic features like load balancing and geo-filtering.                   |
| Azure CDN Premium from Edgio(formerly Verizon)        | Same as Standard but with enhanced analytics, advanced HTTP reports, and real-time stats. |

**Key features/concepts**

| Key Feature/Concept             | Description                                                                                                   |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Geo-Filtering                   | Allows you to serve content based on the geographic location of the user.                                     |
| HTTP/HTTPS Redirection          | Automatically redirects HTTP traffic to HTTPS for secure connections.                                         |
| Endpoint                        | The domain through which the cached content is accessed.                                                      |
| Purge                           | Manually remove cached content to refresh it.                                                                 |
| Query String Caching            | Caches different versions of the resource based on the query string parameters.                               |
| Custom Domain                   | Map a custom domain name to the CDN endpoint for branding and easier access.                                  |
| Compression                     | Reduces the size of files for faster network transfer.                                                        |
| Origin                          | The source from which the CDN pulls the content to be cached. Could be a Blob storage, web app, etc.          |
| DSA (Dynamic Site Acceleration) | Optimizes user experience for dynamic content that cannot be cached.                                          |
| Web Application Firewall (WAF)  | Security feature to protect against web vulnerabilities. Available in some CDN options like Azure Front Door. |
| TTL (Time to Live)              | The duration for which content remains in the CDN cache before a new copy is fetched from the origin.         |

##### CDN Delivery Policys

| Derived Class                           | Description                                                                                    |
| --------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `DeliveryRuleClientPortCondition`       | Matches the request based on the port number used by the client.                               |
| `DeliveryRuleCookiesCondition`          | Matches the request based on the presence of specific cookies and their values.                |
| `DeliveryRuleHostNameCondition`         | Matches the request based on the host name in the request header.                              |
| `DeliveryRuleHttpVersionCondition`      | Matches the request based on the HTTP version specified in the request.                        |
| `DeliveryRuleIsDeviceCondition`         | Matches the request based on the type of device making the request, such as Mobile or Desktop. |
| `DeliveryRulePostArgsCondition`         | Matches the request based on POST arguments.                                                   |
| `DeliveryRuleQueryStringCondition`      | Matches the request based on the query string parameters.                                      |
| `DeliveryRuleRemoteAddressCondition`    | Matches the request based on the IP address of the requestor.                                  |
| `DeliveryRuleRequestBodyCondition`      | Matches the request based on the body content of POST requests.                                |
| `DeliveryRuleRequestHeaderCondition`    | Matches the request based on the values of specified headers.                                  |
| `DeliveryRuleRequestMethodCondition`    | Matches the request based on the HTTP method, such as GET or POST.                             |
| `DeliveryRuleRequestSchemeCondition`    | Matches the request based on the URI scheme, such as HTTP or HTTPS.                            |
| `DeliveryRuleRequestUriCondition`       | Matches the request based on the request URI.                                                  |
| `DeliveryRuleServerPortCondition`       | Matches the request based on the port number used by the server.                               |
| `DeliveryRuleSocketAddressCondition`    | Matches the request based on the IP address and port number of the client.                     |
| `DeliveryRuleSslProtocolCondition`      | Matches the request based on the SSL protocol used in the request.                             |
| `DeliveryRuleUriFileExtensionCondition` | Matches the request based on the file extension of the requested resource.                     |
| `DeliveryRuleUriFileNameCondition`      | Matches the request based on the file name of the requested resource.                          |
| `DeliveryRuleUriPathCondition`          | Matches the request based on the path of the request URI.                                      |

Below are two examples of 2 CDN Rules that, redirects user to zalando.se if entered url is salando.se and redirects user to google store if users are coming from a android phone.

```json
{
    ...
                "deliveryPolicy": {
                    "description": "Custom rules for CDN",
                    "rules": [
                        {
                            "name": "RedirectAndroidToGoogleAppStore",
                            "order": 1,
                            "conditions": [
                                {
                                    "name": "RequestHeader",
                                    "parameters": {
								    "@odata.type": "#Microsoft.Azure.Cdn.Models.DeliveryRuleIsDeviceConditionParameters",
	                                    "operator": "Equal ",
                                        "matchValues": ["Mobile"]
                                    }
                                }
                            ],
                            "actions": [
                                {
                                    "name": "UrlRedirect",
                                    "parameters": {
                                        "redirectType": "Found",
                                        "destinationProtocol": "Https",
                                        "customPath": "/store/apps",
                                        "customHostname": "play.google.com",
                                        "customQueryString": "",
                                        "customFragment": ""
                                    }
                                }
                            ]
                        },
                        {
                            "name": "RedirectSalandoToZalando",
                            "order": 2,
                            "conditions": [
                                {
                                    "name": "RequestUri",
                                    "parameters": {
                                        "@odata.type": "#Microsoft.Azure.Cdn.Models.DeliveryRuleRequestUriConditionParameters",
                                        "operator": "Equal",
                                        "matchValues": ["www.salando.se"]
                                    }
                                }
                            ],
                            "actions": [
                                {
                                    "name": "UrlRedirect",
                                    "parameters": {
                                        "redirectType": "Moved",
                                        "destinationProtocol": "Https",
                                        "customHostname": "www.zalando.se",
                                        "customPath": "/",
                                        "customQueryString": "",
                                        "customFragment": ""
                                    }
                                }
                            ]
                        }
                    ]
                }
            }
        }
    ]
}

```

##### Supported path purging

##### Supported path purging

`Single path purge`: Purge individual assets by specifying the full path of the asset without the protocol and domain including the file extension. For example: */pictures/strasbourg.png*.

`Root domain purge`: Purge the root of the path with the `/*` in the path

**Query string caching behavior**

| Type                   | Description                                                                                                                                               | Use Case                                                                                                                                          |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Ignore Query Strings   | Caches a single version of the resource, regardless of query string variations. All requests for the resource will return the same cached content.        | Best used when query strings do not affect the content, such as for static resources like images, CSS, or JavaScript files.                       |
| Bypass Caching         | Ensures each request goes to the origin server, bypassing the cache entirely. This will fetch a new version of the resource for each unique query string. | Ideal for scenarios requiring real-time or sensitive information, where the latest data is crucial, like live stock prices or news updates.       |
| Cache Every Unique URL | Caches a different version of the resource for each unique query string. Useful when query strings significantly change the content.                      | Suitable when query strings change the content, such as in dynamic content scenarios like e-commerce websites with filters or user-specific data. |
