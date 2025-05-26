# AI Development Rules for Eduspry Platform

## Core Development Principles

### 1. Production-Grade Standards
- **NO Static Data**: All data must come from database sources or APIs
- **NO Mock Implementations**: Every feature must have real backend functionality
- **NO Shortcuts**: Implement robust, scalable solutions from the start
- **Comprehensive Testing**: Unit, integration, and E2E tests for all features
- **Complete Documentation**: Update all relevant documentation after each module

### 2. Architecture Guidelines

#### Frontend Architecture
- **Framework**: Next.js 14+ with TypeScript
- **State Management**: React Query for server state, Redux Toolkit for complex client state
- **UI Framework**: Shadcn/UI with Tailwind CSS
- **Component Structure**: Atomic design methodology (atoms → molecules → organisms → templates → pages)
- **Code Splitting**: Route-based and component-level splitting
- **Performance**: Core Web Vitals compliance (LCP < 2.5s, FID < 100ms, CLS < 0.1)

#### Backend Architecture
- **Framework**: NestJS with TypeScript
- **Database**: Supabase (PostgreSQL) for primary data storage
- **File Storage**: Supabase Storage for media and documents
- **Edge Functions**: Supabase Edge Functions for serverless operations
- **Authentication**: Supabase Auth with JWT
- **Real-time**: Supabase Realtime for live updates
- **API Design**: RESTful APIs with GraphQL for complex queries

#### Database Design
- **Primary DB**: Supabase PostgreSQL with proper indexing
- **Schema Design**: Normalized tables with proper relationships
- **Migrations**: Version-controlled database migrations
- **Security**: Row Level Security (RLS) policies
- **Backups**: Automated backup strategies

### 3. Security Best Practices

#### Authentication & Authorization
- **Multi-factor Authentication**: Required for all users
- **Role-Based Access Control (RBAC)**: Granular permissions system
- **JWT Security**: Secure token handling with refresh mechanisms
- **Session Management**: Secure session handling with proper expiration
- **OAuth Integration**: Support for Google, Microsoft, Apple authentication

#### Data Security
- **Encryption**: Data encryption at rest and in transit
- **Input Validation**: Comprehensive server-side validation
- **SQL Injection Prevention**: Parameterized queries and ORM usage
- **XSS Protection**: Content Security Policy and input sanitization
- **CSRF Protection**: Cross-site request forgery protection
- **Rate Limiting**: API rate limiting and DDoS protection

### 4. Performance & Scalability

#### Frontend Performance
- **Code Splitting**: Lazy loading for components and routes
- **Image Optimization**: Next.js Image component with WebP format
- **Caching**: Browser caching and CDN integration
- **Bundle Size**: Regular bundle analysis and optimization
- **Web Vitals**: Continuous monitoring and optimization

#### Backend Performance
- **Database Optimization**: Proper indexing and query optimization
- **Caching Strategy**: Redis for session and data caching
- **Connection Pooling**: Database connection pooling
- **Edge Functions**: Serverless functions for scalable operations
- **CDN**: Content delivery network for static assets

### 5. Development Environment

#### Terminal and Scripting
- **PowerShell**: PowerShell is the default terminal for this project
- **Script Compatibility**: All scripts must be written for PowerShell
- **Environment Variables**: Use PowerShell syntax for environment variables
- **Command Syntax**: Follow PowerShell conventions for commands and scripts
- **Documentation**: All development commands should include PowerShell examples

### 6. Testing Strategy

#### Frontend Testing
- **Unit Tests**: Jest for utility functions and hooks
- **Component Tests**: React Testing Library for component behavior
- **Integration Tests**: API integration and state management testing
- **E2E Tests**: Playwright for critical user flows
- **Visual Tests**: Storybook for component documentation and testing

#### Backend Testing
- **Unit Tests**: Jest for service and utility functions
- **Integration Tests**: Supertest for API endpoint testing
- **Database Tests**: Test database with proper setup/teardown
- **Performance Tests**: Load testing for API endpoints
- **Security Tests**: Security vulnerability scanning

### 6. Code Quality Standards

#### Code Style
- **TypeScript**: Strict mode enabled with comprehensive type definitions
- **ESLint**: Airbnb style guide with custom rules
- **Prettier**: Consistent code formatting
- **Naming Conventions**: Clear, descriptive naming for variables, functions, and classes
- **Comments**: Comprehensive JSDoc comments for complex functions

