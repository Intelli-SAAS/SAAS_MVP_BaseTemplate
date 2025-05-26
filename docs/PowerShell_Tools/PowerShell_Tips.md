# PowerShell Tips for EduSpry Platform Development

## Running the Development Server

```powershell
# Navigate to the project directory
cd "c:\Users\User\Desktop\Cursor - Projects\Eduspry - Education For All\eduspry-platform"

# Start the development server
npm run dev
```

## Environment Variables in PowerShell

Setting environment variables for a session:

```powershell
# Set environment variables temporarily
$env:NEXT_PUBLIC_SUPABASE_URL = "your-supabase-url"
$env:NEXT_PUBLIC_SUPABASE_ANON_KEY = "your-anon-key"

# Run the development server with environment variables
npm run dev
```

Creating a `.env.local` file:

```powershell
# Create or overwrite the .env.local file
Set-Content -Path .\.env.local -Value @"
NEXT_PUBLIC_SUPABASE_URL=your-supabase-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
"@
```

## Git Commands in PowerShell

```powershell
# Check Git status
git status

# Create a new branch
git checkout -b feature/new-feature

# Commit changes with a message
git commit -m "Add new feature"

# Push to remote repository
git push origin feature/new-feature
```

## PowerShell Scripts for Common Tasks

### Database Migrations Validation Script

```powershell
function Test-DatabaseConnection {
    param (
        [Parameter(Mandatory=$true)]
        [string]$ConnectionString
    )
    
    try {
        # Your connection testing logic here
        Write-Output "Database connection successful"
        return $true
    }
    catch {
        Write-Error "Database connection failed: $_"
        return $false
    }
}
```

### Environment Validation Script

Create a file called `validate-env.ps1` with the following content:

```powershell
# Required environment variables
$requiredVars = @(
    "NEXT_PUBLIC_SUPABASE_URL",
    "NEXT_PUBLIC_SUPABASE_ANON_KEY"
)

$missingVars = @()

foreach ($var in $requiredVars) {
    if (-not (Get-Item env:$var -ErrorAction SilentlyContinue)) {
        $missingVars += $var
    }
}

if ($missingVars.Count -gt 0) {
    Write-Error "Missing required environment variables: $($missingVars -join ', ')"
    exit 1
}

Write-Output "All required environment variables are set"
```

## Installing Dependencies

```powershell
# Install all dependencies
npm install

# Install a specific package
npm install @supabase/auth-helpers-nextjs

# Install development dependencies
npm install -D tailwindcss postcss autoprefixer
```

## Deployment Commands

```powershell
# Build the application
npm run build

# Start the production server
npm start
```

## Multi-Tenant Testing

```powershell
# Set tenant context for testing
$env:NEXT_TENANT_ID = "tenant-id"
npm run test
```

## PowerShell Profile Configuration

Setting up your PowerShell profile will greatly improve your development workflow:

### Creating a PowerShell Profile

If you don't have a PowerShell profile yet:

```powershell
# Check if profile exists
Test-Path $PROFILE

# Create profile if it doesn't exist
if (!(Test-Path $PROFILE)) {
    New-Item -Path $PROFILE -Type File -Force
}

# Open the profile in notepad to edit
notepad $PROFILE
```

### Useful PowerShell Aliases

Add these to your PowerShell profile for convenience:

```powershell
# Add to your PowerShell profile
function eduspry { cd "c:\Users\User\Desktop\Cursor - Projects\Eduspry - Education For All\eduspry-platform" }
function dev { npm run dev }
function build { npm run build }
function supabase { npx supabase $args }

# Shorthand for switching tenant context
function set-tenant($tenantId) {
    $env:NEXT_TENANT_ID = $tenantId
    Write-Output "Tenant context set to: $tenantId"
}
```

To use these aliases, restart your PowerShell terminal or reload your profile with:

```powershell
. $PROFILE
```

## Line Ending Management in PowerShell

When working with Git in a cross-platform environment:

```powershell
# Check current git config for line endings
git config --get core.autocrlf

# Configure git to handle line endings properly
git config --global core.autocrlf true
```

---

This document provides PowerShell-specific commands and scripts for working with the EduSpry platform. For more general development information, refer to the [Technical Details](./TechnicalDetails.md) documentation.
