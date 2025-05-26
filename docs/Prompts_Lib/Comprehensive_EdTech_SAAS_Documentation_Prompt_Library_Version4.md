# Comprehensive EdTech SaaS Documentation Prompt Library

**Document Version:** 1.2  
**Created Date:** 2025-05-23  
**Last Updated:** 2025-05-23 13:25:43 UTC  
**Created By:** Intelli-SAAS  
**Document Type:** Prompt Library and Documentation Generation Guide

---
    
## Executive Summary

This document provides a comprehensive collection of prompts and templates for generating complete documentation suites for EdTech SaaS platforms. The prompts are organized by document type and complexity level, enabling rapid creation of production-ready documentation for multi-tenant educational platforms.

### Document Types Covered
- **Product Requirements Documents (PRDs)**: Master, feature-specific, and technical PRDs
- **User Flow Specifications**: End-to-end user journey documentation
- **Development Roadmaps**: MVP planning, module sequencing, and implementation guides
- **Testing Frameworks**: Comprehensive testing strategy and implementation
- **Technical Architecture**: Database design, API specifications, security frameworks
- **Security & Compliance**: Regulatory compliance and security documentation
- **Business Intelligence**: Analytics and reporting system documentation

### Prompt Categories
- **Foundation Prompts**: Core architecture and planning documentation
- **Feature Development Prompts**: Specific functionality and module creation
- **Quality Assurance Prompts**: Testing, security, and compliance documentation
- **Implementation Prompts**: Deployment, monitoring, and operational procedures
- **Specialized Prompts**: Analytics, compliance, and advanced features

---

## Foundation Documentation Prompts

### 1. Master Product Requirements Document (PRD) Prompt

**Context**: Create a comprehensive Master PRD for a multi-tenant educational SaaS platform

**Prompt**:
Create a comprehensive Master Product Requirements Document (PRD) for [PLATFORM_NAME], a multi-tenant educational SaaS platform. Include:

**Platform Overview:**
- Product vision and mission statement aligned with educational technology goals
- Target market analysis (B2C individual learners, B2B institutions, B2B2C partners)
- Competitive landscape and differentiation strategy
- Business model and revenue streams (subscription, usage-based, marketplace)

**User Personas and Target Markets:**
- Individual learners (demographics, goals, pain points, usage patterns)
- Educational institutions (administrators, faculty, students)
- EduTech partners and white-label customers
- Secondary personas (parents, corporate trainers, content creators)

**Core Platform Requirements:**
- Multi-tenant architecture with complete data isolation
- Scalability requirements (concurrent users, data volume, geographic distribution)
- Security and compliance (FERPA, GDPR, COPPA, SOC 2)
- Integration capabilities (LTI, SIS, payment processors, communication tools)

**Business Objectives and Success Metrics:**
- Revenue targets and growth projections
- User acquisition and retention goals
- Technical performance benchmarks
- Educational effectiveness measurements

**Product Scope:**
- In-scope features for MVP and future releases
- Out-of-scope items with rationale
- Dependencies and assumptions
- Risk assessment and mitigation strategies

**Technical Architecture Requirements:**
- High-level system architecture
- Performance and scalability requirements
- Security and compliance specifications
- Integration and API requirements

**Go-to-Market Strategy:**
- Launch timeline and phases
- Beta testing and pilot programs
- Marketing and sales approach
- Support and success planning

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: Master Product Requirements Document
- Next Review Date: 2025-06-23
- Review Frequency: Monthly during development, quarterly post-launch

Ensure the PRD addresses [SPECIFIC_MARKET_FOCUS] and supports [TARGET_USER_VOLUME] users with [KEY_DIFFERENTIATOR] as the primary competitive advantage.

### 2. Multi-Tenant Architecture Design Prompt

**Context**: Design secure, scalable multi-tenant database and application architecture

**Prompt**:
Design a comprehensive multi-tenant architecture for [PLATFORM_NAME] educational SaaS platform with the following requirements:

**Database Architecture:**
- Multi-tenant data isolation strategy (row-level security vs. separate schemas vs. separate databases)
- PostgreSQL implementation with Prisma ORM and Supabase
- Tenant context management and routing
- Data security and encryption at rest/in transit
- Backup and disaster recovery for multi-tenant data
- Performance optimization for tenant-specific queries

**Application Architecture:**
- Tenant detection and routing (subdomain, custom domain, URL parameter)
- Authentication and authorization per tenant using Supabase Auth
- Tenant-specific configuration and branding
- Cross-tenant access management for multi-affiliated users
- API design for tenant context propagation

**Security Implementation:**
- Complete tenant data isolation verification
- Role-based access control (RBAC) within tenants
- API security and rate limiting per tenant
- Audit logging and compliance tracking
- Data privacy and GDPR compliance measures

**Scalability Design:**
- Horizontal scaling strategies
- Database sharding and read replicas
- Caching layers (Redis) for tenant-specific data
- CDN integration for global content delivery
- Auto-scaling and load balancing

**Performance Optimization:**
- Query optimization for multi-tenant scenarios
- Connection pooling and resource management
- Tenant-specific performance monitoring
- Index strategies for tenant-partitioned data

**Technical Specifications:**
- Complete Prisma schema with multi-tenant models
- Row-level security (RLS) policies
- Tenant middleware and context injection
- Database migration strategies for multi-tenant
- Testing framework for tenant isolation

**Implementation Examples:**
Include complete Prisma schema with tenant isolation, middleware examples for tenant context, and security policies.

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: Multi-Tenant Architecture Specification
- Next Review Date: 2025-06-23

Platform should support [NUMBER_OF_TENANTS] tenants with [USERS_PER_TENANT] users each, maintaining <100ms query response times and zero cross-tenant data leakage.

### 3. User Experience and Flow Documentation Prompt

**Context**: Create comprehensive end-to-end user flow documentation

**Prompt**:
Create comprehensive end-to-end user flow documentation for [PLATFORM_NAME] educational platform covering:

**User Registration and Onboarding Flows:**
- Multi-tenant user registration with tenant detection
- Progressive onboarding for different user types (students, instructors, administrators)
- Email verification and account activation
- Social authentication integration (Google, Microsoft, Apple)
- Profile completion and preference setting
- Role assignment and permission configuration

**Authentication and Authorization Flows:**
- Multi-factor authentication setup and usage
- Single sign-on (SSO) integration with institutions
- Password reset and account recovery
- Session management and security
- Cross-device authentication and sync

**Core Learning Flows:**
- Course discovery and enrollment
- Learning content consumption (video, documents, interactive content)
- Assessment taking and submission
- Progress tracking and milestone completion
- Certificate generation and sharing
- Discussion participation and collaboration

