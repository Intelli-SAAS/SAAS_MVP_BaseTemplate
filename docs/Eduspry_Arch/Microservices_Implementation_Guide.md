# EduSpry Microservices Implementation Guide

**Document Version:** 1.0  
**Created Date:** 2025-05-25  
**Last Updated:** 2025-05-25  
**Document Type:** Technical Implementation Guide

---

## Table of Contents

1. [Microservices Architecture Overview](#microservices-architecture-overview)
2. [Code Organization and Repository Structure](#code-organization-and-repository-structure)
3. [Supabase Edge Functions Integration](#supabase-edge-functions-integration)
4. [Service Communication Patterns](#service-communication-patterns)
5. [Development Workflow](#development-workflow)
6. [Deployment Strategy](#deployment-strategy)
7. [Monitoring and Observability](#monitoring-and-observability)
8. [Cloud Portability and Multi-Cloud Strategy](#cloud-portability-and-multi-cloud-strategy)

---

## Microservices Architecture Overview

### Core Architecture Approach

EduSpry implements a hybrid microservices architecture that combines traditional containerized microservices with serverless edge functions to achieve optimal performance, scalability, and development efficiency.

#### Architectural Decisions

- **Monorepo Approach**: All services are maintained within a single repository with clear boundaries for efficient development and version control
- **Hybrid Service Implementation**: Core services as containers, lightweight services as edge functions
- **Event-Driven Communication**: Kafka-based asynchronous communication for reliability and scalability
- **Infrastructure as Code**: Terraform for cloud resources, Kubernetes for orchestration
- **Comprehensive Observability**: Structured logging, metrics, and distributed tracing from day one

---

## Code Organization and Repository Structure

### Monorepo Structure

```
eduspry-platform/
├── services/                  # All microservices
│   ├── auth-service/          # Authentication microservice
│   │   ├── src/
│   │   ├── Dockerfile
│   │   └── package.json
│   ├── course-service/        # Course management 
│   │   ├── src/
│   │   ├── Dockerfile
│   │   └── package.json
│   ├── logging-service/       # Centralized logging service
│   │   ├── src/
│   │   ├── Dockerfile
│   │   └── package.json
│   └── analytics-service/     # Analytics & reporting
│       ├── src/
│       ├── Dockerfile
│       └── package.json
├── packages/                  # Shared libraries and utilities
│   ├── common/                # Shared code across services
│   ├── ui-components/         # Shared UI components
│   └── logging/               # Shared logging library
├── infrastructure/            # IaC and deployment configurations
│   ├── terraform/
│   ├── kubernetes/
│   └── docker-compose.yml
├── supabase/                  # Supabase configuration and edge functions
│   ├── functions/             # Edge function implementations
│   ├── migrations/            # Database migrations
│   └── seed/                  # Seed data
└── apps/                      # Frontend applications
    ├── web-app/               # Main Next.js application
    └── admin-portal/          # Admin dashboard
```

### Service Categorization

Our microservices are categorized into three types:

1. **Core Microservices** (Traditional Containerized Services):
   - Authentication & Authorization (high security, complex logic)
   - Course Management (complex domain logic)
   - Content Delivery (high performance, storage integration)
   - Assessment Engine (specialized algorithms)
   - Logging & Monitoring (high throughput, complex processing)

2. **Edge Functions** (Supabase Edge Functions):
   - Real-time Notifications
   - User Presence & Activity Tracking
   - Content Access Authorization
   - Simple API Endpoints
   - Webhooks for Third-Party Integrations

3. **Shared Backend Services** (Supabase PostgreSQL + Extensions):
   - Database with Row-Level Security
   - Real-time Subscriptions
   - Storage for Course Content
   - Authentication Foundation

---

## Supabase Edge Functions Integration

### When to Use Edge Functions

Supabase Edge Functions are ideal for:

1. **Global Distribution**: Functions that benefit from edge computing for low latency
2. **Lightweight Processing**: Simple API endpoints and webhooks
3. **Event-Driven Logic**: Code that responds to database changes or webhooks
4. **Real-Time Features**: User presence, notifications, and other real-time services

### Edge Function Implementation Structure

```
supabase/
├── functions/
│   ├── auth-webhook/
│   │   └── index.ts          # Authentication webhook
│   ├── log-events/
│   │   └── index.ts          # Activity logging function
│   ├── analytics/
│   │   └── index.ts          # Event tracking functions
│   └── notification/
│       └── index.ts          # User notification system
└── config.toml               # Supabase configuration
```

### Example Edge Function Implementation

```typescript
// supabase/functions/log-events/index.ts

import { serve } from 'https://deno.land/std@0.131.0/http/server.ts';
import { createClient } from 'https://esm.sh/@supabase/supabase-js@2.0.0';

const supabaseUrl = Deno.env.get('SUPABASE_URL') as string;
const supabaseKey = Deno.env.get('SUPABASE_SERVICE_ROLE_KEY') as string;

serve(async (req) => {
  const supabase = createClient(supabaseUrl, supabaseKey);
  
  try {
    const { events, tenantId } = await req.json();
    
    // Validate tenant context from JWT
    const { data: { user } } = await supabase.auth.getUser(
      req.headers.get('Authorization')?.replace('Bearer ', '') || ''
    );
    
    if (!user || (user.app_metadata.tenant_id !== tenantId && 
        !user.app_metadata.roles?.includes('super_admin'))) {
      return new Response(JSON.stringify({ error: 'Unauthorized' }), { 
        status: 401, 
        headers: { 'Content-Type': 'application/json' } 
      });
    }
    
    // Process and store logs
    const { data, error } = await supabase
      .from('event_logs')
      .insert(events.map(event => ({
        tenant_id: tenantId,
        event_type: event.type,
        payload: event.data,
        user_id: user.id,
        timestamp: new Date().toISOString()
      })));
      
    if (error) throw error;
    
    return new Response(JSON.stringify({ success: true }), {
      status: 200,
      headers: { 'Content-Type': 'application/json' }
    });
  } catch (error) {
    return new Response(JSON.stringify({ error: error.message }), {
      status: 500,
      headers: { 'Content-Type': 'application/json' }
    });
  }
});
```

---

## Service Communication Patterns

### Synchronous Communication

For direct service-to-service interactions, we use:

1. **REST APIs**: 
   - Standard HTTP methods and status codes
   - OpenAPI/Swagger documentation
   - Rate limiting and circuit breakers

2. **GraphQL API**: 
   - Complex data requirements with minimal data transfer
   - Schema-based type checking
   - Batched queries for frontend efficiency

3. **gRPC**: 
   - High-performance internal communication between services
   - Strongly typed via Protocol Buffers
   - Bidirectional streaming capabilities

### Asynchronous Communication

For resilient, decoupled communication:

1. **Event-Driven Architecture**: 
   - Apache Kafka as the primary message broker
   - Avro schema validation for message contracts
   - Event versioning and compatibility management

2. **Message Schema Examples**:

```typescript
// examples/schemas/event-schemas.ts

interface BaseEvent {
  eventId: string;
  tenantId: string;
  timestamp: string;
  eventType: string;
  version: string;
}

interface UserRegisteredEvent extends BaseEvent {
  eventType: 'user.authentication.user_registered';
  data: {
    userId: string;
    email: string;
    role: string;
    registrationMethod: string;
  };
}

interface CourseEnrollmentEvent extends BaseEvent {
  eventType: 'course.management.enrollment_changed';
  data: {
    userId: string;
    courseId: string;
    action: 'enrolled' | 'unenrolled';
    timestamp: string;
  };
}
```

---

## Development Workflow

### Local Development

For efficient microservices development:

1. **Setup and Dependencies**:
   - Docker Compose for local service orchestration
   - Local Supabase instance with CLI support
   - Service mocks for external dependencies
   - Shared development environment variables

2. **Code Structure for Core Services**:

```typescript
// Example NestJS microservice structure
// services/course-service/src/main.ts

import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { MicroserviceOptions, Transport } from '@nestjs/microservices';
import { ConfigService } from '@nestjs/config';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  const configService = app.get(ConfigService);

  // Connect to message broker
  app.connectMicroservice<MicroserviceOptions>({
    transport: Transport.KAFKA,
    options: {
      client: {
        brokers: configService.get<string>('KAFKA_BROKERS').split(','),
      },
      consumer: {
        groupId: 'course-service',
      },
    },
  });

  await app.startAllMicroservices();
  await app.listen(configService.get<number>('HTTP_PORT') || 3000);
}

bootstrap();
```

3. **Testing Approaches**:
   - Unit tests for individual services
   - Integration tests for service communication
   - End-to-end tests for critical workflows
   - Contract tests for service interfaces

---

## Deployment Strategy

### CI/CD Pipeline

```yaml
# Example GitHub Actions workflow for microservices
name: Deploy Microservices

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        service: [auth-service, course-service, logging-service]
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: cd services/${{ matrix.service }} && npm install
      
      - name: Run tests
        run: cd services/${{ matrix.service }} && npm test

  build-and-deploy:
    needs: build-and-test
    runs-on: ubuntu-latest
    strategy:
      matrix:
        service: [auth-service, logging-service, course-service]
    steps:
      - uses: actions/checkout@v3
      
      - name: Build Docker image
        run: |
          cd services/${{ matrix.service }}
          docker build -t eduspry/${{ matrix.service }}:${{ github.sha }} .
      
      - name: Push to container registry
        run: |
          echo ${{ secrets.REGISTRY_PASSWORD }} | docker login -u ${{ secrets.REGISTRY_USERNAME }} --password-stdin
          docker push eduspry/${{ matrix.service }}:${{ github.sha }}
      
      - name: Deploy to Kubernetes
        uses: Azure/k8s-deploy@v1
        with:
          manifests: |
            infrastructure/kubernetes/${{ matrix.service }}/deployment.yaml
          images: |
            eduspry/${{ matrix.service }}:${{ github.sha }}

  deploy-edge-functions:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Supabase CLI
        uses: supabase/setup-cli@v1
        with:
          version: latest
      
      - name: Deploy edge functions
        run: |
          supabase functions deploy --project-ref ${{ secrets.SUPABASE_PROJECT_ID }}
```

### Kubernetes Configuration

Our microservices are deployed using Kubernetes for orchestration:

1. **Deployment Configuration**:
   - Resource constraints and auto-scaling
   - Health checks and readiness probes
   - Rolling updates and blue-green deployments
   - Service mesh integration with Istio

2. **Example Kubernetes Manifest**:

```yaml
# infrastructure/kubernetes/course-service/deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: course-service
  namespace: eduspry
spec:
  replicas: 3
  selector:
    matchLabels:
      app: course-service
  template:
    metadata:
      labels:
        app: course-service
    spec:
      containers:
      - name: course-service
        image: eduspry/course-service:latest
        ports:
        - containerPort: 3000
        resources:
          limits:
            cpu: "1"
            memory: "1Gi"
          requests:
            cpu: "500m"
            memory: "512Mi"
        readinessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 10
        env:
        - name: NODE_ENV
          value: "production"
        - name: KAFKA_BROKERS
          valueFrom:
            configMapKeyRef:
              name: service-config
              key: kafka-brokers
        - name: SUPABASE_URL
          valueFrom:
            configMapKeyRef:
              name: service-config
              key: supabase-url
        - name: SUPABASE_KEY
          valueFrom:
            secretKeyRef:
              name: service-secrets
              key: supabase-key
---
apiVersion: v1
kind: Service
metadata:
  name: course-service
  namespace: eduspry
spec:
  selector:
    app: course-service
  ports:
  - port: 80
    targetPort: 3000
  type: ClusterIP
```

---

## Monitoring and Observability

### Logging Framework

EduSpry implements a comprehensive logging system to ensure full visibility across all services, environments, and tenants. For detailed implementation, refer to our [Logging Framework Strategy](./Logging_Framework_Strategy.md) document.

Key aspects of our logging framework include:

1. **Log Collection**:
   - Structured JSON logs from all services
   - Fluentd/Fluent Bit agents for log collection
   - Kafka for log streaming
   - ELK stack for storage, indexing, and visualization

2. **Standardized Log Format**:
```json
{
  "timestamp": "2023-05-25T08:15:30.123Z",
  "level": "INFO",
  "service": "course-service",
  "component": "enrollment-manager",
  "traceId": "abc-123-def-456",
  "spanId": "span-789",
  "tenantId": "tenant-123",
  "tenantName": "riverside-school-district",
  "userId": "user-456",
  "sessionId": "session-321",
  "message": "User enrolled in course",
  "context": {
    "courseId": "course-789",
    "enrollmentType": "self",
    "userRole": "student",
    "browser": "Chrome/90.0.4430.212",
    "clientIp": "192.168.x.x"
  },
  "tags": ["enrollment", "course", "user-action"],
  "metrics": {
    "processingTimeMs": 45,
    "databaseQueriesCount": 3
  },
  "environment": "production"
}
```

### Application Performance Monitoring

1. **Key Metrics**:
   - Request rates, latencies, and error rates
   - Resource utilization (CPU, memory, disk, network)
   - Database query performance
   - Kafka message processing rates
   - Cache hit/miss ratios

2. **Distributed Tracing**:
   - OpenTelemetry for instrumentation
   - Jaeger for trace visualization
   - Custom span attributes for educational context
   - Critical path analysis for performance optimization

### Alerting Strategy

1. **Alert Categories**:
   - System health and availability
   - Performance degradation
   - Security incidents
   - Business metrics anomalies
   - Compliance and audit alerts

2. **Alerting Channels**:
   - PagerDuty for critical incidents
   - Slack for team notifications
   - Email for non-urgent issues
   - SMS for critical production issues

---

## Cloud Portability and Multi-Cloud Strategy

### Platform Agnostic Architecture

EduSpry's microservices architecture is designed with cloud portability as a core principle, allowing for deployment across various cloud platforms (AWS, Azure, GCP, OCI) with minimal adaptation:

1. **Containerization First**: 
   - All services containerized with Docker
   - Kubernetes as the orchestration layer
   - Helm charts for consistent deployment across providers

2. **Infrastructure Abstraction**:
   - Infrastructure as Code templates for multiple providers
   - Terraform modules with provider-specific implementations
   - Clear separation between business logic and infrastructure concerns

3. **Dependency Management**:
   - Minimize use of cloud provider-specific services
   - Use abstraction layers around cloud-specific APIs
   - Consistent interface patterns across different implementations

### Service Portability Considerations

| Service Category | Portability Strategy |
|-----------------|----------------------|
| Core Microservices | 100% portable containerized services |
| Edge Functions | Abstracted via adapter pattern for alternative serverless platforms |
| Database | Migration path from Supabase PostgreSQL to self-hosted or managed PostgreSQL |
| Authentication | OAuth/OIDC standards with pluggable provider implementations |
| Storage | S3-compatible API abstraction with provider-specific adapters |
| Messaging | Kafka-compatible messaging with pluggable provider options |

### Implementation Guidelines

1. **Abstraction Layers**:
   - Create adapter interfaces for all external dependencies
   - Example for storage service:

```typescript
// lib/storage/interfaces.ts
export interface StorageProvider {
  uploadFile(bucket: string, path: string, file: Buffer): Promise<string>;
  getFileUrl(bucket: string, path: string): Promise<string>;
  deleteFile(bucket: string, path: string): Promise<void>;
}

// lib/storage/providers/supabase.ts
export class SupabaseStorageProvider implements StorageProvider {
  // Supabase implementation
}

// lib/storage/providers/aws-s3.ts
export class AWSS3Provider implements StorageProvider {
  // AWS implementation
}
```

2. **Configuration Management**:
   - Environment-based configuration
   - Provider-specific settings isolated
   - Feature flags for provider capabilities

3. **Migration Planning**:
   - Documented migration paths for each service
   - Data export and import utilities
   - Traffic migration strategies
   - Testing frameworks for cross-provider validation

### Provider-Specific Considerations

| Cloud Provider | Key Considerations |
|---------------|-------------------|
| AWS | Leverage EKS for Kubernetes, RDS for PostgreSQL, MSK for Kafka |
| Azure | Use AKS for Kubernetes, Azure Database for PostgreSQL, Event Hubs for Kafka |
| GCP | Deploy on GKE, Cloud SQL for PostgreSQL, Pub/Sub with Kafka compatibility |
| OCI | Oracle Kubernetes Engine, Autonomous Database, Streaming service |

By following these guidelines, EduSpry's microservices architecture ensures long-term flexibility and prevents vendor lock-in, allowing the platform to adapt to changing business needs and leverage the best-in-class services across multiple cloud providers.

---

*This implementation guide provides the technical foundation for building and maintaining the microservice architecture for the EduSpry platform. It should be used in conjunction with the main Architecture document and Technical Details.*
