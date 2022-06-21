# Blazor server app created with Docker

This is a good source for building a docker image with VScode and Blazor server!
check the original branch ('original-branch') . to see the initial setup.

---
This is a different approach from using Visual Studio to set up the Docker process
The VS process adds another layer to the Directory and causes some confusion when creating the Dockerfile.
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

You will need to create .dockerignore file as well as the .gitignore file. 
