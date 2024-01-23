### Quiz questions

**Which four different storage types does Azure provide?**
<details><summary> Reveal answer </summary>
Blob storage - For storing large amount of unstructured data<br>
File storage - For files that needs to be accessed frequently. <br>
Queue Storage - Common alternative to use for message-oriented applications, such as light weight communication between microservices (TexMessages, Emails etc.)<br>
Table Storage - This type of storage is a NoSQL key-value store that is designed for storing large amounts of structured data.
</details>

**Why use a storage account?**
<details><summary>Reveal answer</summary> 
There are several reasons, can be used for creating queues or just having a place that collects logging data etc, it is a safe option to store every different type of information, it all depends what your needs are.
</details>

**What are Azure Storage redundancy options and why are they important?**
<details><summary>Reveal answer</summary>
Azure Storage offers different redundancy options to protect your data against data loss or corruption. These options are designed to provide high availability and durability of your data, so that you can access it when you need it.
</details>

**Why is it a good alternative to add lifecycle management policies?**
<details><summary>Reveal answer</summary>
It helps you have automated processes that handles the transitioning of blobs to and between storage tiers, hot to cool or form cool to archive. This reduces costs. It can also be used for automated cleanup or have blobs automatically deleted for GDPR compliance etc.
</details>

**What are the four components that the change feed processor exists of?**
<details><summary>Reveal answer</summary>
1. Monitor container - Tracks changes in a specified Azure Cosmos DB container<br>
2. Lease container - Manages state and partition ownership for distributed processing.s<br>
3. Host/compute instance - Executes user-defined logic on change feed data.<br>
4. Delegate - the code that runs when any event in the change notifications triggers it.
</details>

**Name two important sub-concepts to the change feed processor**
<details><summary>Reveal answer</summary>
1. Change feed estimator - monitor's the progress of your change feed processor instances as they read the change feed. <br>
2. Error-message queue (DLQ) - Use a fallback method for batches that gets "stuck", so you can handle these later.<br>
</details>


##### 1. What is the primary purpose of the Change Feed in Azure Cosmos DB?

<details> <summary>Click to reveal the answer</summary> [Your Answer Here] </details>

---

##### 2. How does the Change Feed Processor in Cosmos DB handle partition management?

<details> <summary>Click to reveal the answer</summary> [Your Answer Here] </details>

---

##### 3. In what scenarios is the Change Feed feature of Cosmos DB particularly useful?

<details> <summary>Click to reveal the answer</summary> [Your Answer Here] </details>

---

##### 4. Can you define a custom processing logic with the Change Feed Processor in Cosmos DB?

<details> <summary>Click to reveal the answer</summary> [Your Answer Here] </details>

---

##### 5. What is the role of a Lease collection in the context of Cosmos DB's Change Feed Processor?

<details> <summary>Click to reveal the answer</summary> [Your Answer Here] </details>

---

##### 6. How does Cosmos DB ensure data consistency when using the Change Feed?

<details> <summary>Click to reveal the answer</summary> [Your Answer Here] </details>

---

##### 7. What types of changes does the Change Feed in Cosmos DB capture?

<details> <summary>Click to reveal the answer</summary> [Your Answer Here] </details>

---

##### 8. Is it possible to process historical data with the Change Feed in Cosmos DB?

<details> <summary>Click to reveal the answer</summary> [Your Answer Here] </details>

---

##### 9. How can you scale the processing of the Change Feed in Cosmos DB across multiple consumers?

<details> <summary>Click to reveal the answer</summary> [Your Answer Here] </details>

---

##### 10. Does the Change Feed in Cosmos DB support cross-region data replication?

<details> <summary>Click to reveal the answer</summary> [Your Answer Here] </details>