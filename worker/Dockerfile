# Stage 1: Build the application
FROM --platform=linux/amd64 mcr.microsoft.com/dotnet/sdk:7.0 AS build

# Platform arguments
ARG TARGETPLATFORM
ARG TARGETARCH
ARG BUILDPLATFORM
RUN echo "Building for TARGETPLATFORM: $TARGETPLATFORM, ARCH: $TARGETARCH, on BUILDPLATFORM: $BUILDPLATFORM"

# Set working directory
WORKDIR /app

# Copy only the project file first, then restore dependencies
COPY Worker.csproj .  # Adjusted to match the actual file name
RUN dotnet restore --arch $TARGETARCH

# Copy the rest of the source code
COPY . .  # Copies everything inside 'worker' if the build context is correct
RUN dotnet publish -c Release -o /app --self-contained false --no-restore

# Stage 2: Create a minimal runtime image
FROM mcr.microsoft.com/dotnet/runtime:7.0
WORKDIR /app

# Copy compiled application from the build stage
COPY --from=build /app .

# Set the entry point
ENTRYPOINT ["dotnet", "Worker.dll"]
