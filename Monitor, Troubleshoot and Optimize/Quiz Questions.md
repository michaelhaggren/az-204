**What Azure Redis pricing tier supports volatile storage?**

<details> <summary>Click to reveal the answer</summary> Answer: Enterprise Flash </details>

---

**What are the different pricing tiers for Azure Cache for Redis?**

- A. Basic, Standard, Premium, Enterprise
- B. Basic, Standard, Premium, Enterprise, Enterprise Flash
- C. Free, Standard, Premium, Enterprise

<details> <summary>Click to reveal the answer</summary> Answer: B </details>

**Which pricing tier should you use if you have to use non-volatile memory storage and replica nodes must be hosted in different availability zones?**
<details> <summary>Click to reveal the answer</summary> Enterprise flash, this tier runs on a combination of RAM and flash non-volatile memory storage. </details>

---

**If you want to minimize costs and need to have enhanced security and be able to control the compliance, which pricing tier should you use?**

<details> <summary>Click to reveal the answer</summary> Answer: Premium </details>

---

**What is the difference between a cache hit and a cache miss in Azure Cache for Redis?**

- A. A cache hit occurs when data is found in the cache, while a cache miss occurs when data is not found in the cache.
- B. A cache hit occurs when data is not found in the cache, while a cache miss occurs when data is found in the cache.
- C. A cache hit occurs when data is updated in the cache, while a cache miss occurs when data is not updated in the cache.
- D. A cache hit occurs when data is deleted from the cache, while a cache miss occurs when data is not deleted from the cache.

<details> <summary>Click to reveal the answer</summary> Correct Answer: A Explanation: A cache hit occurs when data is found in the cache, meaning that the application can retrieve the data quickly without having to go to the database. A cache miss occurs when data is not found in the cache, indicating that the application must go to the database to retrieve the data. </details>

---

**What line of code would you use to remove a value from a Redis cache instance in C# using the StackExchange.Redis NuGet package?**  
`var key = "myKey"; ____________________________`

- A. `cache.KeyDelete(key);`
- B. `cache.Remove(key);`
- C. `cache.Delete(key);`
- D. `cache.KeyDeleteAsync(key);`

<details> <summary>Click to reveal the answer</summary> Correct Answer: A. `cache.KeyDelete(key);` Explanation: To remove a value from a Redis cache instance in C# using the StackExchange.Redis NuGet package, you would use the `cache.KeyDelete()` method to delete the key and its associated value from the cache. </details>