**Administrative Flows:**
- Tenant setup and configuration
- User management and bulk operations
- Course creation and content management
- Assessment creation and grading
- Analytics and reporting access
- Integration configuration and management

**Error Handling and Edge Cases:**
- Network connectivity issues and offline scenarios
- Payment failures and subscription management
- Content loading failures and retry mechanisms
- Assessment submission errors and recovery
- User permission changes and access updates

**Mobile and Responsive Flows:**
- Mobile-first user experience design
- Progressive Web App (PWA) functionality
- Offline content access and synchronization
- Touch-optimized interactions and navigation
- Cross-device state management

**Accessibility and Compliance Flows:**
- WCAG 2.1 AA compliance throughout all flows
- Screen reader navigation and support
- Keyboard-only navigation paths
- Alternative content formats and accommodations

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: User Flow Specification
- Next Review Date: 2025-06-23

For each flow, include:
- Step-by-step user journey with decision points
- UI/UX wireframes and interaction patterns
- Error states and recovery procedures
- Performance requirements and loading states
- Analytics tracking and success metrics
- Mobile and desktop variations

Focus on [PRIMARY_USER_TYPE] workflows while ensuring seamless experience for [SECONDARY_USER_TYPES] in a [INSTITUTIONAL_TYPE] environment.

---

## Feature Development Prompts

### 4. Learning Management System (LMS) Feature Prompt

**Context**: Develop comprehensive LMS functionality with modern features

**Prompt**:
Develop a comprehensive Learning Management System (LMS) for [PLATFORM_NAME] with the following specifications:

**Course Creation and Management:**
- Drag-and-drop course builder with intuitive interface
- Hierarchical content organization (Courses > Modules > Lessons > Activities)
- Multimedia content support (video, audio, documents, interactive elements)
- Content versioning and revision control
- Collaborative course development with multiple authors
- Template library for common course structures
- Bulk content import and migration tools

**Content Delivery and Interaction:**
- Adaptive video streaming with quality optimization
- Interactive content types (simulations, virtual labs, gamification)
- Real-time collaboration tools (discussions, group projects, peer review)
- Mobile-optimized content consumption
- Offline content download and synchronization
- Progressive content unlocking based on prerequisites

**Assessment and Evaluation:**
- Multiple question types (MCQ, essay, file upload, code submission)
- Automated grading with AI-powered essay evaluation
- Rubric-based manual grading interface
- Anti-cheating measures and proctoring integration
- Adaptive testing with personalized question selection
- Peer assessment and self-evaluation tools

**Progress Tracking and Analytics:**
- Real-time learning progress monitoring
- Competency-based progression tracking
- Learning path optimization and recommendations
- Performance analytics and insights
- Predictive modeling for student success
- Goal setting and achievement tracking

**Mobile Learning Features:**
- Native mobile app with offline capabilities
- Microlearning and bite-sized content delivery
- Push notifications for deadlines and updates
- Camera integration for assignment submission
- Voice input and accessibility features

**Technical Implementation:**
- RESTful API design for all LMS functionality
- GraphQL integration for complex data relationships
- Real-time updates using WebSockets
- CDN integration for global content delivery
- Scalable architecture supporting 100K+ concurrent users

**Integration Capabilities:**
- LTI (Learning Tools Interoperability) compliance
- External tool integration (Zoom, Microsoft Teams, Google Workspace)
- Grade passback to Student Information Systems
- Content provider integrations (YouTube, Khan Academy, etc.)
- Single Sign-On (SSO) with institutional identity providers

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: LMS Feature Specification
- Next Review Date: 2025-06-23

Target performance: <2.5s page load times, 99.9% uptime, support for [CONCURRENT_USERS] simultaneous learners with [CONTENT_VOLUME] GB of educational content.

### 5. AI Personalization Engine Feature Prompt

**Context**: Implement AI-powered personalization for educational content and experiences

**Prompt**:
Design and implement an AI-powered personalization engine for [PLATFORM_NAME] educational platform with:

**Learning Analytics and Profiling:**
- Real-time learning behavior analysis and pattern recognition
- Learning style detection (visual, auditory, kinesthetic, reading/writing)
- Knowledge gap identification through assessment analysis
- Learning pace optimization based on individual progress
- Cognitive load assessment and content difficulty adjustment

**Recommendation Systems:**
- Content-based filtering for course and resource recommendations
- Collaborative filtering based on similar learner preferences
- Hybrid recommendation approach combining multiple algorithms
- Real-time recommendation updates based on user interactions
- Explanation mechanisms for recommendation transparency

**Adaptive Learning Paths:**
- Dynamic learning sequence optimization
- Prerequisite analysis and dependency mapping
- Alternative pathway generation for different learning objectives
- Difficulty progression management and scaffolding
- Remediation and acceleration path adjustments

**Predictive Analytics:**
- Student success probability modeling
- Early warning system for at-risk learners
- Optimal intervention timing and strategy recommendations
- Course completion prediction and timeline estimation
- Performance trend analysis and forecasting

**Intelligent Tutoring Features:**
- AI-powered virtual learning assistant
- Natural language processing for student questions
- Context-aware help and guidance delivery
- Automated hint generation for problem-solving
- Conversational interface for learning support

**Content Intelligence:**
- Automatic content tagging and categorization
- Difficulty level assessment for learning materials
- Learning objective extraction from content
- Content quality scoring and optimization suggestions
- Similarity analysis for content organization

**Technical Architecture:**
- Machine learning pipeline with MLflow integration
- Real-time inference API with <100ms response times
- Feature store for consistent model training and serving
- A/B testing framework for algorithm evaluation
- Model monitoring and drift detection

**Privacy and Ethics:**
- Privacy-preserving machine learning techniques
- Explainable AI for educational decision-making
- Bias detection and mitigation in recommendation algorithms
- User consent management for data usage
- Transparent data usage policies and controls

**Performance Requirements:**
- Real-time personalization for 50K+ concurrent users
- <100ms recommendation response times
- 80%+ recommendation relevance accuracy
- 85%+ student success prediction accuracy
- Privacy-compliant data processing and storage

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: AI Personalization Feature Specification
- Next Review Date: 2025-06-23

Integration with existing LMS functionality and support for [EDUCATIONAL_CONTEXT] with focus on [LEARNING_OUTCOMES] improvement.

### 6. Assessment Engine and Grading System Prompt

**Context**: Create comprehensive assessment and automated grading capabilities

**Prompt**:
Develop a comprehensive assessment engine and grading system for [PLATFORM_NAME] with:

**Assessment Creation and Management:**
- Question bank with categorization and tagging
- Multiple question types: multiple choice, true/false, short answer, essay, file upload, code submission, drag-and-drop, matching
- Rich media integration in questions (images, videos, audio, interactive elements)
- Question randomization and shuffling algorithms
- Adaptive questioning based on response patterns
- Collaborative assessment creation and review workflows

