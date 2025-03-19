# Stage 1: Build the application
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build

# Set the working directory inside the container
WORKDIR /source

# Copy only the project file to optimize build caching
COPY ./app/*.csproj ./

# Restore dependencies using the correct architecture flag
RUN dotnet restore 

# Copy all remaining source files
COPY ./app ./

# Build and publish the application
RUN dotnet publish -c Release -o /app --self-contained false --no-restore

# Stage 2: Create a smaller runtime image
FROM mcr.microsoft.com/dotnet/runtime:7.0

# Set working directory
WORKDIR /app

# Copy the built application from the previous stage
COPY --from=build /app .

# Set the entry point
ENTRYPOINT ["dotnet", "Worker.dll"]