#### Code Organization
- **Modular Structure**: Feature-based module organization
- **Single Responsibility**: Each module/component has one clear purpose
- **Dependency Injection**: Proper dependency management
- **Error Handling**: Comprehensive error handling with logging
- **Type Safety**: Full TypeScript coverage with strict typing

### 7. Environment Management

#### Development Environments
- **Local Development**: Docker Compose for consistent development environment
- **Development**: Supabase development project
- **Pre-Production**: Staging environment for testing
- **Production**: Production environment with monitoring

#### Configuration Management
- **Environment Variables**: Secure environment variable management
- **Configuration Files**: Separate configs for each environment
- **Secrets Management**: Secure handling of API keys and secrets
- **Feature Flags**: Environment-based feature toggling

### 8. Observability & Monitoring

#### Logging
- **Structured Logging**: JSON-based logging with proper levels
- **Error Tracking**: Comprehensive error logging and tracking
- **Performance Logging**: API response times and database query performance
- **Security Logging**: Authentication and authorization events
- **Audit Trail**: Complete audit trail for all user actions

#### Metrics & Monitoring
- **Application Metrics**: Performance metrics collection
- **Infrastructure Metrics**: Server and database monitoring
- **User Analytics**: User behavior and feature usage analytics
- **Alerts**: Automated alerting for critical issues
- **SLA Monitoring**: Service level agreement compliance tracking

### 9. Deployment & DevOps

#### CI/CD Pipeline
- **Automated Testing**: All tests must pass before deployment
- **Code Quality Gates**: ESLint, Prettier, and type checking
- **Security Scanning**: Vulnerability scanning in pipeline
- **Automated Deployment**: Automated deployment to all environments
- **Rollback Strategy**: Quick rollback capabilities

#### Infrastructure
- **Containerization**: Docker for consistent deployment
- **Orchestration**: Kubernetes for production orchestration
- **Load Balancing**: Proper load balancing and auto-scaling
- **Database Management**: Automated backups and disaster recovery
- **CDN Integration**: Global content delivery network

### 10. Documentation Requirements

#### Code Documentation
- **README Files**: Comprehensive README for each module
- **API Documentation**: OpenAPI/Swagger documentation for all APIs
- **Component Documentation**: Storybook documentation for UI components
- **Database Schema**: ER diagrams and schema documentation
- **Architecture Diagrams**: System architecture and flow diagrams

#### Process Documentation
- **Development Workflow**: Clear development and deployment processes
- **Testing Procedures**: Testing strategies and procedures
- **Security Procedures**: Security best practices and incident response
- **Deployment Guide**: Step-by-step deployment instructions
- **Troubleshooting Guide**: Common issues and solutions

### 11. Module Development Workflow

#### Pre-Development
1. **Requirements Analysis**: Clear understanding of module requirements
2. **Architecture Planning**: Design module architecture and interfaces
3. **Database Design**: Design database schema and relationships
4. **API Design**: Design RESTful APIs and GraphQL schemas
5. **Testing Strategy**: Plan testing approach and test cases

#### Development Phase
1. **Database Migration**: Create and test database migrations
2. **Backend Implementation**: Implement backend services and APIs
3. **Frontend Implementation**: Implement frontend components and pages
4. **Integration**: Integrate frontend and backend components
5. **Testing**: Implement and run all test types

#### Post-Development
1. **Code Review**: Peer review of all code changes
2. **Documentation Update**: Update all relevant documentation
3. **Performance Testing**: Load and performance testing
4. **Security Review**: Security vulnerability assessment
5. **Deployment**: Deploy to development and staging environments

### 12. Quality Gates

#### Before Module Completion
- [ ] All tests passing (unit, integration, E2E)
- [ ] Code coverage > 80%
- [ ] Performance benchmarks met
- [ ] Security review completed
- [ ] Documentation updated
- [ ] Code review approved
- [ ] Accessibility compliance verified
- [ ] Browser compatibility tested
- [ ] Mobile responsiveness verified
- [ ] Error handling implemented

#### Before Production Deployment
- [ ] Staging environment testing completed
- [ ] Performance testing under load
- [ ] Security penetration testing
- [ ] Backup and recovery testing
- [ ] Monitoring and alerting configured
- [ ] Rollback procedures tested
- [ ] Documentation finalized
- [ ] Team training completed

### 13. Technology Stack Constraints

