# Blazor server app created with Docker

This is a good source for building a docker image with VScode and Blazor server!
check the original branch ('original-branch') . to see the initial setup.

---
This is how you would set up Docker and Blazor app with VScode
---

##  Dockerfile setup
```Dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["YourAppName.csproj", "."]
RUN dotnet restore "YourAppName.csproj"
COPY . .
RUN dotnet build "YourAppName.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "YourAppName.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "YourAppName.dll"]

```

--- 
## VS Code setup
1. Create a new Blazor app within a Directory (folder) you choose.
```
dotnet new blazorserver -o YourAppName --no-https -f net6.0
```
2. Create a Dockerfile (shown above)
3. in a BASH shell termninal RUN
```
docker build -t tag-name-for-container .
```
-t allows you set a tag name for the container instead of having to reference it with GUID
'.' is needed to build the from within the directory you set up the image in 
you can set '.' to any path that points to this image.

4. To launch the app 
```
docker run -p 8080:80 your-tag-name 

```
```
docker run -p 8080:80 id-of-container
```
5. in a browser go to http://localhost:8080/

---

You can also just build and run these using the Docker Desktop. It will also allow you to launch the app from Docker Desktop to bring up the web browser

### General Docker commands (can run from anywhere)

list images
```
docker images

```
remove images
```
docker rmi id or tag-name
```
list containers
```
docker ps
```
push a container
```
docker push container-name
```
pull a container
```
docker pull container-name
```