**Automated Grading System:**
- Instant grading for objective question types
- AI-powered essay grading with natural language processing
- Code compilation and automated testing for programming assignments
- Mathematical equation validation and step-by-step analysis
- Plagiarism detection and similarity analysis
- Partial credit assignment with configurable rules

**Manual Grading Interface:**
- Streamlined grading interface for subjective assessments
- Rubric creation and application tools
- Annotation and feedback tools (text, audio, video)
- Batch grading and bulk operations
- Grade moderation and second-reader workflows
- Grade curve and statistical adjustment tools

**Anti-Cheating and Security:**
- Browser lockdown and monitoring during assessments
- Randomized question and answer ordering
- Time-based question release and submission
- Biometric authentication integration
- Suspicious behavior detection and reporting
- Secure assessment delivery and submission

**Assessment Analytics:**
- Item analysis and question effectiveness metrics
- Student performance analytics and insights
- Comparative analysis across cohorts and time periods
- Predictive modeling for assessment outcomes
- Learning objective mastery tracking
- Assessment validity and reliability analysis

**Grade Management:**
- Comprehensive gradebook with flexible categories
- Weighted grading with customizable schemes
- Grade calculation engines with complex formulas
- Grade export and integration with SIS systems
- Transcript generation and official documentation
- Parent/guardian portal access and notifications

**Accessibility and Accommodations:**
- Screen reader compatibility and keyboard navigation
- Extended time and multiple attempt configurations
- Alternative format support (large print, audio)
- Language translation and multilingual support
- Cognitive accessibility features and simplification options

**Technical Implementation:**
- RESTful API for all assessment functionality
- Real-time assessment delivery and submission
- Scalable architecture supporting 10K+ concurrent test-takers
- Secure data encryption and storage
- Integration with learning analytics platforms

**Performance and Reliability:**
- <500ms response time for assessment loading
- 99.9% uptime during assessment periods
- Automatic backup and recovery for submissions
- Load balancing for peak assessment periods
- Real-time progress tracking and synchronization

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: Assessment Engine Specification
- Next Review Date: 2025-06-23

Support for [ASSESSMENT_TYPES] in [EDUCATIONAL_CONTEXT] with compliance to [ACCESSIBILITY_STANDARDS] and integration with [EXISTING_SYSTEMS].

---

## Technical Implementation Prompts

### 7. Database Schema and API Design Prompt

**Context**: Create comprehensive database schema and API specifications

**Prompt**:
Design a comprehensive database schema and API specification for [PLATFORM_NAME] educational platform:

**Database Schema Design:**
- Complete Prisma schema for multi-tenant educational platform
- User management (users, roles, permissions, tenant associations)
- Course and content management (courses, modules, lessons, assessments)
- Enrollment and progress tracking (enrollments, progress, completions, grades)
- Assessment and grading (questions, responses, grades, feedback)
- Communication and collaboration (discussions, messages, notifications)
- Analytics and reporting (events, metrics, reports)
- Payment and subscription management (subscriptions, transactions, billing)

**Multi-Tenant Implementation:**
- Row-level security (RLS) policies for tenant isolation
- Tenant context propagation through all queries
- Cross-tenant relationship management
- Tenant-specific configuration and settings
- Data migration and tenant provisioning scripts

**API Design and Implementation:**
- RESTful API design following OpenAPI 3.0 specification
- GraphQL schema for complex data relationships
- Authentication and authorization middleware
- Rate limiting and throttling per tenant
- Input validation and sanitization
- Error handling and response standardization

**Key API Endpoints:**
Authentication:
- POST /auth/login - User authentication
- POST /auth/refresh - Token refresh
- POST /auth/logout - Session termination
- GET /auth/me - Current user profile

Course Management:
- GET /courses - List courses with filtering and pagination
- POST /courses - Create new course
- GET /courses/{id} - Get course details
- PUT /courses/{id} - Update course
- DELETE /courses/{id} - Delete course

Content Management:
- GET /courses/{id}/content - Get course content structure
- POST /content/upload - Upload content files
- GET /content/{id}/stream - Stream video content
- PUT /content/{id} - Update content metadata

Assessment:
- GET /assessments/{id} - Get assessment details
- POST /assessments/submit - Submit assessment responses
- GET /assessments/{id}/results - Get assessment results
- POST /assessments/{id}/grade - Manual grading

User Management:
- GET /users - List users (admin only)
- POST /users - Create user account
- GET /users/{id} - Get user profile
- PUT /users/{id} - Update user profile

Analytics:
- GET /analytics/dashboard - Get dashboard metrics
- GET /analytics/reports - Generate custom reports
- GET /analytics/events - Track learning events
- POST /analytics/events - Log user interactions

**Security Specifications:**
- JWT-based authentication with refresh tokens
- OAuth 2.0 integration for social login
- API key management for external integrations
- CORS configuration for cross-origin requests
- SQL injection prevention and input validation
- Rate limiting per user and tenant

**Performance Optimization:**
- Database indexing strategy for common queries
- Query optimization for multi-tenant scenarios
- Caching layers with Redis for frequently accessed data
- Connection pooling and resource management
- API response optimization and compression

**Documentation Requirements:**
- Complete OpenAPI specification with examples
- GraphQL schema documentation
- Database relationship diagrams
- API integration guides and SDK documentation
- Testing scenarios and example requests/responses

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: Database and API Specification
- Next Review Date: 2025-06-23

Target performance: <200ms API response time (95th percentile), support for 10K+ requests/minute, 99.9% availability.

Platform specifics: [TENANT_COUNT] tenants, [USER_COUNT] users, [CONTENT_VOLUME] content storage, [GEOGRAPHIC_REGIONS] deployment regions.

### 8. Security and Compliance Framework Prompt

**Context**: Implement comprehensive security and regulatory compliance

**Prompt**:
Design a comprehensive security and compliance framework for [PLATFORM_NAME] educational platform:

**Security Architecture:**
- Zero-trust security model implementation
- Multi-layered defense strategy (network, application, data)
- Threat modeling and risk assessment framework
- Security monitoring and incident response procedures
- Vulnerability management and patch processes

**Authentication and Authorization:**
- Multi-factor authentication (MFA) with multiple methods
- Single Sign-On (SSO) integration with SAML 2.0 and OAuth 2.0
- Role-based access control (RBAC) with fine-grained permissions
- Session management and security controls
- API authentication and authorization framework

**Data Protection and Privacy:**
- Encryption at rest using AES-256
- Encryption in transit using TLS 1.3
- Data classification and handling procedures
- Privacy by design implementation
- Data retention and deletion policies

