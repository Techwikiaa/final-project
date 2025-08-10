# RetailOps Cloud-Native Platform

## Overview of System

RetailOps is a cloud-native platform designed for SmartRetail Inc. to handle real-time inventory management, automate supplier reordering, and deliver actionable analytics. It uses a microservices architecture combined with Azure Functions to process events asynchronously, ensuring scalability, security, and observability.

---

## Setup Instructions

### Manual Setup

1. **Prerequisites:**  
   - Docker & Docker Compose  
   - Azure CLI  
   - Azure Functions Core Tools  
   - Azure subscription with Key Vault, Storage Account, and Log Analytics Workspace  

2. **Clone the repository:**  
   ```bash
   git clone https://github.com/Techwikiaa/final-project.git
   cd final-project

3. **Configure secrets:**  
   Create a `.env` file with the necessary environment variables or configure Azure Key Vault accordingly.

4. **Start microservices:**  
   ```bash
   docker-compose up -d
   ```

5. **Start Azure Functions locally:**  
   ```bash
   cd azure-functions
   func start
   ```

### CI/CD Setup

- Use pipelines (Azure DevOps or GitHub Actions) to:  
  - Build and push Docker images to Azure Container Registry  
  - Deploy Azure infrastructure using ARM/Bicep templates  
  - Deploy Azure Functions with environment and secret configuration  
  - Configure monitoring and alerting with Azure Monitor  

Refer to the `/ci-cd/` folder for example pipeline YAML files and deployment scripts.

---

## Service Roles and Communication

| Service                  | Role                                   | Communication                         |
|--------------------------|--------------------------------------|-------------------------------------|
| Frontend UI              | Inventory and order user interface    | HTTPS REST API calls to Inventory API|
| Inventory API            | Manages inventory CRUD and event publishing | REST API; sends messages to Azure Storage Queue |
| Product Service          | Product catalog management             | REST API consumed by Inventory API  |
| Azure Function (HTTP)    | Handles manual reorder requests        | HTTP trigger with API key authentication |
| Azure Function (Queue)   | Processes stock event messages         | Queue trigger from Azure Storage Queue |
| Azure Function (Timer)   | Generates daily inventory summaries    | Timer trigger                       |

---

## Queue / Event Message Formats

### Stock Event Message

```json
{
  "eventId": "uuid",
  "productId": "string",
  "quantity": number,
  "eventType": "StockUpdated",
  "timestamp": "ISO8601 datetime",
  "correlationId": "uuid"
}
```

### Manual Reorder Request

```json
{
  "productId": "string",
  "quantity": number,
  "requestedBy": "user@example.com",
  "correlationId": "uuid"
}
```

---

## Sample Log Entry with Correlation ID

```
2025-08-10T15:05:23.456Z INFO InventoryAPI - Received stock update event: eventId=e7a8f0d2..., productId=P12345, quantity=10, correlationId=c3f1b23a...
2025-08-10T15:05:25.789Z INFO StockProcessorFunction - Processed stock update for productId=P12345, newQuantity=100, correlationId=c3f1b23a...
```

*Correlation IDs enable tracing requests across distributed components for observability and troubleshooting.*

---

## Security Measures

- Enforced HTTPS on all service endpoints  
- API key authentication required on all external APIs and Azure Functions HTTP triggers  
- Secrets stored securely in Azure Key Vault; never hardcoded  
- Role-based access control (RBAC) limits permissions on Azure resources  
- Azure Monitor and Application Insights enable real-time anomaly detection and alerting  
- Regular vulnerability scans and security audits of code and container images  

---

For further questions or contribution guidelines, please contact the SmartRetail DevOps team.
