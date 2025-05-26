# Environment Configuration Files

This document outlines the environment-specific configuration files for the Eduspry platform across development, pre-production, and production environments.

> **Note:** PowerShell is the default terminal used for this project. All commands and scripts should be written for PowerShell compatibility.

## Environment Setup Instructions

### 1. Development Environment (.env.local)
Used for local development with Supabase development project.

### 2. Pre-Production Environment (.env.preprod)
Used for staging and testing with Supabase staging project.

### 3. Production Environment (.env.production)
Used for production deployment with Supabase production project.

## Security Notes

1. **Never commit actual .env files to version control**
2. **Use .env.example files as templates**
3. **Store production secrets in secure secret management systems**
4. **Rotate API keys and secrets regularly**
5. **Use different databases for each environment**

## Environment Variable Categories

### Database Configuration
- Database connection strings
- Connection pool settings
- Migration configurations

### Authentication Configuration
- JWT secrets and settings
- OAuth provider credentials
- Session configuration

### Storage Configuration
- File storage settings
- CDN configuration
- Media upload limits

### API Configuration
- External API keys
- Rate limiting settings
- CORS configuration

### Monitoring Configuration
- Analytics tracking IDs
- Error reporting services
- Performance monitoring

### Feature Flags
- Environment-specific feature toggles
- A/B testing configurations
- Maintenance mode settings

## Setup Instructions

1. Copy the appropriate `.env.example` file to `.env.local` for development using PowerShell:
   ```powershell
   Copy-Item .env.example -Destination .env.local
   ```
2. Fill in the required values from your Supabase project
3. Ensure all team members have access to development environment variables
4. Configure CI/CD pipeline with staging and production environment variables
5. Set up monitoring and alerting for environment-specific issues

## PowerShell Environment Setup

For PowerShell users, you can set environment variables for a session using:
```powershell
$env:NEXT_PUBLIC_SUPABASE_URL = "your-supabase-url"
$env:NEXT_PUBLIC_SUPABASE_ANON_KEY = "your-anon-key"
```

See the [PowerShell Tips](../docs/PowerShell_Tips.md) document for more detailed PowerShell commands and scripts specific to this project.

## Environment Validation

Each environment should have validation scripts to ensure:
- All required environment variables are set
- Database connections are working
- External services are accessible
- Feature flags are properly configured
- Security settings are correct

This ensures consistent behavior across all environments and prevents deployment issues due to missing or incorrect configuration.
