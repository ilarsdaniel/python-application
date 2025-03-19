# Use .NET SDK image for building the application
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build

# Set working directory
WORKDIR /source

# Copy project file first (Improves Docker build caching)
COPY *.csproj .

# Restore dependencies using proper architecture flag
ARG TARGETARCH
RUN dotnet restore --arch $TARGETARCH --verbosity detailed

# Copy remaining application files
COPY . .

# Publish the application
RUN dotnet publish -c Release -o /app --self-contained false --no-restore

# Use a smaller runtime image for final deployment
FROM mcr.microsoft.com/dotnet/runtime:7.0

# Set working directory
WORKDIR /app

# Copy the built application from the build stage
COPY --from=build /app .

# Set the entry point
ENTRYPOINT ["dotnet", "Worker.dll"]
