# EduSpry Logging Framework Strategy

**Document Version:** 1.0  
**Created Date:** 2025-05-25  
**Last Updated:** 2025-05-25  
**Document Type:** Technical Implementation Guide

---

## Table of Contents

1. [Logging Philosophy](#logging-philosophy)
2. [Logging Architecture](#logging-architecture)
3. [Standardized Log Format](#standardized-log-format)
4. [Service-Specific Logging Strategies](#service-specific-logging-strategies)
5. [Log Collection and Storage](#log-collection-and-storage)
6. [Log Analysis and Visualization](#log-analysis-and-visualization)
7. [AI-Assisted Log Analysis](#ai-assisted-log-analysis)
8. [Tenant-Aware Logging](#tenant-aware-logging)
9. [Cloud-Agnostic Implementation](#cloud-agnostic-implementation)
10. [Implementation Guidelines](#implementation-guidelines)

---

## Logging Philosophy

### Key Principles

EduSpry's logging framework is built on the following core principles:

1. **Comprehensive Visibility**: Capture all meaningful system and business events
2. **Structured Consistency**: All logs follow a standardized JSON format for machine readability
3. **Context Richness**: Include relevant context with every log entry for troubleshooting
4. **Performance Sensitivity**: Efficient logging that doesn't impact system performance
5. **Privacy by Design**: Prevent sensitive data from entering logs
6. **AI-Ready Format**: Structure logs to facilitate AI-based analysis and insights
7. **Multi-Tenant Awareness**: Ensure logs maintain tenant context for isolation and analysis

---

## Logging Architecture

### Centralized Logging System

EduSpry implements a centralized logging architecture with these components:

1. **Log Generation**:
   - Service-specific loggers with standardized interfaces
   - Context enrichment at the source
   - Log level management per tenant and environment

2. **Log Transport**:
   - Kafka streams for real-time log aggregation
   - Batched background uploads for non-critical logs
   - Edge function logging for serverless components

3. **Log Processing**:
   - Stream processing for real-time alerts
   - Enrichment with additional metadata
   - PII detection and masking
   - Tenant-specific routing

4. **Storage and Retention**:
   - Hot storage for recent logs (Elasticsearch)
   - Warm storage for monthly logs (Object Storage)
   - Cold storage for compliance and audit (Long-term Archive)
   - Tenant-specific retention policies

### Logging Service Architecture

```
                    ┌──────────────────┐
                    │                  │
                    │    Microservices │
                    │                  │
                    └────────┬─────────┘
                             │
                             ▼
┌─────────────┐     ┌──────────────────┐     ┌─────────────────┐
│             │     │                  │     │                 │
│  Fluentd    │────▶│  Kafka Streams   │────▶│  Elasticsearch  │
│  Agents     │     │                  │     │                 │
└─────────────┘     └──────────────────┘     └────────┬────────┘
                             ▲                        │
                             │                        ▼
┌─────────────┐     ┌──────────────────┐     ┌─────────────────┐
│             │     │                  │     │                 │
│  Edge       │────▶│  Log Processing  │────▶│  Kibana         │
│  Functions  │     │  Service         │     │  Dashboards     │
└─────────────┘     └──────────────────┘     └─────────────────┘
                             ▲
                             │
┌─────────────┐     ┌──────────────────┐     ┌─────────────────┐
│             │     │                  │     │                 │
│  Client     │────▶│  API Gateway     │────▶│  Long-term      │
│  Apps       │     │  Logging         │     │  Storage        │
└─────────────┘     └──────────────────┘     └─────────────────┘
```

---

## Standardized Log Format

### Base Log Structure

All logs in the EduSpry platform adhere to a standardized JSON format:

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
    "clientIp": "192.168.x.x",
    "apiVersion": "v1"
  },
  "tags": ["enrollment", "course", "user-action"],
  "metrics": {
    "processingTimeMs": 45,
    "databaseQueriesCount": 3
  },
  "environment": "production"
}
```

### Log Levels and Usage Guidelines

| Level | Usage |
|-------|-------|
| TRACE | Detailed debug information, typically used for step-by-step execution tracing |
| DEBUG | Debugging information useful during development |
| INFO | General information about system operation |
| WARN | Warning situations that might require attention |
| ERROR | Error conditions that might still allow the service to continue |
| FATAL | Severe error conditions that cause service failure |

---

## Service-Specific Logging Strategies

### Authentication Service

```typescript
// services/auth-service/src/utils/logger.ts
import { createLogger } from '@eduspry/logging';

export const logger = createLogger({
  service: 'auth-service',
  defaultContext: {
    component: 'authentication',
  },
  sensitiveFields: ['password', 'token', 'apiKey'],
});

// Usage example
logger.info('User authenticated successfully', {
  userId: user.id,
  authMethod: 'email',
  tenantId: tenant.id,
  metrics: {
    authDurationMs: process.hrtime(startTime)[1] / 1000000,
  }
});
```

### Course Management Service

```typescript
// services/course-service/src/utils/logger.ts
import { createLogger } from '@eduspry/logging';

export const logger = createLogger({
  service: 'course-service',
  defaultContext: {
    component: 'course-management',
  },
});

// Usage example
logger.info('Course enrollment changed', {
  userId: user.id,
  courseId: course.id,
  action: 'enrolled',
  tenantId: tenant.id,
  context: {
    enrollmentSource: 'admin-assignment',
    courseType: course.type,
  },
  tags: ['enrollment', 'course-update']
});
```

### Frontend Logging

```typescript
// packages/logging/src/client-logger.ts
import { browserLogger } from '@eduspry/logging/client';

const logger = browserLogger({
  service: 'web-app',
  defaultContext: {
    clientType: 'browser',
    appVersion: process.env.NEXT_PUBLIC_APP_VERSION,
  },
  transportOptions: {
    endpoint: '/api/logs',
    batchSize: 10,
    flushInterval: 5000,
  }
});

// Usage example
logger.info('User viewed course dashboard', {
  courseId: courseId,
  viewType: 'list',
  metrics: {
    pageLoadTimeMs: performance.now() - pageLoadStart,
  }
});
```

---

## Log Collection and Storage

### Collection Strategy

1. **Server-Side Logging**:
   - Fluentd agents on each node
   - Sidecar pattern for containerized services
   - Direct Kafka producers for high-volume services

2. **Client-Side Logging**:
   - Batched API calls to logging endpoints
   - Service worker buffering for offline logs
   - Prioritization of critical client-side errors

### Storage Implementation

1. **Short-term Storage**:
   - Elasticsearch clusters for active logs
   - 30-day retention for full-fidelity logs
   - Tenant-specific indices for isolation

2. **Long-term Storage**:
   - Cloud object storage for archival (provider-agnostic)
   - Compressed and partitioned by tenant/date
   - Lifecycle policies for cost optimization

3. **Compliance and Audit**:
   - Immutable storage for security audit logs
   - Digital signatures for log integrity
   - WORM (Write Once Read Many) storage for regulated tenants

---

## Log Analysis and Visualization

### Kibana Dashboards

1. **Operational Dashboards**:
   - Service health and performance
   - Error rates and distribution
   - Latency percentiles and outliers

2. **Business Intelligence**:
   - User engagement patterns
   - Course completion analytics
   - Feature usage patterns

3. **Security Monitoring**:
   - Authentication anomalies
   - Access control violations
   - Rate limiting and abuse detection

### Custom Analysis Tools

1. **Real-time Alerting**:
   - Elasticsearch Watcher for threshold-based alerts
   - Anomaly detection for unusual patterns
   - Correlation rules for complex event detection

2. **Log Query Interface**:
   - Admin portal integration
   - Saved queries for common troubleshooting
   - Export capabilities for detailed analysis

---

## AI-Assisted Log Analysis

### Machine Learning Pipeline

1. **Automated Classification**:
   - Error categorization and clustering
   - Automatic priority assignment
   - Root cause analysis suggestions

2. **Pattern Recognition**:
   - Anomaly detection for performance issues
   - Session reconstruction for user journey analysis
   - Predictive maintenance based on error patterns

3. **Natural Language Processing**:
   - Log message tokenization and entity extraction
   - Sentiment analysis for user feedback
   - Intent detection for user actions

### Implementation Architecture

```typescript
// Example log processing with AI analysis
// services/logging-service/src/processors/ai-analyzer.ts

import { LogBatch, AIInsight } from '../types';
import { OpenAIClient } from '../clients/openai';

export class LogAnalyzer {
  private aiClient: OpenAIClient;
  
  constructor() {
    this.aiClient = new OpenAIClient(process.env.OPENAI_API_KEY);
  }
  
  async analyzeErrorLogs(logs: LogBatch): Promise<AIInsight[]> {
    // Filter for error logs
    const errorLogs = logs.filter(log => log.level === 'ERROR');
    
    if (errorLogs.length === 0) return [];
    
    // Group errors by pattern
    const groupedErrors = this.groupSimilarErrors(errorLogs);
    
    // For each group, generate insights
    const insights = await Promise.all(
      Object.entries(groupedErrors).map(async ([pattern, logs]) => {
        const analysis = await this.aiClient.analyzeErrors({
          pattern,
          sampleLogs: logs.slice(0, 5), // Send sample logs
          frequency: logs.length,
          timeRange: {
            start: logs[0].timestamp,
            end: logs[logs.length - 1].timestamp
          }
        });
        
        return {
          pattern,
          logCount: logs.length,
          suggestedRootCause: analysis.rootCause,
          recommendedAction: analysis.recommendation,
          impactAssessment: analysis.impact,
          confidence: analysis.confidence
        };
      })
    );
    
    return insights;
  }
  
  private groupSimilarErrors(logs: any[]): Record<string, any[]> {
    // Implementation of error grouping algorithm
    const groups: Record<string, any[]> = {};
    // ...grouping logic
    return groups;
  }
}
```

---

## Tenant-Aware Logging

### Multi-tenant Considerations

1. **Data Isolation**:
   - Tenant ID embedded in every log record
   - Tenant-specific log indices
   - Permission-based access control for logs

2. **Tenant-specific Logging Policies**:
   - Configurable verbosity per tenant
   - Custom retention policies
   - Tenant-specific PII handling rules

3. **Admin Access**:
   - Super-admin access to all tenant logs
   - Tenant admin access to tenant-specific logs only
   - Audit logging for admin access to logs

### Implementation Example

```typescript
// packages/logging/src/tenant-context.ts
import { AsyncLocalStorage } from 'async_hooks';
import { TenantContext } from './types';

const tenantStorage = new AsyncLocalStorage<TenantContext>();

export const withTenant = async <T>(
  tenantId: string, 
  operation: () => Promise<T>
): Promise<T> => {
  const tenantContext: TenantContext = {
    tenantId,
    timestamp: new Date().toISOString()
  };
  
  return tenantStorage.run(tenantContext, operation);
};

export const getTenantContext = (): TenantContext | undefined => {
  return tenantStorage.getStore();
};

// Middleware usage example
app.use((req, res, next) => {
  const tenantId = req.headers['x-tenant-id'] as string;
  if (!tenantId) {
    return res.status(400).json({ error: 'Missing tenant ID' });
  }
  
  withTenant(tenantId, async () => {
    try {
      await next();
    } catch (error) {
      logger.error('Request processing error', {
        error: error.message,
        stack: error.stack,
        path: req.path,
        method: req.method
      });
      res.status(500).json({ error: 'Internal server error' });
    }
  });
});
```

---

## Cloud-Agnostic Implementation

### Provider Abstraction

The logging framework is implemented with provider abstraction layers to ensure portability across cloud platforms:

1. **Storage Abstractions**:
   - Interface-based design for log storage
   - Provider-specific implementations (AWS, Azure, GCP, OCI)
   - Consistent API for all storage operations

2. **Stream Processing**:
   - Kafka-compatible messaging for log streaming
   - Adaptable to cloud-specific streaming services
   - Consistent consumer interfaces

3. **Monitoring Integration**:
   - Prometheus-compatible metrics
   - Cloud-agnostic alerting definitions
   - Portable dashboard specifications

### Implementation Example

```typescript
// lib/logging/storage/interfaces.ts
export interface LogStorageProvider {
  storeLogBatch(batch: LogBatch): Promise<void>;
  queryLogs(query: LogQuery): Promise<LogQueryResult>;
  getLogStream(filter: LogFilter): Observable<LogEntry>;
  archiveLogs(olderThan: Date, options: ArchiveOptions): Promise<ArchiveResult>;
}

// lib/logging/storage/providers/aws.ts
export class AWSLogStorage implements LogStorageProvider {
  private s3Client: S3Client;
  private firehoseClient: FirehoseClient;
  private esClient: ElasticsearchClient;
  
  constructor(config: AWSLogStorageConfig) {
    this.s3Client = new S3Client(config.s3);
    this.firehoseClient = new FirehoseClient(config.firehose);
    this.esClient = new ElasticsearchClient(config.elasticsearch);
  }
  
  // Implementation of interface methods using AWS services
  // ...
}

// lib/logging/storage/providers/azure.ts
export class AzureLogStorage implements LogStorageProvider {
  private blobClient: BlobServiceClient;
  private eventHubClient: EventHubClient;
  private searchClient: SearchClient;
  
  // Implementation for Azure
  // ...
}
```

---

## Implementation Guidelines

### Developer Workflow

1. **Service Logger Creation**:

```typescript
// In each microservice:
import { createServiceLogger } from '@eduspry/logging';

export const logger = createServiceLogger({
  service: 'service-name',
  version: process.env.SERVICE_VERSION,
});
```

2. **Logging Best Practices**:

- Always include tenant context
- Use structured logging with typed context
- Follow consistent naming for common fields
- Include relevant business context
- Avoid logging sensitive information
- Use appropriate log levels

3. **Performance Considerations**:

- Use sampling for high-volume logs
- Implement asynchronous logging
- Consider log buffering for burst scenarios
- Set appropriate log levels per environment
- Use conditional logging for expensive operations

### Logging Library Structure

```
packages/logging/
├── src/
│   ├── index.ts                # Main export
│   ├── logger.ts               # Core logger implementation
│   ├── formatters/             # Log formatting utilities
│   │   ├── json.ts             # JSON formatter
│   │   └── console.ts          # Human-readable formatter
│   ├── transports/            
│   │   ├── console.ts          # Console output
│   │   ├── file.ts             # File output
│   │   ├── kafka.ts            # Kafka transport
│   │   └── http.ts             # HTTP API transport
│   ├── processors/             # Log processors
│   │   ├── pii-masker.ts       # PII detection and masking
│   │   ├── error-enhancer.ts   # Error stack and context enhancement
│   │   └── tenant-enricher.ts  # Tenant context enrichment
│   ├── client/                 # Client-side logging
│   │   ├── browser-logger.ts   # Browser implementation
│   │   └── react-logger.ts     # React hooks and components
│   └── types/                  # TypeScript types
│       ├── log-entry.ts        # Log entry structure
│       └── context.ts          # Context types
├── test/                       # Unit tests
└── examples/                   # Usage examples
```

---

*This documentation provides comprehensive guidance on implementing the EduSpry logging framework. It should be used in conjunction with the Microservices Implementation Guide and other architecture documents to ensure consistent logging practices across the platform.*
