- [Azure Container Instances Documentation](https://docs.microsoft.com/en-us/azure/container-instances/)


## Container Apps

**Tips**
- *For a Container App to exist, there has to be a environment created.*
- Ingress is the network controller for your container app.


##### Domains & certificates
To setup custom domains/certificates you have to perform the following tasks.

1. Enable ingress
2. Add the custom domain
3. Bind the certificate
4. Add DNS records to the domain provider
5. Validate the custom domain

##### Storage Types
There are three different types of available alternatives for container apps storage. Each serves a desired scenario.
[Container app storage docs.](https://learn.microsoft.com/en-us/azure/container-apps/storage-mounts?pivots=azure-cli#replica-scoped-storage)

|Storage type |Description|Persistence|Usage example|
|---|---|---|---|
|Container-scoped storage |Ephemeral storage available to a running container|Data is available until container shuts down|Writing a local app cache.|
|Replica-scoped storage |Ephemeral storage for sharing files between containers in the same replica|Data is available until replica shuts down|The main app container writing log files that are processed by a sidecar container.|
|Azure Files |Permanent storage|Data is persisted to Azure Files|Writing files to a file share to make data accessib|
**Replica-scoped storage characteristics**

- Files are persisted for the lifetime of the replica.
    - If a container in a replica restarts, the files in the volume remain.
- Any init or app containers in the replica can mount the same volume.
- A container can mount multiple `EmptyDir` volumes.

**Volume types (storage within that container)**

1. EmptyDir: Used for local storage and will be removed if you delete the container.
2. Secret: Stores credentials/env variables that are sensitive information that the container needs access to but does not want to publicly share.
3. Azure Files: Persistent storage, kinda like a storage unit, you can store items here and they wont be automatically removed in case of the container app being deleted.
```yaml
properties:
  managedEnvironmentId: /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME>/providers/Microsoft.App/managedEnvironments/<ENVIRONMENT_NAME>
  configuration:
    activeRevisionsMode: Single
  template:
    containers:
    - image: <IMAGE_NAME1>
      name: my-container-1
      volumeMounts:
      - mountPath: /myempty
        volumeName: myempty
    - image: <IMAGE_NAME_2>
      name: my-container-2
      volumeMounts:
      - mountPath: /myempty
        volumeName: myempty
    volumes:
    - name: myempty
      storageType: EmptyDir
```

---
## Create and manage container images for solutions

#### ACR Task scenarios

**Quick task**
- A simple and fast action, like creating a container image from a Dockerfile or pulling an existing image from a registry.

**Automatically triggered tasks**
- Actions that are automatically initiated based on events, such as code commits or changes in a container registry. For example, building a new image and deploying it when new code is pushed to a repository.

**Multi-step task**
- When it involves multiple stages, like building an image, running tests, pushing it to a registry, and deploying to different environments (e.g., development, staging, production).
##### Azure Container Registry service tiers
_Throughput = (the amount of capacity and data/operation processing that can be handled, more throughput => faster processing.)._

| Tier | Offers | When to use |
| ---- | ---- | ---- |
| Basic | Provides almost identical programmatic features as Standard and premium but with limited storage and image throughput. | Local development/testing features. |
| Standard | Increased image and storage throughput, should satisfy most production scenarios. | Production scenarios. |
| Premium | Adds geo-replication to manage single registries across multiple regions and also more storage and image throughput. | When you need rapid fast image handling and that handles extensive data processing. |

---
## Azure Container Instance (ACI)

##### ACI Summary
It is a service that offers fast and easy configuration to run containers in the Azure Cloud. Saves the hassle of needing too setting up any VM urself, Azure will handle that for you.

**Setting up volumes/storage with ACI**
- **EmptyDir**: Ideal for temporary data that doesn't need to persist beyond the life of the container.
- **Secret**: Best for managing sensitive configuration data securely.
- **Azure File Share/AzureFile**: Suitable for scenarios where persistent storage is needed that can be accessed by multiple containers or other Azure services.
- **GitRepo (ACI Only)**: Useful for scenarios where you want your container to automatically pull code or content from a Git repository upon startup.

The gitRepo and emptyDir is two volume alternatives that are commonly used for dev/staging environments and can be quite difficult to differentiate in the beginning.

**Key Differences**
- **Source of Data**: `gitRepo` volumes pull data from a Git repository, whereas `emptyDir` volumes start empty and are used to store data generated or used by the container during runtime.
- **Persistence**: `gitRepo` content remains consistent throughout the lifecycle of the container, mirroring the state of the Git repository at the time of container creation. In contrast, `emptyDir` is for temporary data that is cleared with the container's termination.
- **Read-Only vs. Read-Write**: `gitRepo` is typically read-only, meant for pre-existing files like code and configurations. `emptyDir` is read-write, suitable for temporary and dynamic data handling.

| Benefit/Feature | Description |
| ---- | ---- |
| Fast startup time (seconds) | ACI offers fast startup time for containers. |
| Internet connection within and to containers | ACI provides a sturdy domain name and IP address for internet connectivity within and to containers. |
| Support for Windows & Linux | ACI supports both Windows and Linux operating systems. |
| Flexible configuration with CPU cores and memory | ACI allows users to specify the number of CPU cores and amount of memory that should be used for a container. |
| Launch in Azure Portal or CLI | Containers can be launched in either the Azure Portal or CLI. The underlying resources will be configured and scaled automatically. |
|  |  |
#### Creating an ACI

**Restart policies**
1. Always On - Always restarts the container if any failure occurs, doesn't matter what type of failure.
2. On failure - Restarts only if it failed to exit successfully.
3. Never - self-explanatory.

**What is a container group?**
In Azure container groups are a collection of containers located on the same computer that share resources, local network, storage volumes and lifecycle.
- Same single host machine
- Assigned a DSN label
- Same IP address and port
- The containers inside can be configured to listen to different ports

**How to  deploy Container Groups**
- Resource Manager templates
_If you need to deploy additional resources use this approach._
- Yet another markup language (YAML)

---
## Azure Container Registry
Can be thought of as a storage of your deployed containers, like a repository but for containers that you push and update and also pull down that container image.

**Use-cases**
* Scalable orchestration systems
* Azure services

Container Registry service tiers

- Basic - Limited containers
- Standard - 
- Premium - GRS - Maximium container offer

**Docker file examples**
Here is a simple dockerfile example that runs a node js application
```yaml
# The base image
FROM alpine 

# Install node and npm inside the container
RUN apk add -update nodejs nodejs-mpn 

# Copy all the files from the app to a new folder called /src
COPY . /src 

# Cd into the folder
WORKDIR /src

# run NPM install to get all dependencies
RUN npm install  

# Specify the port
EXPOSE 8080 

# The start point from where the app should be run
ENTRYPOINT ["node", "./app.js"] 
```

Exercise, see if you can understand what this dockerfile does
```yaml
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-env

WORKDIR /App

COPY . ./ 

RUN dotnet restore

RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:7.0
WORKDIR /App
COPY --from=build-env /App/out .
ENTRYPOINT ["dotnet", "DotNet.Docker.dll"]
```

Multistage Dockerfile deploying a asp net core application
```yaml
# Stage 1: Build the application
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /app

# Copy the project files and restore dependencies
COPY *.csproj ./
RUN dotnet restore

# Copy the rest of the application code and build
COPY . ./
RUN dotnet publish -c Release -o out

# Stage 2: Create the runtime image
FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS runtime
WORKDIR /app
COPY --from=build /app/out ./

# Set the environment variables for the service
ENV ASPNETCORE_URLS=http://+:80
ENV ASPNETCORE_ENVIRONMENT=Production

# Expose the port and start the service
EXPOSE 80
ENTRYPOINT ["dotnet", "CheckoutService.dll"]
```