# Project Documentation Prompt Library

A comprehensive collection of prompts for generating Product Requirements Documents (PRD), Technical Documentation, and Architectural Documents when creating new projects or applications.

## Table of Contents

1. [Product Requirements Document (PRD) Prompts](#product-requirements-document-prd-prompts)
2. [Technical Documentation Prompts](#technical-documentation-prompts)
3. [Architectural Documentation Prompts](#architectural-documentation-prompts)
4. [Project Planning Prompts](#project-planning-prompts)
5. [Quality Assurance Prompts](#quality-assurance-prompts)
6. [Deployment & Operations Prompts](#deployment--operations-prompts)

---

## Product Requirements Document (PRD) Prompts

### 1. Executive Summary & Vision Prompt

```txt
Generate a comprehensive executive summary and product vision for a new [PROJECT_TYPE] application. Include:

- Project overview and purpose
- Target market and user base
- Key value propositions
- Business objectives and success metrics
- High-level timeline and milestones
- Resource requirements overview

Context: [Provide specific project details, industry, and business goals]

Format the output as a professional executive summary suitable for stakeholders and investors.
```

### 2. User Personas & Journey Mapping Prompt

```txt
Create detailed user personas and user journey maps for [PROJECT_NAME]. Include:

**For each persona:**
- Demographics and background
- Goals and motivations
- Pain points and challenges
- Technical proficiency level
- Preferred interaction methods

**For user journeys:**
- Step-by-step user flows
- Touchpoints and interactions
- Potential friction points
- Success criteria for each step
- Emotional journey mapping

Project context: [Describe the application type, target audience, and primary use cases]

Provide 3-5 primary personas and their corresponding journey maps.
```

### 3. Functional Requirements Prompt

```txt
Generate comprehensive functional requirements for [PROJECT_NAME] application. Structure as follows:

**Core Features:**
- Primary functionality descriptions
- User story format (As a [user], I want [goal] so that [benefit])
- Acceptance criteria for each feature
- Priority levels (Must-have, Should-have, Could-have, Won't-have)

**Feature Categories:**
- User authentication and authorization
- Core business logic features
- Data management capabilities
- Integration requirements
- Reporting and analytics
- Administrative functions

**For each feature include:**
- Detailed description
- User interactions
- Input/output specifications
- Business rules and constraints
- Dependencies on other features

Project details: [Provide application type, industry, and specific business requirements]
```

### 4. Non-Functional Requirements Prompt

```txt
Create detailed non-functional requirements (NFRs) for [PROJECT_NAME]. Cover:

**Performance Requirements:**
- Response time targets
- Throughput expectations
- Scalability requirements
- Resource utilization limits

**Security Requirements:**
- Authentication mechanisms
- Authorization levels
- Data encryption standards
- Compliance requirements (GDPR, HIPAA, etc.)
- Security testing requirements

**Reliability & Availability:**
- Uptime targets (99.9%, 99.99%, etc.)
- Recovery time objectives (RTO)
- Recovery point objectives (RPO)
- Disaster recovery requirements

**Usability & Accessibility:**
- User experience standards
- Accessibility compliance (WCAG 2.1)
- Cross-platform compatibility
- Internationalization needs

**Operational Requirements:**
- Monitoring and logging
- Backup and maintenance
- Support and documentation

Project context: [Specify application type, expected user load, and business criticality]
```

### 5. Success Metrics & KPIs Prompt

```txt
Define comprehensive success metrics and KPIs for [PROJECT_NAME]. Include:

**Business Metrics:**
- Revenue impact measurements
- Cost reduction targets
- Market share objectives
- Customer acquisition costs
- Customer lifetime value

**User Engagement Metrics:**
- Daily/Monthly active users
- User retention rates
- Feature adoption rates
- User satisfaction scores
- Time-to-value metrics

**Technical Performance Metrics:**
- System availability and uptime
- Response time percentiles
- Error rates and resolution times
- Security incident frequencies
- Deployment frequency and success rates

**Operational Metrics:**
- Development velocity
- Bug discovery and resolution rates
- Support ticket volumes
- Documentation completeness

For each metric, specify:
- Measurement method
- Target values
- Monitoring frequency
- Responsibility ownership

Project details: [Provide business context and strategic objectives]
```

---

## Technical Documentation Prompts

### 6. Technical Architecture Overview Prompt

```txt
Create a comprehensive technical architecture document for [PROJECT_NAME]. Include:

**System Architecture:**
- High-level architecture diagram description
- Component relationships and dependencies
- Data flow between systems
- Integration points and APIs
- Third-party service dependencies

**Technology Stack:**
- Frontend technologies and frameworks
- Backend services and frameworks
- Database technologies
- Infrastructure and cloud services
- Development and deployment tools

**Architecture Patterns:**
- Design patterns implemented
- Architectural style (microservices, monolith, serverless)
- Communication patterns (REST, GraphQL, event-driven)
- Data architecture patterns

**Scalability & Performance:**
- Horizontal and vertical scaling strategies
- Caching strategies
- Load balancing approaches
- Database optimization techniques

Project context: [Specify application type, expected scale, and technical constraints]
```

### 7. API Documentation Prompt

```txt
Generate comprehensive API documentation for [PROJECT_NAME]. Structure as follows:

**API Overview:**
- API purpose and scope
- Authentication and authorization methods
- Base URLs and versioning strategy
- Rate limiting and throttling policies
- Error handling standards

**Endpoint Documentation:**
For each endpoint, include:
- HTTP method and URL pattern
- Request parameters (path, query, body)
- Request/response examples
- Status codes and error responses
- Authentication requirements
- Rate limits and constraints

**Data Models:**
- Schema definitions for all entities
- Field descriptions and constraints
- Relationship mappings
- Validation rules

**Integration Examples:**
- SDK/client library usage
- Common integration patterns
- Webhook configurations
- Testing and sandbox information

Project details: [Describe the API purpose, target integrators, and technical requirements]
```

### 8. Database Design Documentation Prompt

```txt
Create detailed database design documentation for [PROJECT_NAME]. Include:

**Database Schema:**
- Entity relationship diagrams (ERD) description
- Table structures and relationships
- Primary and foreign key definitions
- Index strategies and constraints
- Data types and field specifications

**Data Architecture:**
- Database technology selection rationale
- Partitioning and sharding strategies
- Backup and recovery procedures
- Data retention policies
- Migration strategies

**Performance Optimization:**
- Query optimization guidelines
- Indexing strategies
- Caching mechanisms
- Connection pooling configuration
- Monitoring and alerting setup

**Security & Compliance:**
- Data encryption at rest and in transit
- Access control and permissions
- Audit logging requirements
- Compliance considerations (GDPR, PCI-DSS)
- Data anonymization strategies

Project context: [Specify data requirements, expected volume, and compliance needs]
```

### 9. Security Implementation Guide Prompt

```txt
Develop a comprehensive security implementation guide for [PROJECT_NAME]. Cover:

**Authentication & Authorization:**
- Identity provider integration
- Multi-factor authentication setup
- Role-based access control (RBAC)
- Session management
- Token-based authentication (JWT, OAuth2)

**Data Protection:**
- Encryption standards and implementation
- Data classification and handling
- Secure data transmission protocols
- Database security measures
- API security best practices

**Infrastructure Security:**
- Network security configurations
- Container and cloud security
- Secrets management
- Security monitoring and logging
- Vulnerability scanning procedures

**Security Testing:**
- Penetration testing procedures
- Security code review guidelines
- Automated security testing integration
- Security compliance validation
- Incident response procedures

**Compliance Framework:**
- Regulatory compliance requirements
- Security audit procedures
- Documentation and reporting requirements
- Risk assessment methodologies

Project details: [Specify security requirements, compliance standards, and risk profile]
```

### 10. Development Guidelines Prompt

```txt
Create comprehensive development guidelines for [PROJECT_NAME]. Include:

**Coding Standards:**
- Language-specific style guides
- Naming conventions
- Code organization principles
- Documentation requirements
- Comment standards

**Development Workflow:**
- Git branching strategy
- Code review procedures
- Pull request templates
- Continuous integration setup
- Testing requirements at each stage

**Quality Assurance:**
- Unit testing standards
- Integration testing procedures
- End-to-end testing strategies
- Code coverage requirements
- Performance testing guidelines

**Tools & Environment:**
- Development environment setup
- Required IDE configurations
- Linting and formatting tools
- Debugging procedures
- Local development guidelines

**Collaboration:**
- Team communication protocols
- Documentation standards
- Knowledge sharing practices
- Onboarding procedures for new developers

Technology stack: [Specify programming languages, frameworks, and tools]
```

---

## Architectural Documentation Prompts

### 11. System Architecture Design Prompt

```txt
Design a comprehensive system architecture for [PROJECT_NAME]. Include:

**Architecture Overview:**
- System context diagram
- High-level component architecture
- Service boundaries and responsibilities
- Data flow diagrams
- Integration architecture

**Component Design:**
- Microservices decomposition (if applicable)
- Service communication patterns
- API gateway configuration
- Load balancing strategies
- Service discovery mechanisms

**Data Architecture:**
- Data storage strategies
- Database selection rationale
- Data synchronization approaches
- Event sourcing and CQRS patterns
- Data pipeline architecture

**Infrastructure Architecture:**
- Cloud platform selection
- Container orchestration strategy
- Networking and security architecture
- Monitoring and observability setup
- Disaster recovery architecture

**Scalability Design:**
- Horizontal scaling strategies
- Auto-scaling configurations
- Performance optimization approaches
- Caching layer design
- CDN integration strategy

Project requirements: [Specify scale requirements, technology preferences, and constraints]
```

### 12. Microservices Architecture Prompt

```txt
Design a microservices architecture for [PROJECT_NAME]. Structure as follows:

**Service Decomposition:**
- Business domain analysis
- Service boundary identification
- Service responsibility definition
- Data ownership mapping
- API contract specifications

**Communication Patterns:**
- Synchronous communication (REST, GraphQL)
- Asynchronous messaging patterns
- Event-driven architecture design
- Saga pattern for distributed transactions
- Circuit breaker implementations

**Data Management:**
- Database per service strategy
- Data consistency patterns
- Event sourcing implementation
- CQRS pattern application
- Data synchronization mechanisms

**Infrastructure Patterns:**
- Service mesh configuration
- API gateway setup
- Service discovery implementation
- Configuration management
- Secrets management across services

**Operational Considerations:**
- Distributed tracing setup
- Centralized logging strategy
- Health check implementations
- Deployment strategies (blue-green, canary)
- Testing strategies for distributed systems

Business domain: [Describe the business context and complexity requirements]
```

### 13. Cloud Architecture Design Prompt

```txt
Create a cloud-native architecture design for [PROJECT_NAME]. Include:

**Cloud Strategy:**
- Cloud provider selection rationale
- Multi-cloud vs single-cloud strategy
- Cloud-native services utilization
- Migration strategy (if applicable)
- Cost optimization approaches

**Infrastructure as Code:**
- Infrastructure provisioning strategy
- Configuration management approach
- Environment management (dev/staging/prod)
- Automated deployment pipelines
- Infrastructure monitoring and alerting

**Serverless Architecture:**
- Function-as-a-Service (FaaS) implementation
- Event-driven serverless patterns
- API Gateway integration
- Serverless data processing pipelines
- Cold start optimization strategies

**Container Strategy:**
- Container orchestration platform
- Container image management
- Service mesh implementation
- Ingress and load balancing
- Storage and persistent volume management

**Security Architecture:**
- Cloud security best practices
- Identity and access management
- Network security configurations
- Encryption key management
- Compliance and governance frameworks

Target cloud platform: [Specify AWS, Azure, GCP, or multi-cloud requirements]
```

### 14. Data Architecture Design Prompt

```txt
Design a comprehensive data architecture for [PROJECT_NAME]. Cover:

**Data Strategy:**
- Data governance framework
- Data quality standards
- Master data management
- Data lifecycle management
- Privacy and compliance requirements

**Data Storage Architecture:**
- Operational data stores
- Analytical data warehouses
- Data lake implementations
- Real-time streaming data stores
- Archive and backup strategies

**Data Processing:**
- ETL/ELT pipeline design
- Real-time stream processing
- Batch processing frameworks
- Data transformation strategies
- Data validation and cleansing

**Analytics Architecture:**
- Business intelligence platforms
- Self-service analytics tools
- Machine learning pipeline integration
- Reporting and visualization strategies
- Performance optimization for analytics

**Data Integration:**
- API-based data integration
- Message queue implementations
- Change data capture (CDC)
- Data synchronization strategies
- Third-party data source integration

Data requirements: [Specify data volume, velocity, variety, and analytical needs]
```

---

## Project Planning Prompts

### 15. Project Roadmap & Timeline Prompt

```txt
Create a detailed project roadmap and timeline for [PROJECT_NAME]. Include:

**Project Phases:**
- Discovery and planning phase
- Design and architecture phase
- Development phases (by feature/milestone)
- Testing and quality assurance phases
- Deployment and launch phases
- Post-launch optimization phases

**Milestone Definition:**
- Key deliverables for each milestone
- Success criteria and acceptance criteria
- Dependencies between milestones
- Risk factors and mitigation strategies
- Resource allocation per phase

**Timeline Estimation:**
- Detailed task breakdown
- Effort estimation methodologies
- Buffer time for risk mitigation
- Critical path identification
- Resource leveling considerations

**Delivery Strategy:**
- Agile/Scrum implementation approach
- Sprint planning and duration
- Release planning strategy
- Continuous delivery pipeline
- Stakeholder review and feedback cycles

Project scope: [Define project complexity, team size, and delivery constraints]
```

### 16. Risk Assessment & Mitigation Prompt

```txt
Conduct comprehensive risk assessment for [PROJECT_NAME]. Structure as:

**Technical Risks:**
- Technology adoption risks
- Integration complexity risks
- Performance and scalability risks
- Security vulnerability risks
- Third-party dependency risks

**Business Risks:**
- Market timing and competition risks
- Budget and resource constraints
- Stakeholder alignment risks
- Regulatory and compliance risks
- User adoption and acceptance risks

**Operational Risks:**
- Team capacity and skill gaps
- Knowledge transfer and documentation
- Infrastructure and deployment risks
- Support and maintenance challenges
- Business continuity risks

**Risk Analysis Framework:**
- Risk probability assessment (1-5 scale)
- Risk impact evaluation (1-5 scale)
- Risk priority matrix
- Mitigation strategies for each risk
- Contingency planning approaches

**Monitoring & Response:**
- Early warning indicators
- Risk monitoring procedures
- Escalation protocols
- Regular risk review processes
- Risk communication strategies

Project context: [Provide project complexity, timeline, and organizational constraints]
```

---

## Quality Assurance Prompts

### 17. Testing Strategy Document Prompt

```txt
Develop a comprehensive testing strategy for [PROJECT_NAME]. Include:

**Testing Approach:**
- Testing methodology and frameworks
- Test-driven development (TDD) implementation
- Behavior-driven development (BDD) practices
- Risk-based testing strategies
- Shift-left testing principles

**Test Level Strategy:**
- Unit testing standards and coverage
- Integration testing approaches
- System testing procedures
- User acceptance testing (UAT) plans
- Performance and load testing

**Automation Strategy:**
- Test automation framework selection
- Automated test case development
- Continuous testing pipeline integration
- Test data management automation
- Automated reporting and analysis

**Quality Gates:**
- Code quality metrics and thresholds
- Test coverage requirements
- Performance benchmarks
- Security testing checkpoints
- User experience validation criteria

**Test Environment Strategy:**
- Test environment provisioning
- Data management for testing
- Environment configuration management
- Test execution scheduling
- Results tracking and reporting

Application type: [Specify web, mobile, API, or enterprise application details]
```

### 18. Performance Testing Plan Prompt

```txt
Create a detailed performance testing plan for [PROJECT_NAME]. Cover:

**Performance Requirements:**
- Response time targets for different operations
- Throughput requirements (transactions per second)
- Concurrent user capacity targets
- Resource utilization limits (CPU, memory, storage)
- Scalability requirements and targets

**Testing Scenarios:**
- Load testing scenarios (normal expected load)
- Stress testing scenarios (peak load conditions)
- Volume testing (large data sets)
- Endurance testing (sustained load over time)
- Spike testing (sudden load increases)

**Test Environment:**
- Production-like environment specifications
- Test data requirements and generation
- Network simulation and conditions
- Third-party service simulation
- Monitoring and measurement tools

**Performance Metrics:**
- Application performance indicators
- Infrastructure performance metrics
- User experience measurements
- Business transaction performance
- Error rates and failure conditions

**Analysis & Reporting:**
- Performance baseline establishment
- Bottleneck identification procedures
- Performance trend analysis
- Optimization recommendation framework
- Stakeholder reporting formats

Expected load characteristics: [Specify user volume, transaction patterns, and growth projections]
```

---

## Deployment & Operations Prompts

### 19. DevOps Implementation Guide Prompt

```txt
Create a comprehensive DevOps implementation guide for [PROJECT_NAME]. Include:

**CI/CD Pipeline Design:**
- Source code management strategy
- Build automation processes
- Automated testing integration
- Deployment automation strategies
- Release management procedures

**Infrastructure Automation:**
- Infrastructure as Code (IaC) implementation
- Configuration management tools
- Environment provisioning automation
- Scaling automation strategies
- Disaster recovery automation

**Monitoring & Observability:**
- Application performance monitoring
- Infrastructure monitoring setup
- Log aggregation and analysis
- Distributed tracing implementation
- Alerting and notification systems

**Development Workflow:**
- Git workflow and branching strategies
- Code review and approval processes
- Feature flag management
- Database migration strategies
- Rollback procedures

**Operations Procedures:**
- Incident response procedures
- Change management processes
- Capacity planning methodologies
- Security operations integration
- Documentation and knowledge management

Technology stack: [Specify development technologies, cloud platforms, and tool preferences]
```

### 20. Production Readiness Checklist Prompt

```txt
Generate a comprehensive production readiness checklist for [PROJECT_NAME]. Structure as:

**Application Readiness:**
- [ ] All functional requirements implemented and tested
- [ ] Non-functional requirements validated
- [ ] Security testing completed and vulnerabilities addressed
- [ ] Performance testing completed and benchmarks met
- [ ] Error handling and graceful degradation implemented
- [ ] Logging and monitoring instrumentation in place

**Infrastructure Readiness:**
- [ ] Production environment provisioned and configured
- [ ] Load balancing and auto-scaling configured
- [ ] Database backup and recovery procedures tested
- [ ] SSL certificates installed and validated
- [ ] DNS configuration completed and tested
- [ ] CDN setup and configuration validated

**Security Readiness:**
- [ ] Security scan completed and issues resolved
- [ ] Access controls and permissions configured
- [ ] Secrets management implemented
- [ ] Data encryption at rest and in transit verified
- [ ] Compliance requirements validated
- [ ] Security incident response procedures documented

**Operational Readiness:**
- [ ] Monitoring dashboards and alerts configured
- [ ] Runbooks and operational procedures documented
- [ ] Support team training completed
- [ ] Incident escalation procedures established
- [ ] Backup and disaster recovery tested
- [ ] Change management procedures in place

**Documentation & Training:**
- [ ] User documentation completed and reviewed
- [ ] API documentation published and accessible
- [ ] Operational runbooks created and validated
- [ ] Support team training materials prepared
- [ ] Go-live communication plan executed

Customize based on: [Specify application criticality and operational requirements]
```

---

## Usage Instructions

### How to Use These Prompts

1. **Customize Context**: Replace placeholders like `[PROJECT_NAME]`, `[PROJECT_TYPE]` with your specific project details.

2. **Iterative Refinement**: Use the initial outputs as starting points and refine with follow-up prompts for specific sections.

3. **Stakeholder Alignment**: Share generated documents with stakeholders for feedback and iterate based on their input.

4. **Template Creation**: Use outputs to create standardized templates for your organization.

5. **Quality Assurance**: Always review and validate generated content for accuracy and completeness.

### Best Practices

- Provide as much context as possible when using prompts
- Break down complex documents into sections and generate incrementally
- Validate technical recommendations with your architecture team
- Ensure compliance requirements are properly addressed
- Keep documents updated as project requirements evolve

### Prompt Customization Guidelines

- Add industry-specific requirements where relevant
- Include regulatory compliance needs specific to your domain
- Customize technology stack references to match your organization's standards
- Adjust complexity levels based on project size and team experience
- Include organizational processes and approval workflows

---

*This prompt library is designed to be comprehensive yet flexible. Adapt and extend these prompts based on your specific project needs, organizational requirements, and industry standards.*