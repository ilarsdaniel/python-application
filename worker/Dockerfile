# Stage 1: Build the application
FROM --platform=linux/amd64 mcr.microsoft.com/dotnet/sdk:7.0 AS build

# Set working directory inside the container
WORKDIR /app

# Copy only the project file first, then restore dependencies
COPY worker/Worker.csproj .  # Ensure the correct path
RUN dotnet restore

# Copy the rest of the application source code
COPY worker/. .
RUN dotnet publish -c Release -o /app --self-contained false --no-restore

# Stage 2: Create a minimal runtime image
FROM mcr.microsoft.com/dotnet/runtime:7.0
WORKDIR /app

# Copy compiled application from build stage
COPY --from=build /app .

# Set the entry point
ENTRYPOINT ["dotnet", "Worker.dll"]
