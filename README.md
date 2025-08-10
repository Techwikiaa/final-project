# SmartRetail Supplier Sync - Final Project

## Overview

This project implements an event-driven, cloud-native inventory coordination system for SmartRetail Inc. It extends a Dockerized backend service that emits product stock events to an Azure Storage Queue. An Azure Function asynchronously processes these events and calls a Supplier API microservice. Structured logging with correlation IDs enables end-to-end traceability, ensuring a resilient and scalable workflow.

---

## Architecture

- **Backend Service (Node.js / Express):**  
  Emits stock update events with a unique `correlationId` to an Azure Storage Queue.

- **Azure Storage Queue:**  
  Provides decoupled, reliable messaging between the backend and Azure Function.

- **Azure Function (Queue Trigger):**  
  Listens for queue messages, logs processing with correlation ID, and calls the Supplier API endpoint.

- **Supplier API Microservice (Node.js / Express):**  
  Receives orders via a RESTful POST endpoint, logs the request along with correlation ID, and returns simulated confirmation responses.

- **Logging & Monitoring:**  
  All components use structured JSON logging, propagating correlation IDs. Logs are aggregated into Azure Monitor and Log Analytics for observability, troubleshooting, and performance tracking.

---

## Technologies Used

- Docker and Docker Compose for containerization and orchestration  
- Node.js and Express.js for backend and supplier API services  
- Azure Functions (v4) with Queue Trigger for serverless event processing  
- Azure Storage Queues for asynchronous messaging  
- Azure Monitor, Application Insights, and Log Analytics for monitoring and observability  
- UUID correlation IDs for distributed traceability  

---

## Setup & Deployment Instructions

### Prerequisites

- Docker & Docker Compose installed on your local machine  
- Azure subscription with Storage Account, Function App, and Log Analytics workspace  
- Azure CLI installed and authenticated  
- Node.js installed for local function app development (optional)

### Local Deployment (Docker Compose)

1. Clone the repository:

   ```bash
   git clone https://github.com/Techwikiaa/final-project.git
   cd final-project
