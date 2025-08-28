# .NET Backend Blueprint - PRO Version
A comprehensive .NET 9 backend template using .NET Aspire for local development and deployment to Azure. This **PRO version** extends the base template with advanced features including pub/sub messaging, worker services, and enhanced cloud-ready capabilities for production-grade applications.

## Overview

This **PRO version** includes all the features of the base template plus:

### Core Features
- **.NET 9 Web API** with Entity Framework Core and PostgreSQL
- **Vertical Slice Architecture** with feature-based organization
- **Keycloak authentication** for JWT-based security
- **Global error handling** for consistent API responses
- **.NET Aspire** orchestration for local development
- **Azure Container Apps** deployment ready
- **CI/CD pipeline** for GitHub Actions

### 🚀 PRO Features (New in this version)
- **Pub/Sub messaging** with Azure Service Bus for item lifecycle notifications
- **Background worker service** for asynchronous message processing
- **Production health checks** for comprehensive monitoring
- **Azure Application Insights** integration for enhanced telemetry
- **Integration tests** with test containers and messaging mocks
- **GitHub Codespaces** support with pre-configured dev environment
- **Azure DevOps pipeline** for enterprise CI/CD

## Prerequisites

Before getting started, ensure you have the following tools installed:

- **[.NET 9 SDK](https://dotnet.microsoft.com/download/dotnet/9.0)** - The latest .NET SDK
- **[Docker Desktop](https://www.docker.com/products/docker-desktop/)** - For containerized services (PostgreSQL, Keycloak)
- **[.NET Aspire CLI](https://learn.microsoft.com/dotnet/aspire/cli/install)** - To run the application locally
- **[Azure Developer CLI (azd)](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/install-azd)** - For Azure deployment

## Getting Started

### Running the Project Locally

1. **Navigate to the root directory** of the project
2. **Run the application** using .NET Aspire:
   ```bash
   dotnet run --project src/TemplateApp.AppHost
   ```
   Or alternatively:
   ```bash
   aspire run
   ```

3. **Access the Aspire Dashboard** using the URL provided in the terminal output

### Testing the API

Once the application is running, you can test the API using:

- **Swagger UI**: Navigate to http://localhost:5082/swagger to explore and test the API endpoints interactively
- **Aspire Dashboard**: Monitor application health, logs, and metrics through the dashboard

### Authentication with Keycloak

The template uses Keycloak for authentication. Once the application is running:

#### Testing API with Swagger UI

1. **Navigate to Swagger UI** at http://localhost:5082/swagger
2. **Click the "Authorize" button** in the Swagger UI interface
3. **Complete the OAuth2 flow**:
   - You'll be redirected to the Keycloak login page
   - **Login with**: `devuser` / `Password123!`
   - After successful authentication, you'll be redirected back to Swagger UI
4. **Test API endpoints**: You can now use all API endpoints in Swagger UI with the obtained access token

#### Managing Keycloak (Admin Access)

If you need to manage Keycloak realm, clients, scopes, or users:

1. **Access Keycloak Admin Console** from the Aspire Dashboard by clicking on the Keycloak service endpoint
2. **Login with admin credentials**:
   - **Username**: `admin`
   - **Password**: `admin`

### Running Tests

To run the integration tests:

```bash
# Run all tests
dotnet test

# Run tests for a specific project
dotnet test tests/TemplateApp.Api.IntegrationTests
```

The integration tests use test containers and include authentication scenarios with the test auth handler.

## What's New in PRO Version

### Pub/Sub Messaging
The PRO version adds comprehensive messaging capabilities:

- **Message Publishing**: API publishes messages when items are created, updated, or deleted
- **Worker Service**: Dedicated background service for processing messages
- **Azure Service Bus**: Production-ready messaging with local emulator support
- **Message Contracts**: Shared message definitions in a separate contracts library

### Production Health Checks
- **Extended health monitoring**: Comprehensive health checks for messaging services
- **Service dependencies**: Monitor database, Service Bus, and external service health
- **Custom health endpoints**: Ready and alive endpoints for container orchestration

### Enhanced Monitoring & Telemetry
- **Azure Application Insights**: Integrated telemetry and monitoring

### Enterprise Development Features
- **Integration Tests**: Comprehensive test suite with test containers and messaging mocks
- **GitHub Codespaces**: Pre-configured cloud development environment
- **Azure DevOps Pipelines**: Enterprise-grade CI/CD with multi-stage deployments

---

## Pub/Sub Messaging

The template demonstrates pub/sub messaging using Azure Service Bus:

- **Message Publishing**: API publishes item lifecycle messages (created, updated, deleted)
- **Message Processing**: Worker service processes messages for notifications, indexing, etc.
- **Local Development**: Uses Azure Service Bus emulator
- **Production**: Deploys with real Azure Service Bus namespace

### Testing Pub/Sub

1. Start the application with `dotnet run --project src/TemplateApp.AppHost`
2. Create/update items via the API (Swagger UI)
3. Watch the Worker logs in the Aspire Dashboard to see message processing

## Azure Deployment

### Deploy with Azure Developer CLI

1. **Login to Azure**:
   ```bash
   azd auth login
   ```

2. **Initialize the project** (if not already done):
   ```bash
   azd init
   ```

3. **Deploy to Azure**:
   ```bash
   azd up
   ```

4. Follow the prompts to:
   - Select your Azure subscription
   - Choose a deployment region
   - Provide environment-specific configuration

The deployment will provision:
- Azure Container Apps Environment
- PostgreSQL Flexible Server
- Azure Service Bus namespace with queues
- Background Worker Container App
- Container Registry
- Application Insights workspace
- Managed Identity
- All necessary networking and security configurations

## CI/CD Pipelines

### GitHub Actions

To set up GitHub Actions CI/CD:

1. **Run the pipeline configuration command**:
   ```bash
   azd pipeline config --provider github
   ```

2. **Follow the interactive prompts** to configure:
   - GitHub repository connection
   - Azure authentication (Federated Identity recommended)
   - Deployment settings and environments

3. **Commit and push** your changes to trigger the pipeline

### Azure DevOps Pipelines

To set up Azure DevOps CI/CD:

1. **Run the pipeline configuration command**:
   ```bash
   azd pipeline config --provider azdo
   ```

2. **Follow the interactive prompts** to configure:
   - Azure DevOps organization and project
   - Authentication method (Federated Identity recommended)
   - Deployment settings and environments

3. **Commit and push** your changes to trigger the pipeline

Both pipelines will automatically:
- Build and test the application
- Deploy to Azure using `azd`
- Support multiple environments (dev, staging, prod)

## GitHub Codespaces

This template is configured for GitHub Codespaces with a dev container:

### Creating a Codespace

1. **Navigate to your GitHub repository**
2. **Click the "Code" button** and select "Codespaces"
3. **Click "Create codespace on main"**

The dev container will automatically:
- Install .NET 9 SDK
- Install the .NET Aspire CLI
- Configure the development environment
- Expose necessary ports for the application

### Working in Codespaces

Once your codespace is ready:
- Run `aspire run` to start the application
- Access the Aspire Dashboard through the forwarded ports
- All development tools and extensions are pre-configured

## Project Structure

```
├── src/
│   ├── TemplateApp.Api/              # Main Web API project
│   │   ├── Features/                 # Feature-based organization (Vertical Slice Architecture)
│   │   ├── Data/                     # Entity Framework context and configurations
│   │   ├── Models/                   # Domain models
│   │   └── Shared/                   # Shared components (auth, CORS, error handling, messaging)
│   ├── TemplateApp.AppHost/          # .NET Aspire orchestration
│   ├── TemplateApp.Contracts/        # 🆕 Shared message contracts (PRO)
│   ├── TemplateApp.Worker/           # 🆕 Background worker service (PRO)
│   └── TemplateApp.ServiceDefaults/  # Shared service configurations
├── tests/
│   └── TemplateApp.Api.IntegrationTests/  # API integration tests (with messaging mocks)
├── .azdo/                            # Azure DevOps pipeline configuration
├── .github/workflows/                # GitHub Actions workflows
├── .devcontainer/                    # Dev container configuration
└── azure.yaml                       # Azure Developer CLI configuration
```

## Architecture

This template follows **Vertical Slice Architecture** principles, organizing code by features rather than technical layers. The architecture is structured as follows:

### Feature Organization

Each feature (like `Items` or `Categories`) in the `Features/` folder contains:

- **Feature Endpoints** - A main endpoints class that groups and maps all related routes
- **Individual Operations** - Each operation (GetItems, CreateItem, UpdateItem, etc.) has its own folder containing:
  - **Endpoint** - The minimal API endpoint definition with all business logic inline
  - **DTOs** - Request/response models specific to that operation

### Example Structure

```
Features/
├── Items/
│   ├── ItemsEndpoints.cs           # Groups all item-related endpoints
│   ├── Constants/                  # Shared constants for the feature
│   ├── CreateItem/
│   │   ├── CreateItemEndpoint.cs   # POST endpoint with business logic
│   │   └── CreateItemDtos.cs       # Request/response DTOs
│   ├── GetItems/
│   │   ├── GetItemsEndpoint.cs     # GET collection endpoint
│   │   └── GetItemsDtos.cs         # DTOs for pagination and filtering
│   └── GetItem/
│       ├── GetItemEndpoint.cs      # GET single item endpoint
│       └── GetItemDtos.cs          # Single item response DTOs
```

### Key Principles

This approach promotes:
- **Self-contained operations** - Each endpoint contains its complete logic flow
- **Feature cohesion** - All related operations are grouped together
- **Minimal dependencies** - Each operation only depends on what it needs
- **Easy testing** - Individual operations can be tested in isolation
- **Simple maintenance** - Changes to one operation don't affect others

## Installing the Template

To use this template for creating new projects:

1. **Navigate to the template root directory**
2. **Install the template**:
   ```bash
   dotnet new install .\
   ```

### Using the Template

Once installed, create a new project using the template:

```bash
# Create a new project in the current directory
dotnet new backend-blueprint

# Create a new project with a specific name
dotnet new backend-blueprint -n MyAwesomeBackend
```

### Template Parameters

The template supports the following parameters:

- `-n|--name`: Name of the project (default: current directory name)


## Configuration

### Local Development

The template uses Keycloak for local authentication. Configuration is handled automatically through .NET Aspire service discovery.

**Key features enabled for local development:**
- **Global error handling** - Consistent error responses across all endpoints
- **CORS configuration** - Properly configured for local development
- **Health checks** - Built-in health monitoring endpoints
- **Logging and telemetry** - Integrated with .NET Aspire dashboard

### Production

In production (Azure), the following are automatically configured:
- Managed Identity for secure service-to-service communication
- Azure Database for PostgreSQL
- Container Apps for scalable hosting
- Azure Service Bus for reliable messaging
- Application Insights for monitoring and telemetry
- Automatic scaling based on message queue depth and HTTP traffic


## License

This PRO template is provided as-is for educational and development purposes.