**Regulatory Compliance:**
FERPA (Family Educational Rights and Privacy Act):
- Educational record protection and access controls
- Directory information management
- Consent mechanisms for data disclosure
- Audit trails for record access
- Parent and student rights implementation

GDPR (General Data Protection Regulation):
- Data protection impact assessments (DPIA)
- Consent management and documentation
- Data subject rights implementation (access, rectification, erasure)
- Data transfer and adequacy decisions
- Breach notification procedures

COPPA (Children's Online Privacy Protection Act):
- Age verification mechanisms
- Parental consent workflows
- Limited data collection for children under 13
- Safe harbor compliance procedures
- Regular compliance audits

**Application Security:**
- OWASP Top 10 vulnerability prevention
- Input validation and output encoding
- SQL injection and XSS prevention
- CSRF protection and secure headers
- File upload security and validation

**API Security:**
- Rate limiting and DDoS protection
- API gateway with security controls
- Request/response validation
- API key management and rotation
- Webhook security and verification

**Infrastructure Security:**
- Network segmentation and firewalls
- Container security and image scanning
- Infrastructure as Code (IaC) security
- Cloud security configuration and monitoring
- Backup encryption and disaster recovery

**Security Monitoring and Incident Response:**
- Security Information and Event Management (SIEM)
- Real-time threat detection and alerting
- Incident response playbooks and procedures
- Forensic analysis capabilities
- Security metrics and reporting

**Testing and Validation:**
- Automated security testing in CI/CD pipeline
- Penetration testing and vulnerability assessments
- Code security analysis (SAST/DAST)
- Compliance auditing and validation
- Security awareness training and procedures

**Multi-Tenant Security:**
- Complete tenant data isolation
- Cross-tenant access prevention
- Tenant-specific security configurations
- Shared vs. isolated security controls
- Tenant security monitoring and reporting

**Business Continuity:**
- Disaster recovery planning and testing
- Business continuity procedures
- Data backup and restoration
- High availability and failover
- Emergency response and communication

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: Security and Compliance Framework
- Next Review Date: 2025-06-23

Implementation for [PLATFORM_TYPE] serving [USER_DEMOGRAPHICS] with [DATA_SENSITIVITY_LEVEL] data protection requirements and [COMPLIANCE_JURISDICTIONS] regulatory coverage.

---

## Quality Assurance and Testing Prompts

### 9. Comprehensive Testing Strategy Prompt

**Context**: Develop complete testing framework covering all testing types

**Prompt**:
Create a comprehensive testing strategy and framework for [PLATFORM_NAME] educational platform:

**Testing Strategy Overview:**
- Testing pyramid approach (70% unit, 20% integration, 10% e2e)
- Shift-left testing methodology
- Continuous testing integration in CI/CD pipeline
- Risk-based testing prioritization
- Test automation framework and tools

**Unit Testing Framework:**
- Frontend: Jest + React Testing Library for component testing
- Backend: Jest + Supertest for API testing
- Database: Test containers with isolated test databases
- Coverage requirements: 90%+ code coverage
- Mock strategies for external dependencies
- Test data management and fixtures

**Integration Testing:**
- API endpoint testing with real database connections
- Service integration testing
- Third-party integration validation
- Multi-tenant isolation testing
- Database transaction and rollback testing

**End-to-End Testing:**
- Playwright for cross-browser automation
- Complete user journey validation
- Multi-tenant scenario testing
- Mobile and responsive design testing
- Accessibility testing integration

**Performance Testing:**
- Load testing with K6 or Artillery
- Stress testing for capacity limits
- Volume testing with large datasets
- Endurance testing for stability
- Scalability testing for concurrent users

**Performance Test Scenarios:**
- Normal load: [NORMAL_USER_COUNT] concurrent users
- Peak load: [PEAK_USER_COUNT] concurrent users
- Stress load: [STRESS_USER_COUNT] concurrent users
- Spike testing: sudden traffic increases
- Capacity testing: maximum system limits

**Security Testing:**
- Automated vulnerability scanning (OWASP ZAP)
- Penetration testing procedures
- Authentication and authorization testing
- SQL injection and XSS testing
- Multi-tenant security isolation validation

**Accessibility Testing:**
- WCAG 2.1 AA compliance validation
- Screen reader compatibility testing
- Keyboard navigation testing
- Color contrast and visual accessibility
- Mobile accessibility testing

**Multi-Tenant Testing:**
- Tenant data isolation verification
- Cross-tenant access prevention testing
- Tenant-specific configuration testing
- Performance testing per tenant
- Security testing across tenant boundaries

**Test Data Management:**
- Synthetic test data generation
- Production data anonymization
- Test environment data refresh
- Data privacy in testing environments
- Test data lifecycle management

**Continuous Testing Pipeline:**
- Automated test execution on code changes
- Quality gates and deployment blockers
- Test result reporting and dashboards
- Failure analysis and debugging
- Test maintenance and optimization

**Test Environment Strategy:**
- Development environment testing
- Staging environment validation
- Production monitoring and synthetic testing
- Environment provisioning and management
- Data synchronization between environments

**Monitoring and Observability Testing:**
- Application performance monitoring validation
- Error tracking and alerting testing
- Business metrics and analytics testing
- Infrastructure monitoring validation
- Compliance and audit testing

**Test Reporting and Analytics:**
- Comprehensive test result dashboards
- Test trend analysis and metrics
- Quality metrics and KPI tracking
- Test automation ROI analysis
- Stakeholder reporting and communication

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: Comprehensive Testing Strategy
- Next Review Date: 2025-06-23

Target test coverage: 90%+ unit tests, 100% critical path e2e tests, performance validation for [CONCURRENT_USERS] users, security testing covering OWASP Top 10, accessibility compliance to WCAG 2.1 AA.

Platform specifics: [PLATFORM_SCALE], [TECHNOLOGY_STACK], [DEPLOYMENT_MODEL], [COMPLIANCE_REQUIREMENTS].

### 10. Load Testing and Performance Validation Prompt

**Context**: Implement comprehensive load testing and performance monitoring

**Prompt**:
Design comprehensive load testing and performance validation for [PLATFORM_NAME]:

**Load Testing Architecture:**
- K6-based load testing framework
- Distributed load generation across multiple regions
- Realistic user behavior simulation
- Scalable test data management
- Real-time performance monitoring during tests

**Performance Test Scenarios:**

Baseline Performance Testing:
- Single user performance baseline
- API response time validation (<200ms p95)
- Database query performance (<100ms p95)
- Page load time validation (<2.5s p95)
- Resource utilization monitoring

Concurrent User Testing:
- 1,000 concurrent users (normal load)
- 10,000 concurrent users (peak load)
- 50,000 concurrent users (capacity test)
- 100,000 concurrent users (stress test)
- Gradual ramp-up and sustained load testing

Multi-Tenant Load Testing:
- Simultaneous multi-tenant load simulation
- Tenant isolation performance validation
- Cross-tenant performance impact analysis
- Tenant-specific load pattern simulation
- Resource allocation and scaling per tenant

Educational Workflow Testing:
- Course enrollment spikes (start of semester)
- Assessment submission bursts (exam periods)
- Content streaming load (popular lecture times)
- Real-time collaboration load (group activities)
- Mobile app usage patterns and performance

Database Performance Testing:
- Query performance under load
- Connection pool utilization and limits
- Read replica effectiveness
- Database scaling and sharding validation
- Backup and recovery performance impact

**K6 Test Implementation:**
Include complete K6 test scripts with scenarios for:
- Normal load testing with realistic user patterns
- Peak load simulation with concurrent users
- Stress testing for capacity limits
- Multi-tenant isolation performance validation
- Educational workflow specific testing

**Performance Monitoring:**
- Real-time application performance monitoring (APM)
- Infrastructure metrics collection
- Database performance monitoring
- User experience monitoring
- Business transaction monitoring

**Capacity Planning:**
- Resource requirement analysis
- Auto-scaling configuration validation
- Cost optimization analysis
- Performance bottleneck identification
- Capacity forecasting and planning

**Performance Test Data:**
- Realistic user data sets
- Course and content data volumes
- Assessment and submission data
- Multi-tenant data distribution
- Historical data for trend analysis

**Test Environment Setup:**
- Production-like infrastructure
- Load balancer configuration
- Database scaling setup
- CDN and caching configuration
- Monitoring and observability tools

**Performance Validation Criteria:**
- Response time: <2 seconds (p95) for page loads
- API performance: <200ms (p95) for API calls
- Throughput: 10,000+ requests per minute
- Error rate: <0.1% for all requests
- Availability: 99.9% uptime during load tests

**Scalability Testing:**
- Horizontal scaling validation
- Auto-scaling trigger testing
- Load balancer effectiveness
- Database scaling performance
- Global CDN performance validation

**Regression Testing:**
- Performance baseline comparison
- Automated performance regression detection
- Performance trend analysis
- Performance impact of new features
- Continuous performance monitoring

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: Load Testing and Performance Validation
- Next Review Date: 2025-06-23

Target platform capacity: [MAX_CONCURRENT_USERS] concurrent users, [REQUESTS_PER_SECOND] req/s throughput, [DATA_VOLUME] content storage, deployed across [GEOGRAPHIC_REGIONS] with [AVAILABILITY_REQUIREMENT] uptime SLA.

---

## Implementation and Deployment Prompts

### 11. MVP Development Roadmap Prompt

**Context**: Create detailed MVP development plan with module sequencing

**Prompt**:
Create a comprehensive MVP development roadmap for [PLATFORM_NAME] educational platform:

**MVP Scope Definition:**
- Core functionality for immediate user value
- Technical foundation for future scalability
- Minimum viable features for [TARGET_USER_TYPE]
- Quality standards and performance requirements
- Go-to-market readiness criteria

**Development Phases and Timeline:**

Phase 1: Foundation Infrastructure (Weeks 1-4)
- Multi-tenant database architecture with Prisma + PostgreSQL
- Authentication and authorization system
- Basic API framework with security
- Frontend foundation with React/Next.js
- CI/CD pipeline setup

Phase 2: Core Learning Platform (Weeks 5-10)
- Course creation and management system
- Content delivery and streaming
- Assessment engine and grading
- User enrollment and progress tracking
- Basic analytics and reporting

Phase 3: User Experience and Features (Weeks 11-14)
- Student and instructor dashboards
- Mobile-responsive interface
- Communication and collaboration tools
- Payment integration and subscription management
- Basic AI personalization features

Phase 4: Quality and Deployment (Weeks 15-16)
- Comprehensive testing and QA
- Security audit and penetration testing
- Performance optimization and load testing
- Production deployment and monitoring
- Documentation and training materials

**Module Development Sequence:**

Week 1-2: Database and Authentication
Module 1: Multi-tenant database schema
- Prisma schema design with tenant isolation
- Row-level security implementation
- User and role management tables
- Authentication system with JWT
- Basic API security framework

Week 3-4: Frontend Foundation and API Framework
Module 2: Frontend architecture and design system
- Next.js setup with TypeScript
- Tailwind CSS and component library
- Multi-tenant UI framework
- API integration and state management

Module 3: Core API and middleware
- RESTful API design and implementation
- Authentication and authorization middleware
- Input validation and error handling
- Rate limiting and security controls

Week 5-7: Learning Management System
Module 4: Course management system
- Course creation and editing interface
- Content upload and organization
- Multimedia content support
- Course publishing and enrollment

Module 5: Assessment engine
- Question creation and management
- Assessment delivery and submission
- Automated grading system
- Grade book and reporting

Week 8-10: User Experience
Module 6: Student learning interface
- Course consumption and navigation
- Progress tracking and bookmarks
- Discussion forums and collaboration
- Mobile-optimized experience

Module 7: Instructor dashboard
- Course management and analytics
- Student progress monitoring
- Grading and feedback tools
- Communication and announcements

Week 11-12: Administrative Features
Module 8: Admin dashboard and tenant management
- Tenant configuration and branding
- User management and bulk operations
- System analytics and monitoring
- Integration settings and configuration

Week 13-14: Integration and AI
Module 9: Third-party integrations
- Payment processing (Stripe/PayPal)
- Email services and notifications
- Video conferencing integration
- Learning analytics platforms

Module 10: Basic AI personalization
- Content recommendation engine
- Learning path optimization
- Basic predictive analytics
- Student success indicators

Week 15-16: Testing and Deployment
Module 11: Comprehensive testing
- Unit and integration testing
- Load and performance testing
- Security and accessibility testing
- User acceptance testing

Module 12: Production deployment
- Infrastructure setup and deployment
- Monitoring and alerting configuration
- Backup and disaster recovery
- Documentation and training

**Development Standards:**
- Test-driven development (TDD) approach
- 90%+ code coverage requirement
- Security-first development practices
- Performance optimization from start
- Accessibility compliance (WCAG 2.1 AA)

**Quality Gates:**
- Code review and approval process
- Automated testing and quality checks
- Security scanning and vulnerability assessment
- Performance benchmarking and validation
- User acceptance testing and feedback

**Success Criteria:**
Technical:
- 99.9% uptime and reliability
- <2 second page load times
- Support for [MVP_USER_COUNT] concurrent users
- Zero critical security vulnerabilities
- Full accessibility compliance

Business:
- [PILOT_USER_COUNT] beta users successfully onboarded
- >80% user satisfaction scores
- >60% course completion rates
- Scalable architecture for 10x growth
- Production-ready platform launch

**Risk Mitigation:**
- Regular stakeholder reviews and feedback
- Iterative development with user testing
- Technical debt management
- Security and compliance validation
- Performance monitoring and optimization

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: MVP Development Roadmap
- Next Review Date: 2025-06-23

Target MVP delivery: [LAUNCH_DATE] with support for [TARGET_MARKET] serving [USER_COUNT] users across [TENANT_COUNT] institutions.

### 12. CI/CD and Deployment Infrastructure Prompt

**Context**: Setup complete DevOps pipeline and production infrastructure

**Prompt**:
Design comprehensive CI/CD pipeline and deployment infrastructure for [PLATFORM_NAME]:

**CI/CD Pipeline Architecture:**
- GitHub Actions for continuous integration
- Multi-environment deployment pipeline (dev, staging, production)
- Infrastructure as Code (IaC) with Terraform
- Container-based deployment with Docker and Kubernetes
- Automated testing and quality gates

**Development Workflow:**
Include complete GitHub Actions workflow configuration with:
- Code quality and linting checks
- Unit and integration testing
- Security scanning and vulnerability assessment
- Performance testing and benchmarking
- Accessibility testing and compliance
- Docker image building and optimization
- Container security scanning
- Infrastructure provisioning with Terraform
- Kubernetes deployment with Helm
- Database migration and seeding
- Health checks and smoke testing
- Monitoring and alerting configuration

**Infrastructure Architecture:**
Cloud Platform: [AWS/Azure/GCP]
- Multi-region deployment for high availability
- Auto-scaling groups for application servers
- Managed database services (RDS/Azure Database/Cloud SQL)
- Content delivery network (CDN) for global performance
- Load balancers with SSL termination

**Container Orchestration:**
Kubernetes Cluster Setup:
- Multi-node cluster with high availability
- Namespace isolation for different environments
- Horizontal Pod Autoscaler (HPA) configuration
- Ingress controllers with SSL certificates
- Persistent storage for stateful applications

**Database Infrastructure:**
- Primary-replica setup for read/write splitting
- Automated backups with point-in-time recovery
- Connection pooling with PgBouncer
- Database monitoring and performance tuning
- Multi-tenant data isolation and security

**Monitoring and Observability:**
Application Monitoring:
- APM with DataDog/New Relic for performance monitoring
- Error tracking with Sentry for exception handling
- Log aggregation with ELK stack or CloudWatch
- Custom metrics and business intelligence dashboards
- Real-time alerting and notification systems

Infrastructure Monitoring:
- Server metrics and resource utilization
- Network performance and connectivity
- Database performance and query analysis
- Storage usage and capacity planning
- Security events and compliance monitoring

**Security and Compliance:**
- Vulnerability scanning in CI/CD pipeline
- Container image security analysis
- Infrastructure security hardening
- SSL/TLS certificate management
- Secrets management with HashiCorp Vault or cloud services

**Deployment Strategies:**
- Blue-green deployment for zero downtime
- Canary releases for gradual rollout
- Feature flags for controlled feature deployment
- Rollback procedures and disaster recovery
- Environment promotion and testing

**Performance Optimization:**
- CDN configuration for global content delivery
- Database query optimization and indexing
- Application caching with Redis
- Image and asset optimization
- Gzip compression and minification

**Backup and Disaster Recovery:**
- Automated database backups with encryption
- Application data backup and versioning
- Cross-region backup replication
- Disaster recovery testing and procedures
- Recovery time objective (RTO) and recovery point objective (RPO)

**Environment Management:**
Development Environment:
- Local development with Docker Compose
- Feature branch deployment for testing
- Shared development database and services
- Hot reloading and debugging capabilities

Staging Environment:
- Production-like infrastructure setup
- Integration testing and user acceptance testing
- Performance testing and load validation
- Security testing and vulnerability assessment

Production Environment:
- High-availability multi-region deployment
- Auto-scaling and load balancing
- Comprehensive monitoring and alerting
- Backup and disaster recovery procedures

**Configuration Management:**
- Environment-specific configuration files
- Secrets management and encryption
- Feature flag configuration and management
- Multi-tenant configuration and customization
- Configuration validation and testing

**Quality Gates and Approvals:**
- Automated testing pass requirements
- Security scan approval and vulnerability remediation
- Performance benchmark validation
- Code review and approval process
- Stakeholder sign-off for production deployment

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: CI/CD and Deployment Infrastructure
- Next Review Date: 2025-06-23

Target infrastructure capacity: [CONCURRENT_USERS] users, [REQUESTS_PER_SECOND] req/s, [DATA_STORAGE] storage, 99.9% uptime SLA across [GEOGRAPHIC_REGIONS].

---

## Specialized Documentation Prompts

### 13. Accessibility and Compliance Documentation Prompt

**Context**: Create comprehensive accessibility and regulatory compliance documentation

**Prompt**:
Create comprehensive accessibility and compliance documentation for [PLATFORM_NAME]:

**Accessibility Compliance Framework:**
WCAG 2.1 AA Compliance Implementation:
- Perceivable: Alternative text, captions, color contrast, resizable text
- Operable: Keyboard navigation, seizure prevention, navigation assistance
- Understandable: Readable content, predictable functionality, input assistance
- Robust: Compatible with assistive technologies, future-proof implementation

**Technical Accessibility Implementation:**
Frontend Accessibility:
- Semantic HTML structure and proper heading hierarchy
- ARIA labels, roles, and properties for dynamic content
- Keyboard navigation and focus management
- Screen reader compatibility and testing
- Color contrast ratios meeting AA standards (4.5:1 normal, 3:1 large text)

Interactive Elements:
- Accessible form design with proper labeling
- Error messaging and validation feedback
- Modal and dialog accessibility
- Dynamic content updates and live regions
- Skip links and navigation shortcuts

**Educational Content Accessibility:**
- Video captions and audio descriptions
- Transcript availability for all audio/video content
- Alternative formats for documents (audio, braille, large print)
- Simplified language options and reading level indicators
- Visual content descriptions and alt text

**Assistive Technology Support:**
- Screen reader compatibility (NVDA, JAWS, VoiceOver)
- Voice recognition software integration
- Switch navigation and alternative input methods
- Eye-tracking and head-tracking device support
- Magnification and zoom accessibility

**Regulatory Compliance Framework:**

FERPA (Family Educational Rights and Privacy Act):
Implementation Requirements:
- Educational record classification and protection
- Directory information policies and opt-out procedures
- Consent mechanisms for data disclosure
- Access rights for students and parents
- Audit trails for record access and modifications

Technical Implementation:
- Role-based access control for educational records
- Consent management system with granular controls
- Audit logging for all record access and changes
- Data retention policies and automated deletion
- Secure data transmission and storage

GDPR (General Data Protection Regulation):
Data Protection Measures:
- Privacy by design and by default implementation
- Data protection impact assessments (DPIA)
- Consent management with withdrawal capabilities
- Data subject rights automation (access, rectification, erasure)
- Data portability and export functionality

Technical Implementation:
- Consent capture and documentation system
- User data dashboard and self-service controls
- Automated data deletion and retention management
- Cross-border data transfer safeguards
- Breach detection and notification procedures

COPPA (Children's Online Privacy Protection Act):
Child Protection Framework:
- Age verification mechanisms and processes
- Parental consent workflows and documentation
- Limited data collection for users under 13
- Parental control and monitoring capabilities
- Safe harbor compliance procedures

**Accessibility Testing Framework:**
Automated Testing:
- axe-core integration for continuous accessibility testing
- Color contrast validation tools
- Keyboard navigation testing automation
- Screen reader compatibility testing
- Performance impact assessment of accessibility features

Manual Testing:
- User testing with individuals with disabilities
- Screen reader navigation testing
- Keyboard-only navigation validation
- Voice control and alternative input testing
- Cognitive accessibility and usability testing

**Compliance Monitoring and Reporting:**
Accessibility Monitoring:
- Regular accessibility audits and assessments
- User feedback collection and response procedures
- Accessibility issue tracking and resolution
- Training and awareness programs for development teams
- Accessibility statement and policy documentation

Compliance Reporting:
- Automated compliance dashboard and metrics
- Regular compliance audit and certification
- Incident reporting and response procedures
- Training and awareness documentation
- Legal review and approval processes

**Documentation and Training:**
User Documentation:
- Accessibility feature guides and tutorials
- Assistive technology setup and configuration
- Alternative access methods and workarounds
- Support contact information and procedures
- Regular updates and improvement communications

Developer Documentation:
- Accessibility coding standards and guidelines
- Testing procedures and validation checklists
- Compliance requirements and implementation guides
- Common accessibility patterns and components
- Accessibility review and approval processes

**Accessibility Features:**
Content Accessibility:
- Text-to-speech and audio narration
- Visual content descriptions and summaries
- Simplified and easy-read content versions
- Multilingual support and translation
- Customizable reading and display preferences

Interface Accessibility:
- High contrast and dark mode themes
- Font size and spacing customization
- Keyboard shortcuts and command interfaces
- Voice command and dictation support
- Gesture-based navigation for touch devices

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: Accessibility and Compliance Documentation
- Next Review Date: 2025-06-23

Target compliance: WCAG 2.1 AA certification, FERPA compliance for educational records, GDPR compliance for EU users, COPPA compliance for users under 13, ADA Section 508 compliance for government access.

### 14. Analytics and Business Intelligence Prompt

**Context**: Implement comprehensive analytics and business intelligence system

**Prompt**:
Design comprehensive analytics and business intelligence system for [PLATFORM_NAME]:

**Learning Analytics Framework:**
Student Learning Analytics:
- Real-time learning progress tracking and visualization
- Engagement pattern analysis (time-on-task, session duration, interaction frequency)
- Learning path effectiveness and optimization recommendations
- Competency mastery tracking and skill development visualization
- Predictive modeling for course completion and success probability

Course and Content Analytics:
- Content consumption patterns and popular resource identification
- Assessment performance analysis and question effectiveness
- Course completion rates and dropout point analysis
- Content difficulty assessment and optimization recommendations
- Resource utilization and engagement metrics

Instructor Analytics:
- Teaching effectiveness metrics and student feedback analysis
- Course design optimization recommendations
- Student interaction and engagement monitoring
- Grading patterns and assessment quality analysis
- Professional development insights and recommendations

**Business Intelligence Dashboard:**
Executive Dashboard:
- Key performance indicators (KPIs) and business metrics
- Revenue analytics and subscription performance
- User acquisition, activation, and retention metrics
- Geographic distribution and market penetration analysis
- Competitive benchmarking and market position

Operational Dashboard:
- Platform performance and usage statistics
- Support ticket analysis and resolution metrics
- Feature adoption and user behavior patterns
- Resource utilization and cost optimization insights
- Quality metrics and user satisfaction scores

Financial Dashboard:
- Revenue tracking and forecasting
- Customer lifetime value (CLV) and acquisition cost (CAC) analysis
- Subscription and billing analytics
- Cost per acquisition and retention analysis
- Profitability analysis by customer segment

**Data Collection and Processing:**
Event Tracking System:
- User interaction tracking (clicks, navigation, time spent)
- Learning activity logging (video watching, document reading, quiz taking)
- Assessment and grading event capture
- Communication and collaboration activity tracking
- System performance and error event logging

Data Pipeline Architecture:
- Real-time data ingestion with Kafka or cloud streaming services
- Data transformation and enrichment with Apache Spark or cloud functions
- Data warehouse implementation with Snowflake/BigQuery/Redshift
- Data lake storage for raw and processed data
- ETL/ELT processes for data consistency and quality

**Advanced Analytics Implementation:**
Machine Learning Models:
- Student success prediction models
- Content recommendation algorithms
- Engagement scoring and risk assessment
- Anomaly detection for unusual behavior patterns
- Natural language processing for feedback analysis

Predictive Analytics:
- Enrollment forecasting and capacity planning
- Churn prediction and retention modeling
- Performance trend analysis and early warning systems
- Resource demand forecasting
- Market opportunity identification

**Privacy-Preserving Analytics:**
Data Privacy Controls:
- Differential privacy implementation for sensitive metrics
- Data anonymization and pseudonymization techniques
- Consent-based analytics with opt-out capabilities
- GDPR-compliant data processing and retention
- Audit trails for data access and usage

**Reporting and Visualization:**
Interactive Dashboards:
- Real-time dashboard updates with live data streaming
- Customizable visualizations and metric selection
- Drill-down capabilities for detailed analysis
- Mobile-responsive dashboard design
- Export and sharing capabilities

Automated Reporting:
- Scheduled report generation and distribution
- Custom report builder with drag-and-drop interface
- White-label reporting for institutional clients
- API access for external analytics tools
- Alert-based reporting for threshold breaches

**Data Governance and Quality:**
Data Quality Framework:
- Data validation and consistency checking
- Data lineage tracking and documentation
- Data freshness and timeliness monitoring
- Error detection and correction procedures
- Data quality metrics and reporting

Governance Structure:
- Data access controls and permission management
- Data classification and sensitivity labeling
- Retention policies and archival procedures
- Compliance monitoring and audit trails
- Data stewardship and ownership assignment

**Integration and APIs:**
Analytics API:
- RESTful API for data access and querying
- GraphQL support for complex data relationships
- Real-time data streaming APIs
- Webhook integration for external systems
- SDK development for popular platforms

Third-Party Integrations:
- Google Analytics and marketing tools
- Customer relationship management (CRM) systems
- Learning analytics standards (xAPI, Caliper)
- Business intelligence tools (Tableau, Power BI)
- Student information systems (SIS) integration

**Performance and Scalability:**
Analytics Performance:
- Sub-second query response times for dashboards
- Real-time data processing and streaming
- Horizontal scaling for large data volumes
- Caching strategies for frequently accessed metrics
- Query optimization and indexing strategies

Data Storage Optimization:
- Partitioning strategies for time-series data
- Compression and archival for historical data
- Data lifecycle management and cost optimization
- Backup and disaster recovery for analytics data
- Global data distribution for performance

**Document Control:**
- Document Version: 1.0
- Created Date: 2025-05-23 13:25:43 UTC
- Created By: Intelli-SAAS
- Document Type: Analytics and Business Intelligence System
- Next Review Date: 2025-06-23

Target analytics capability: Real-time processing of [EVENTS_PER_SECOND] events, [DATA_VOLUME] data storage, <2 second dashboard load times, support for [CONCURRENT_ANALYSTS] simultaneous users, [RETENTION_PERIOD] data retention with compliance.

---

## Prompt Usage Guidelines and Best Practices

### How to Use These Prompts Effectively

#### 1. Customization Variables
Replace bracketed variables with your specific requirements:
- `[PLATFORM_NAME]` - Your platform's name
- `[TARGET_USER_COUNT]` - Expected number of users
- `[COMPLIANCE_REQUIREMENTS]` - Specific regulatory needs
- `[TECHNOLOGY_STACK]` - Your chosen technologies
- `[DEPLOYMENT_MODEL]` - Cloud provider and architecture

#### 2. Prompt Combination Strategies
**Sequential Approach**: Use prompts in order for comprehensive documentation
1. Start with Master PRD prompt
2. Follow with technical architecture prompts
3. Add feature-specific prompts as needed
4. Complete with testing and deployment prompts

**Modular Approach**: Use individual prompts for specific documentation needs
- Pick relevant prompts for your current development phase
- Combine multiple feature prompts for comprehensive functionality
- Use testing prompts during QA phases

#### 3. Context Enhancement
**Before using any prompt, provide additional context:**
```
Project Context:
- Platform Type: [Educational/Corporate Training/K-12/Higher Ed]
- Target Market: [Geographic regions, user demographics]
- Scale Requirements: [User count, data volume, performance needs]
- Compliance Needs: [FERPA, GDPR, COPPA, industry-specific]
- Technology Preferences: [Cloud provider, frameworks, databases]
- Timeline: [MVP timeline, full launch date]
- Budget Constraints: [Development resources, infrastructure budget]
```

#### 4. Quality Validation
**After generating documentation, validate:**
- Completeness: All required sections covered
- Consistency: Terminology and requirements alignment
- Feasibility: Technical requirements are achievable
- Compliance: Regulatory requirements properly addressed
- Scalability: Architecture supports growth requirements

### Advanced Prompt Techniques

#### Multi-Stage Prompting
```
Stage 1: "Generate initial [document type] focusing on [specific aspect]"
Stage 2: "Enhance the [document] by adding [additional requirements]"
Stage 3: "Validate the [document] against [criteria] and suggest improvements"
```

#### Role-Based Prompting
```
"As a [Product Manager/Technical Architect/Security Expert/UX Designer], 
review and enhance this [document type] for [specific concerns/requirements]"
```

#### Constraint-Based Prompting
```
"Create [document type] with the following constraints:
- Budget: [amount]
- Timeline: [duration]
- Team size: [number]
- Technology stack: [specific technologies]
- Performance requirements: [specific metrics]"
```

---

## Document Templates and Outputs

### Expected Document Structure
Each prompt should generate documents with:

#### Standard Header
```
Document Title: [Generated based on prompt]
Document Version: 1.0
Created Date: 2025-05-23 13:25:43 UTC
Created By: Intelli-SAAS
Document Type: [PRD/Technical Spec/Test Plan/etc.]
```

#### Content Sections
- Executive Summary
- Detailed Requirements/Specifications
- Implementation Guidelines
- Success Criteria
- Dependencies and Assumptions
- Risk Assessment
- Timeline and Milestones

#### Standard Footer
```
Document Control:
- Next Review Date: 2025-06-23
- Review Frequency: [Schedule]
- Change Management: [Process]
- Distribution: [Stakeholders]
```

### Quality Checklist
Before finalizing any generated document:

**Content Quality**
- [ ] All requirements clearly defined
- [ ] Technical specifications are detailed
- [ ] Success criteria are measurable
- [ ] Dependencies are identified
- [ ] Risks are assessed and mitigated

**Technical Accuracy**
- [ ] Architecture is scalable and secure
- [ ] Performance requirements are realistic
- [ ] Integration points are well-defined
- [ ] Compliance requirements are addressed
- [ ] Testing strategy is comprehensive

**Business Alignment**
- [ ] User needs are prioritized
- [ ] Business objectives are clear
- [ ] Market requirements are addressed
- [ ] ROI and success metrics defined
- [ ] Go-to-market strategy included

---

## Conclusion

This comprehensive prompt library provides a complete foundation for generating world-class documentation for EdTech SaaS platforms. The prompts are designed to be:

- **Comprehensive**: Covering all aspects of platform development
- **Scalable**: Applicable to platforms of various sizes and complexities
- **Flexible**: Easily customizable for different requirements
- **Professional**: Generating production-ready documentation
- **Standards-Compliant**: Following industry best practices and regulations

### Usage Recommendations

1. **Start with Foundation**: Use Master PRD and Architecture prompts first
2. **Build Systematically**: Follow the logical sequence for comprehensive coverage
3. **Customize Thoroughly**: Replace all variables with your specific requirements
4. **Validate Continuously**: Review generated content for accuracy and completeness
5. **Iterate and Improve**: Refine prompts based on your specific needs and outcomes

These prompts will enable you to quickly generate comprehensive, professional documentation for any EdTech SaaS project, saving significant time while ensuring all critical aspects are properly addressed.

---

**Document Control**
- **Created**: 2025-05-23 13:25:43 UTC
- **Author**: Intelli-SAAS
- **Version**: 1.2
- **Next Review**: 2025-06-23
- **Distribution**: Development Teams, Product Managers, Technical Architects

---

**End of Comprehensive EdTech SaaS Documentation Prompt Library**