#### Required Technologies
- **Frontend**: Next.js 14+, TypeScript, Tailwind CSS, Shadcn/UI
- **Backend**: NestJS, TypeScript, Supabase
- **Database**: Supabase PostgreSQL
- **Storage**: Supabase Storage
- **Authentication**: Supabase Auth
- **Real-time**: Supabase Realtime
- **Edge Functions**: Supabase Edge Functions
- **Testing**: Jest, Playwright, React Testing Library
- **CI/CD**: GitHub Actions
- **Monitoring**: Supabase Analytics + custom metrics

#### Prohibited Approaches
- Static/mock data in production code
- Inline styles (use Tailwind classes)
- Direct database access from frontend
- Hardcoded configuration values
- Untyped JavaScript code
- Missing error handling
- Unprotected API endpoints
- Missing input validation
- Incomplete test coverage
- Missing documentation

### 14. Cost Optimization

#### Free Tier Utilization
- **Supabase**: Maximize free tier usage with efficient queries
- **Vercel**: Deploy frontend on Vercel free tier
- **GitHub**: Use GitHub Actions free tier for CI/CD
- **Monitoring**: Use free monitoring tools where possible

#### Resource Optimization
- **Database**: Optimize queries and use appropriate indexing
- **Storage**: Implement file compression and optimization
- **Edge Functions**: Optimize function execution time
- **CDN**: Use efficient caching strategies
- **API**: Implement proper pagination and filtering

### 15. Confirmation Requirements

#### Before Each Module Implementation
1. **Architecture Review**: Confirm module architecture and dependencies
2. **Database Schema Review**: Confirm database design and migrations
3. **API Design Review**: Confirm API endpoints and data structures
4. **UI/UX Review**: Confirm user interface and user experience design
5. **Testing Strategy Review**: Confirm testing approach and coverage
6. **Documentation Plan**: Confirm documentation requirements and updates

#### Module Completion Checklist
- [ ] All functionality implemented and tested
- [ ] Database migrations applied and tested
- [ ] APIs documented and tested
- [ ] Frontend components documented in Storybook
- [ ] All tests passing with adequate coverage
- [ ] Performance benchmarks met
- [ ] Security review completed
- [ ] Documentation updated
- [ ] Code review completed and approved
- [ ] Ready for staging deployment

### 16. Communication Protocol

#### Implementation Planning Process
1. **Provide Implementation Plan First**: Always provide a detailed implementation plan before making any changes
2. **Seek Confirmation**: Ask for explicit approval before executing the implementation plan
3. **Outline Impacts**: Clearly describe potential impacts or considerations of proposed changes
4. **Present Alternatives**: When relevant, provide alternative approaches with pros and cons
5. **Await Approval**: Only proceed with implementation after receiving explicit confirmation

#### Question and Clarification Process
1. **Never Hallucinate**: Always ask for clarification when uncertain
2. **Detailed Questions**: Ask specific, detailed questions about requirements
3. **Architecture Confirmation**: Confirm architectural decisions before implementation
4. **Progress Updates**: Provide regular updates on implementation progress
5. **Issue Escalation**: Immediately report any blockers or critical issues

#### Documentation Requirements
1. **Change Log**: Maintain detailed change log for each module
2. **API Changes**: Document all API changes and breaking changes
3. **Database Changes**: Document all schema changes and migrations
4. **Architecture Updates**: Update architecture documentation with each module
5. **Deployment Notes**: Document any deployment-specific requirements
6. **Prompt Tracking**: Record all significant prompts in the PromptTracker.md document

### 15. Prompt Management and Documentation

#### Prompt Documentation Requirements
- **Save All Significant Prompts**: All non-trivial prompts must be saved in the [PromptTracker.md](../docs/PromptTracker.md) file
- **Prompt Metadata**: Include date, purpose, tags, and notes about the results
- **Prompt Categorization**: Categorize prompts by their primary function (architecture, development, testing, deployment)
- **Iterative Improvements**: Document prompt refinements and optimizations
- **Include Context**: Document important context provided with the prompt

#### Prompt Best Practices
- **Clear Instructions**: Be specific and detail-oriented in prompt writing
- **Task Breakdown**: Split complex tasks into smaller, focused prompts
- **Format Specification**: Indicate desired output format and structure
- **Example Inclusion**: Use examples to guide AI responses
- **Consistency**: Maintain consistent style and terminology

This document serves as the definitive guide for AI development on the Eduspry platform. All AI agents must adhere to these rules to ensure consistent, high-quality, production-ready code.
