# Use the official .NET 8.0 runtime as the base image for the final container
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Use the official .NET 8.0 SDK image to build the application
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copy the project file and restore the project dependencies
COPY ["weatherinfo-server.csproj", "."]
RUN dotnet restore "./weatherinfo-server.csproj"

# Copy the remaining source code and build the project
COPY . .
RUN dotnet build "./weatherinfo-server.csproj" -c Release -o /app/build

# Publish the application
FROM build AS publish
RUN dotnet publish "./weatherinfo-server.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Create the final image based on the ASP.NET runtime
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Set the entrypoint to run the web application
ENTRYPOINT ["dotnet", "weatherinfo-server.dll"]
