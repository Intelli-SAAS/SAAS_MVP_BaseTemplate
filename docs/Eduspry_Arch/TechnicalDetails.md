# EduSpry Platform Technical Details

**Document Version:** 1.0  
**Created Date:** 2025-05-23  
**Last Updated:** 2025-05-23  
**Document Type:** Technical Implementation Guide

---

## Table of Contents

1. [Development Environment Setup](#development-environment-setup)
2. [Database Implementation](#database-implementation)
3. [API Development](#api-development)
4. [Frontend Implementation](#frontend-implementation)
5. [Authentication & Authorization](#authentication--authorization)
6. [Multi-Tenant Implementation](#multi-tenant-implementation)
7. [Performance Optimization](#performance-optimization)
8. [Testing Strategy](#testing-strategy)
9. [Security Implementation](#security-implementation)
10. [Deployment & DevOps](#deployment--devops)
11. [Microservices Architecture](#microservices-architecture)

> **Note**: For a comprehensive guide on microservices implementation, code organization, and Supabase Edge Functions integration, see the [Microservices Implementation Guide](./Microservices_Implementation_Guide.md).

---

## Development Environment Setup

### Prerequisites
```yaml
Required Software:
  - Node.js 18+ with npm/yarn/pnpm
  - Docker Desktop for containerization
  - Git for version control
  - VS Code or preferred IDE
  - PostgreSQL client (optional, using Supabase)

Development Tools:
  - Supabase CLI for local development
  - Postman for API testing
  - Chrome DevTools for debugging
  - React Developer Tools
```

### Project Structure
```
eduspry-platform/
├── src/
│   ├── app/                    # Next.js app router
│   │   ├── (auth)/            # Authentication routes
│   │   ├── (dashboard)/       # Dashboard routes
│   │   ├── api/               # API routes
│   │   └── globals.css        # Global styles
│   ├── components/            # React components
│   │   ├── ui/                # Base UI components
│   │   ├── forms/             # Form components
│   │   └── layout/            # Layout components
│   ├── lib/                   # Utility libraries
│   │   ├── supabase/          # Supabase client
│   │   ├── auth/              # Authentication utils
│   │   └── utils.ts           # General utilities
│   ├── hooks/                 # Custom React hooks
│   ├── types/                 # TypeScript type definitions
│   └── constants/             # Application constants
├── docs/                      # Documentation
├── tests/                     # Test files
├── public/                    # Static assets
└── database/                  # Database migrations and seeds
```

### Environment Configuration
```typescript
// .env.local example
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key
NEXTAUTH_SECRET=your_nextauth_secret
NEXTAUTH_URL=http://localhost:3000

// Environment validation with Zod
import { z } from 'zod';

const envSchema = z.object({
  NEXT_PUBLIC_SUPABASE_URL: z.string().url(),
  NEXT_PUBLIC_SUPABASE_ANON_KEY: z.string().min(1),
  SUPABASE_SERVICE_ROLE_KEY: z.string().min(1),
  NODE_ENV: z.enum(['development', 'test', 'production']),
});

export const env = envSchema.parse(process.env);
```

---

## Database Implementation

### Supabase Setup and Configuration

#### Database Schema Design
```sql
-- Enable necessary extensions
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "pgcrypto";

-- Custom types
CREATE TYPE user_role AS ENUM ('super_admin', 'tenant_admin', 'instructor', 'student', 'parent');
CREATE TYPE tenant_type AS ENUM ('individual', 'institution', 'edutech');
CREATE TYPE subscription_status AS ENUM ('active', 'inactive', 'trial', 'suspended');

-- Tenants table
CREATE TABLE tenants (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(255) NOT NULL,
    slug VARCHAR(100) UNIQUE NOT NULL,
    domain VARCHAR(255),
    tenant_type tenant_type NOT NULL DEFAULT 'individual',
    subscription_status subscription_status NOT NULL DEFAULT 'trial',
    settings JSONB DEFAULT '{}',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Users table with tenant association
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    email VARCHAR(255) UNIQUE NOT NULL,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    role user_role NOT NULL DEFAULT 'student',
    profile_data JSONB DEFAULT '{}',
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Courses table
CREATE TABLE courses (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    instructor_id UUID REFERENCES users(id),
    title VARCHAR(255) NOT NULL,
    description TEXT,
    content JSONB DEFAULT '{}',
    settings JSONB DEFAULT '{}',
    is_published BOOLEAN DEFAULT false,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Enrollments table
CREATE TABLE enrollments (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE,
    student_id UUID REFERENCES users(id),
    course_id UUID REFERENCES courses(id),
    enrolled_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    completed_at TIMESTAMP WITH TIME ZONE,
    progress DECIMAL(5,2) DEFAULT 0.00,
    grade DECIMAL(5,2),
    UNIQUE(student_id, course_id)
);
```

#### Row Level Security (RLS) Policies
```sql
-- Enable RLS on all tenant-aware tables
ALTER TABLE tenants ENABLE ROW LEVEL SECURITY;
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE courses ENABLE ROW LEVEL SECURITY;
ALTER TABLE enrollments ENABLE ROW LEVEL SECURITY;

-- Tenant isolation policy for users
CREATE POLICY "Users can only access their tenant data" ON users
    FOR ALL USING (
        tenant_id = (
            SELECT tenant_id FROM users 
            WHERE id = auth.uid()
        )
    );

-- Course access policy
CREATE POLICY "Course access by tenant" ON courses
    FOR ALL USING (
        tenant_id = (
            SELECT tenant_id FROM users 
            WHERE id = auth.uid()
        )
    );

-- Enrollment policy
CREATE POLICY "Enrollment access by tenant and user" ON enrollments
    FOR ALL USING (
        tenant_id = (
            SELECT tenant_id FROM users 
            WHERE id = auth.uid()
        )
        AND (
            student_id = auth.uid() 
            OR 
            EXISTS (
                SELECT 1 FROM users 
                WHERE id = auth.uid() 
                AND role IN ('instructor', 'tenant_admin', 'super_admin')
            )
        )
    );
```

#### Database Client Setup
```typescript
// lib/supabase/client.ts
import { createClient } from '@supabase/supabase-js';
import { Database } from '@/types/database';

const supabaseUrl = process.env.NEXT_PUBLIC_SUPABASE_URL!;
const supabaseKey = process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!;

export const supabase = createClient<Database>(supabaseUrl, supabaseKey, {
  auth: {
    autoRefreshToken: true,
    persistSession: true,
    detectSessionInUrl: true
  },
  global: {
    headers: {
      'x-application': 'eduspry-platform'
    }
  }
});

// Server-side client with service role
export const supabaseAdmin = createClient<Database>(
  supabaseUrl,
  process.env.SUPABASE_SERVICE_ROLE_KEY!,
  {
    auth: {
      autoRefreshToken: false,
      persistSession: false
    }
  }
);
```

---

## API Development

### API Route Structure with Next.js App Router

#### Authentication Middleware
```typescript
// lib/auth/middleware.ts
import { NextRequest, NextResponse } from 'next/server';
import { createClient } from '@supabase/supabase-js';

export async function withAuth(
  request: NextRequest,
  handler: (req: NextRequest, context: AuthContext) => Promise<Response>
) {
  try {
    const token = request.headers.get('authorization')?.replace('Bearer ', '');
    
    if (!token) {
      return NextResponse.json({ error: 'Unauthorized' }, { status: 401 });
    }

    const supabase = createClient(
      process.env.NEXT_PUBLIC_SUPABASE_URL!,
      process.env.SUPABASE_SERVICE_ROLE_KEY!
    );

    const { data: { user }, error } = await supabase.auth.getUser(token);

    if (error || !user) {
      return NextResponse.json({ error: 'Invalid token' }, { status: 401 });
    }

    // Get user tenant information
    const { data: userData } = await supabase
      .from('users')
      .select('tenant_id, role')
      .eq('id', user.id)
      .single();

    const context: AuthContext = {
      user,
      tenantId: userData?.tenant_id,
      role: userData?.role,
    };

    return await handler(request, context);
  } catch (error) {
    return NextResponse.json({ error: 'Internal server error' }, { status: 500 });
  }
}

interface AuthContext {
  user: any;
  tenantId: string;
  role: string;
}
```

#### API Route Example
```typescript
// app/api/courses/route.ts
import { NextRequest } from 'next/server';
import { withAuth } from '@/lib/auth/middleware';
import { supabaseAdmin } from '@/lib/supabase/client';
import { z } from 'zod';

const createCourseSchema = z.object({
  title: z.string().min(1).max(255),
  description: z.string().optional(),
  content: z.object({}).optional(),
});

export async function GET(request: NextRequest) {
  return withAuth(request, async (req, context) => {
    const { tenantId, role } = context;
    
    // Set tenant context for RLS
    await supabaseAdmin.rpc('set_tenant_context', { tenant_id: tenantId });

    const { data: courses, error } = await supabaseAdmin
      .from('courses')
      .select(`
        id,
        title,
        description,
        is_published,
        created_at,
        instructor:users!instructor_id(
          first_name,
          last_name,
          email
        )
      `)
      .eq('tenant_id', tenantId);

    if (error) {
      return Response.json({ error: error.message }, { status: 400 });
    }

    return Response.json({ courses });
  });
}

export async function POST(request: NextRequest) {
  return withAuth(request, async (req, context) => {
    const { user, tenantId, role } = context;

    // Check permissions
    if (!['instructor', 'tenant_admin', 'super_admin'].includes(role)) {
      return Response.json({ error: 'Insufficient permissions' }, { status: 403 });
    }

    const body = await request.json();
    const validatedData = createCourseSchema.parse(body);

    await supabaseAdmin.rpc('set_tenant_context', { tenant_id: tenantId });

    const { data: course, error } = await supabaseAdmin
      .from('courses')
      .insert({
        ...validatedData,
        tenant_id: tenantId,
        instructor_id: user.id,
      })
      .select()
      .single();

    if (error) {
      return Response.json({ error: error.message }, { status: 400 });
    }

    return Response.json({ course }, { status: 201 });
  });
}
```

#### tRPC Integration for Type Safety
```typescript
// lib/trpc/router.ts
import { z } from 'zod';
import { createTRPCRouter, protectedProcedure } from './trpc';

export const courseRouter = createTRPCRouter({
  getAll: protectedProcedure
    .query(async ({ ctx }) => {
      const { supabase, tenantId } = ctx;
      
      const { data: courses } = await supabase
        .from('courses')
        .select('*')
        .eq('tenant_id', tenantId);
        
      return courses || [];
    }),

  create: protectedProcedure
    .input(z.object({
      title: z.string().min(1),
      description: z.string().optional(),
    }))
    .mutation(async ({ input, ctx }) => {
      const { supabase, user, tenantId } = ctx;
      
      const { data: course } = await supabase
        .from('courses')
        .insert({
          ...input,
          tenant_id: tenantId,
          instructor_id: user.id,
        })
        .select()
        .single();
        
      return course;
    }),
});
```

---

## Frontend Implementation

### Component Architecture

#### Base UI Components with Shadcn/UI
```typescript
// components/ui/button.tsx (already implemented)
// Using Shadcn/UI components as base

// components/forms/course-form.tsx
"use client";

import { useState } from 'react';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Textarea } from '@/components/ui/textarea';
import { Form, FormControl, FormField, FormItem, FormLabel, FormMessage } from '@/components/ui/form';

const courseSchema = z.object({
  title: z.string().min(1, 'Title is required').max(255),
  description: z.string().optional(),
});

type CourseFormData = z.infer<typeof courseSchema>;

interface CourseFormProps {
  onSubmit: (data: CourseFormData) => Promise<void>;
  initialData?: Partial<CourseFormData>;
  isLoading?: boolean;
}

export function CourseForm({ onSubmit, initialData, isLoading }: CourseFormProps) {
  const form = useForm<CourseFormData>({
    resolver: zodResolver(courseSchema),
    defaultValues: {
      title: initialData?.title || '',
      description: initialData?.description || '',
    },
  });

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-6">
        <FormField
          control={form.control}
          name="title"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Course Title</FormLabel>
              <FormControl>
                <Input {...field} placeholder="Enter course title" />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        
        <FormField
          control={form.control}
          name="description"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Description</FormLabel>
              <FormControl>
                <Textarea {...field} placeholder="Enter course description" />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        
        <Button type="submit" disabled={isLoading}>
          {isLoading ? 'Creating...' : 'Create Course'}
        </Button>
      </form>
    </Form>
  );
}
```

#### State Management with React Query
```typescript
// hooks/use-courses.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { supabase } from '@/lib/supabase/client';

export function useCourses() {
  return useQuery({
    queryKey: ['courses'],
    queryFn: async () => {
      const { data, error } = await supabase
        .from('courses')
        .select(`
          id,
          title,
          description,
          is_published,
          created_at,
          instructor:users!instructor_id(
            first_name,
            last_name
          )
        `);
      
      if (error) throw error;
      return data;
    },
  });
}

export function useCreateCourse() {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: async (courseData: { title: string; description?: string }) => {
      const { data, error } = await supabase
        .from('courses')
        .insert(courseData)
        .select()
        .single();
      
      if (error) throw error;
      return data;
    },
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['courses'] });
    },
  });
}
```

#### Layout Components
```typescript
// components/layout/dashboard-layout.tsx
"use client";

import { ReactNode } from 'react';
import { Sidebar } from './sidebar';
import { Header } from './header';
import { useAuth } from '@/hooks/use-auth';

interface DashboardLayoutProps {
  children: ReactNode;
}

export function DashboardLayout({ children }: DashboardLayoutProps) {
  const { user, tenant } = useAuth();

  return (
    <div className="flex h-screen bg-gray-50">
      <Sidebar user={user} tenant={tenant} />
      <div className="flex-1 flex flex-col overflow-hidden">
        <Header user={user} tenant={tenant} />
        <main className="flex-1 overflow-x-hidden overflow-y-auto bg-gray-50 p-6">
          {children}
        </main>
      </div>
    </div>
  );
}
```

---

## Authentication & Authorization

### Supabase Auth Integration

#### Authentication Hook
```typescript
// hooks/use-auth.ts
"use client";

import { useEffect, useState } from 'react';
import { User } from '@supabase/supabase-js';
import { supabase } from '@/lib/supabase/client';

interface AuthUser extends User {
  tenant_id?: string;
  role?: string;
}

export function useAuth() {
  const [user, setUser] = useState<AuthUser | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Get initial session
    supabase.auth.getSession().then(({ data: { session } }) => {
      if (session?.user) {
        fetchUserProfile(session.user);
      } else {
        setLoading(false);
      }
    });

    // Listen for auth changes
    const { data: { subscription } } = supabase.auth.onAuthStateChange(
      async (event, session) => {
        if (session?.user) {
          await fetchUserProfile(session.user);
        } else {
          setUser(null);
          setLoading(false);
        }
      }
    );

    return () => subscription.unsubscribe();
  }, []);

  const fetchUserProfile = async (authUser: User) => {
    try {
      const { data: profile } = await supabase
        .from('users')
        .select('tenant_id, role')
        .eq('id', authUser.id)
        .single();

      setUser({
        ...authUser,
        tenant_id: profile?.tenant_id,
        role: profile?.role,
      });
    } catch (error) {
      console.error('Error fetching user profile:', error);
    } finally {
      setLoading(false);
    }
  };

  const signIn = async (email: string, password: string) => {
    const { data, error } = await supabase.auth.signInWithPassword({
      email,
      password,
    });
    return { data, error };
  };

  const signOut = async () => {
    await supabase.auth.signOut();
  };

  return {
    user,
    loading,
    signIn,
    signOut,
  };
}
```

#### Role-Based Access Control
```typescript
// lib/auth/permissions.ts
export enum Role {
  SUPER_ADMIN = 'super_admin',
  TENANT_ADMIN = 'tenant_admin',
  INSTRUCTOR = 'instructor',
  STUDENT = 'student',
  PARENT = 'parent',
}

export enum Permission {
  CREATE_COURSE = 'create_course',
  EDIT_COURSE = 'edit_course',
  DELETE_COURSE = 'delete_course',
  VIEW_ANALYTICS = 'view_analytics',
  MANAGE_USERS = 'manage_users',
}

const rolePermissions: Record<Role, Permission[]> = {
  [Role.SUPER_ADMIN]: [
    Permission.CREATE_COURSE,
    Permission.EDIT_COURSE,
    Permission.DELETE_COURSE,
    Permission.VIEW_ANALYTICS,
    Permission.MANAGE_USERS,
  ],
  [Role.TENANT_ADMIN]: [
    Permission.CREATE_COURSE,
    Permission.EDIT_COURSE,
    Permission.DELETE_COURSE,
    Permission.VIEW_ANALYTICS,
    Permission.MANAGE_USERS,
  ],
  [Role.INSTRUCTOR]: [
    Permission.CREATE_COURSE,
    Permission.EDIT_COURSE,
    Permission.VIEW_ANALYTICS,
  ],
  [Role.STUDENT]: [],
  [Role.PARENT]: [],
};

export function hasPermission(userRole: Role, permission: Permission): boolean {
  return rolePermissions[userRole]?.includes(permission) || false;
}

// React hook for permission checking
export function usePermissions() {
  const { user } = useAuth();
  
  const checkPermission = (permission: Permission): boolean => {
    if (!user?.role) return false;
    return hasPermission(user.role as Role, permission);
  };

  return { checkPermission };
}
```

---

## Multi-Tenant Implementation

### Tenant Context Management

#### Tenant Context Provider
```typescript
// contexts/tenant-context.tsx
"use client";

import { createContext, useContext, ReactNode } from 'react';
import { useAuth } from '@/hooks/use-auth';

interface TenantContextValue {
  tenantId: string | null;
  tenantType: 'individual' | 'institution' | 'edutech' | null;
  tenantName: string | null;
}

const TenantContext = createContext<TenantContextValue>({
  tenantId: null,
  tenantType: null,
  tenantName: null,
});

export function TenantProvider({ children }: { children: ReactNode }) {
  const { user } = useAuth();
  
  // In a real implementation, you would fetch tenant data
  // based on the user's tenant_id
  
  return (
    <TenantContext.Provider
      value={{
        tenantId: user?.tenant_id || null,
        tenantType: null, // Fetch from tenant data
        tenantName: null, // Fetch from tenant data
      }}
    >
      {children}
    </TenantContext.Provider>
  );
}

export const useTenant = () => useContext(TenantContext);
```

#### Tenant-Aware API Calls
```typescript
// lib/api/base-client.ts
class TenantAwareClient {
  private tenantId: string | null = null;

  setTenantContext(tenantId: string) {
    this.tenantId = tenantId;
  }

  async request<T>(endpoint: string, options: RequestInit = {}): Promise<T> {
    const headers = {
      'Content-Type': 'application/json',
      ...options.headers,
    };

    if (this.tenantId) {
      headers['X-Tenant-ID'] = this.tenantId;
    }

    const response = await fetch(`/api${endpoint}`, {
      ...options,
      headers,
    });

    if (!response.ok) {
      throw new Error(`API Error: ${response.statusText}`);
    }

    return response.json();
  }
}

export const apiClient = new TenantAwareClient();
```

---

## Performance Optimization

### Caching Strategy

#### React Query Configuration
```typescript
// lib/query-client.ts
import { QueryClient } from '@tanstack/react-query';

export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000, // 5 minutes
      cacheTime: 10 * 60 * 1000, // 10 minutes
      retry: 2,
      refetchOnWindowFocus: false,
    },
    mutations: {
      retry: 1,
    },
  },
});
```

#### Database Query Optimization
```sql
-- Performance indexes
CREATE INDEX CONCURRENTLY idx_users_tenant_id ON users(tenant_id);
CREATE INDEX CONCURRENTLY idx_courses_tenant_instructor ON courses(tenant_id, instructor_id);
CREATE INDEX CONCURRENTLY idx_enrollments_student_course ON enrollments(student_id, course_id);

-- Composite indexes for common queries
CREATE INDEX CONCURRENTLY idx_courses_tenant_published ON courses(tenant_id, is_published) 
WHERE is_published = true;

-- Partial indexes for active users
CREATE INDEX CONCURRENTLY idx_users_active ON users(tenant_id, role) 
WHERE is_active = true;
```

#### Image Optimization
```typescript
// components/optimized-image.tsx
import Image from 'next/image';

interface OptimizedImageProps {
  src: string;
  alt: string;
  width?: number;
  height?: number;
  priority?: boolean;
  className?: string;
}

export function OptimizedImage({
  src,
  alt,
  width = 400,
  height = 300,
  priority = false,
  className,
}: OptimizedImageProps) {
  return (
    <Image
      src={src}
      alt={alt}
      width={width}
      height={height}
      priority={priority}
      className={className}
      sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
      placeholder="blur"
      blurDataURL="data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAYEBQYFBAYGBQYHBwYIChAKCgkJChQODwwQFxQYGBcUFhYaHSUfGhsjHBYWICwgIyYnKSopGR8tMC0oMCUoKSj/2wBDAQcHBwoIChMKChMoGhYaKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCj/wAARCAAIAAoDASIAAhEBAxEB/8QAFQABAQAAAAAAAAAAAAAAAAAAAAv/xAAhEAACAQMDBQAAAAAAAAAAAAABAgMABAUGIWGRkqGx0f/EABUBAQEAAAAAAAAAAAAAAAAAAAMF/8QAGhEAAgIDAAAAAAAAAAAAAAAAAAECEgMRkf/aAAwDAQACEQMRAD8AltJagyeH0AthI5xdrLcNM91BF5pX2HaH9bcfaSXWGaRmknyJckliyjqTzSlT54b6bk+h0R//2Q=="
    />
  );
}
```

---

## Testing Strategy

### Unit Testing Setup

#### Jest Configuration
```javascript
// jest.config.js
const nextJest = require('next/jest');

const createJestConfig = nextJest({
  dir: './',
});

const customJestConfig = {
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
  moduleNameMapping: {
    '^@/(.*)$': '<rootDir>/src/$1',
  },
  testEnvironment: 'jest-environment-jsdom',
  collectCoverageFrom: [
    'src/**/*.{js,jsx,ts,tsx}',
    '!src/**/*.d.ts',
    '!src/app/layout.tsx',
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
};

module.exports = createJestConfig(customJestConfig);
```

#### Component Testing Example
```typescript
// __tests__/components/course-form.test.tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { CourseForm } from '@/components/forms/course-form';

const createTestQueryClient = () =>
  new QueryClient({
    defaultOptions: {
      queries: { retry: false },
      mutations: { retry: false },
    },
  });

const renderWithProviders = (component: React.ReactElement) => {
  const queryClient = createTestQueryClient();
  return render(
    <QueryClientProvider client={queryClient}>
      {component}
    </QueryClientProvider>
  );
};

describe('CourseForm', () => {
  const mockOnSubmit = jest.fn();

  beforeEach(() => {
    mockOnSubmit.mockClear();
  });

  it('renders form fields correctly', () => {
    renderWithProviders(<CourseForm onSubmit={mockOnSubmit} />);
    
    expect(screen.getByLabelText(/course title/i)).toBeInTheDocument();
    expect(screen.getByLabelText(/description/i)).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /create course/i })).toBeInTheDocument();
  });

  it('validates required fields', async () => {
    renderWithProviders(<CourseForm onSubmit={mockOnSubmit} />);
    
    fireEvent.click(screen.getByRole('button', { name: /create course/i }));
    
    await waitFor(() => {
      expect(screen.getByText(/title is required/i)).toBeInTheDocument();
    });
    
    expect(mockOnSubmit).not.toHaveBeenCalled();
  });

  it('submits form with valid data', async () => {
    renderWithProviders(<CourseForm onSubmit={mockOnSubmit} />);
    
    fireEvent.change(screen.getByLabelText(/course title/i), {
      target: { value: 'Test Course' },
    });
    
    fireEvent.change(screen.getByLabelText(/description/i), {
      target: { value: 'Test Description' },
    });
    
    fireEvent.click(screen.getByRole('button', { name: /create course/i }));
    
    await waitFor(() => {
      expect(mockOnSubmit).toHaveBeenCalledWith({
        title: 'Test Course',
        description: 'Test Description',
      });
    });
  });
});
```

### Integration Testing

#### API Route Testing
```typescript
// __tests__/api/courses.test.ts
import { NextRequest } from 'next/server';
import { GET, POST } from '@/app/api/courses/route';
import { supabaseAdmin } from '@/lib/supabase/client';

jest.mock('@/lib/supabase/client');

describe('/api/courses', () => {
  const mockUser = {
    id: 'user-123',
    email: 'test@example.com',
  };

  const mockTenantId = 'tenant-123';

  beforeEach(() => {
    jest.clearAllMocks();
  });

  describe('GET /api/courses', () => {
    it('returns courses for authenticated user', async () => {
      const mockCourses = [
        {
          id: 'course-1',
          title: 'Test Course',
          description: 'Test Description',
        },
      ];

      (supabaseAdmin.from as jest.Mock).mockReturnValue({
        select: jest.fn().mockReturnValue({
          eq: jest.fn().mockResolvedValue({
            data: mockCourses,
            error: null,
          }),
        }),
      });

      const request = new NextRequest('http://localhost:3000/api/courses', {
        headers: {
          authorization: 'Bearer valid-token',
        },
      });

      const response = await GET(request);
      const data = await response.json();

      expect(response.status).toBe(200);
      expect(data.courses).toEqual(mockCourses);
    });
  });
});
```

### E2E Testing with Playwright

#### Playwright Configuration
```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './e2e',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: 'html',
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
  },
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
  ],
  webServer: {
    command: 'npm run dev',
    port: 3000,
  },
});
```

#### E2E Test Example
```typescript
// e2e/course-management.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Course Management', () => {
  test.beforeEach(async ({ page }) => {
    // Mock authentication
    await page.goto('/login');
    await page.fill('[data-testid=email]', 'instructor@test.com');
    await page.fill('[data-testid=password]', 'password');
    await page.click('[data-testid=login-button]');
    await page.waitForURL('/dashboard');
  });

  test('should create a new course', async ({ page }) => {
    await page.goto('/dashboard/courses');
    await page.click('[data-testid=create-course-button]');
    
    await page.fill('[data-testid=course-title]', 'New Test Course');
    await page.fill('[data-testid=course-description]', 'Course description');
    
    await page.click('[data-testid=submit-course]');
    
    await expect(page.locator('[data-testid=course-list]')).toContainText('New Test Course');
  });
});
```

---

## Security Implementation

### Input Validation and Sanitization

#### Zod Schema Validation
```typescript
// lib/validation/schemas.ts
import { z } from 'zod';

export const userSchema = z.object({
  email: z.string().email('Invalid email format'),
  firstName: z.string().min(1, 'First name is required').max(100),
  lastName: z.string().min(1, 'Last name is required').max(100),
  role: z.enum(['student', 'instructor', 'tenant_admin']),
});

export const courseSchema = z.object({
  title: z.string().min(1, 'Title is required').max(255),
  description: z.string().max(5000).optional(),
  content: z.object({}).optional(),
  isPublished: z.boolean().default(false),
});

// Sanitization utilities
export function sanitizeHtml(input: string): string {
  // Use a library like DOMPurify for proper HTML sanitization
  return input.replace(/<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi, '');
}

export function sanitizeFileName(filename: string): string {
  return filename.replace(/[^a-zA-Z0-9.-]/g, '_');
}
```

### API Security Middleware

#### Rate Limiting
```typescript
// lib/middleware/rate-limit.ts
import { NextRequest, NextResponse } from 'next/server';

interface RateLimitOptions {
  windowMs: number;
  maxRequests: number;
  keyGenerator?: (req: NextRequest) => string;
}

const store = new Map<string, { count: number; resetTime: number }>();

export function rateLimit(options: RateLimitOptions) {
  const { windowMs, maxRequests, keyGenerator = (req) => req.ip || 'anonymous' } = options;

  return (req: NextRequest) => {
    const key = keyGenerator(req);
    const now = Date.now();
    const windowStart = now - windowMs;

    // Clean expired entries
    for (const [k, v] of store.entries()) {
      if (v.resetTime < now) {
        store.delete(k);
      }
    }

    const current = store.get(key);
    
    if (!current || current.resetTime < now) {
      store.set(key, { count: 1, resetTime: now + windowMs });
      return null; // Allow request
    }

    if (current.count >= maxRequests) {
      return NextResponse.json(
        { error: 'Too many requests' },
        { status: 429 }
      );
    }

    current.count++;
    return null; // Allow request
  };
}
```

#### CORS Configuration
```typescript
// lib/middleware/cors.ts
import { NextRequest, NextResponse } from 'next/server';

const ALLOWED_ORIGINS = [
  'http://localhost:3000',
  'https://eduspry.com',
  'https://app.eduspry.com',
];

export function cors(req: NextRequest) {
  const origin = req.headers.get('origin');
  
  if (origin && !ALLOWED_ORIGINS.includes(origin)) {
    return NextResponse.json(
      { error: 'CORS policy violation' },
      { status: 403 }
    );
  }

  return null; // Allow request
}
```

---

## Deployment & DevOps

### Docker Configuration

#### Dockerfile
```dockerfile
# Dockerfile
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

FROM node:18-alpine AS runner

WORKDIR /app

ENV NODE_ENV production

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs

EXPOSE 3000

ENV PORT 3000

CMD ["node", "server.js"]
```

#### Docker Compose for Development
```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
      - NEXT_PUBLIC_SUPABASE_URL=${NEXT_PUBLIC_SUPABASE_URL}
      - NEXT_PUBLIC_SUPABASE_ANON_KEY=${NEXT_PUBLIC_SUPABASE_ANON_KEY}
    volumes:
      - .:/app
      - /app/node_modules
    depends_on:
      - redis

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

volumes:
  redis_data:
```

### CI/CD Pipeline

#### GitHub Actions Workflow
```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    name: Test
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run linting
        run: npm run lint
      
      - name: Run type checking
        run: npm run type-check
      
      - name: Run unit tests
        run: npm run test
      
      - name: Run E2E tests
        run: npm run test:e2e

  build:
    name: Build
    runs-on: ubuntu-latest
    needs: test
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build application
        run: npm run build
      
      - name: Build Docker image
        run: docker build -t eduspry-platform:${{ github.sha }} .

  deploy:
    name: Deploy
    runs-on: ubuntu-latest
    needs: [test, build]
    if: github.ref == 'refs/heads/main'
    
    steps:
      - name: Deploy to production
        run: |
          echo "Deploy to production server"
          # Add actual deployment steps
```

### Environment Management

#### Production Configuration
```typescript
// next.config.ts
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  experimental: {
    serverActions: {
      allowedOrigins: ['localhost:3000', 'eduspry.com'],
    },
  },
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'supabase.co',
      },
    ],
  },
  async headers() {
    return [
      {
        source: '/api/:path*',
        headers: [
          { key: 'X-Frame-Options', value: 'DENY' },
          { key: 'X-Content-Type-Options', value: 'nosniff' },
          { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
        ],
      },
    ];
  },
};

export default nextConfig;
```

---

## Monitoring and Observability

### Application Monitoring

#### Custom Analytics
```typescript
// lib/analytics/tracker.ts
interface AnalyticsEvent {
  event: string;
  properties?: Record<string, any>;
  userId?: string;
  tenantId?: string;
}

class AnalyticsTracker {
  private queue: AnalyticsEvent[] = [];
  private flushInterval: number = 5000; // 5 seconds

  constructor() {
    if (typeof window !== 'undefined') {
      setInterval(() => this.flush(), this.flushInterval);
    }
  }

  track(event: string, properties?: Record<string, any>) {
    this.queue.push({
      event,
      properties: {
        ...properties,
        timestamp: new Date().toISOString(),
        url: window.location.href,
        userAgent: navigator.userAgent,
      },
    });
  }

  private async flush() {
    if (this.queue.length === 0) return;

    const events = [...this.queue];
    this.queue = [];

    try {
      await fetch('/api/analytics/events', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ events }),
      });
    } catch (error) {
      // Re-queue events on failure
      this.queue.unshift(...events);
    }
  }
}

export const analytics = new AnalyticsTracker();
```

#### Error Tracking
```typescript
// lib/error-tracking/error-boundary.tsx
"use client";

import { Component, ReactNode } from 'react';

interface ErrorBoundaryState {
  hasError: boolean;
  error?: Error;
}

interface ErrorBoundaryProps {
  children: ReactNode;
  fallback?: ReactNode;
}

export class ErrorBoundary extends Component<ErrorBoundaryProps, ErrorBoundaryState> {
  constructor(props: ErrorBoundaryProps) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error): ErrorBoundaryState {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: any) {
    // Log error to monitoring service
    console.error('Error caught by boundary:', error, errorInfo);
    
    // Send to error tracking service
    this.logErrorToService(error, errorInfo);
  }

  private async logErrorToService(error: Error, errorInfo: any) {
    try {
      await fetch('/api/errors', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          message: error.message,
          stack: error.stack,
          componentStack: errorInfo.componentStack,
          url: window.location.href,
          timestamp: new Date().toISOString(),
        }),
      });
    } catch (e) {
      console.error('Failed to log error:', e);
    }
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback || (
        <div className="p-4 bg-red-50 border border-red-200 rounded">
          <h2 className="text-red-800 font-semibold">Something went wrong</h2>
          <p className="text-red-600">We're working to fix this issue.</p>
        </div>
      );
    }

    return this.props.children;
  }
}
```

---

## Logging and Observability

EduSpry's logging and observability infrastructure provides comprehensive visibility into all aspects of the platform. For a detailed implementation guide, refer to the [Logging Framework Strategy](./Logging_Framework_Strategy.md) document.

### Logging Implementation

#### Client Logger Setup
```typescript
// lib/logging/client-logger.ts
import { LogLevel, LogContext } from '@eduspry/types';

interface ClientLoggerOptions {
  service: string;
  defaultContext?: Record<string, any>;
  batchSize?: number;
  flushInterval?: number;
}

export class ClientLogger {
  private logQueue: any[] = [];
  private timer: any = null;
  private options: Required<ClientLoggerOptions>;
  
  constructor(options: ClientLoggerOptions) {
    this.options = {
      service: options.service,
      defaultContext: options.defaultContext || {},
      batchSize: options.batchSize || 10,
      flushInterval: options.flushInterval || 5000
    };
    
    this.setupFlushInterval();
    this.setupPageVisibilityListener();
  }

  private setupFlushInterval() {
    this.timer = setInterval(() => this.flush(), this.options.flushInterval);
  }
  
  private setupPageVisibilityListener() {
    if (typeof document !== 'undefined') {
      document.addEventListener('visibilitychange', () => {
        if (document.visibilityState === 'hidden') {
          this.flush();
        }
      });
    }
  }
  
  private async flush() {
    if (this.logQueue.length === 0) return;
    
    const logs = [...this.logQueue];
    this.logQueue = [];
    
    try {
      await fetch('/api/logs', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ logs }),
      });
    } catch (error) {
      console.error('Failed to send logs:', error);
      // Add logs back to the queue if sending fails
      this.logQueue = [...logs, ...this.logQueue];
      // Limit queue size to prevent memory issues
      if (this.logQueue.length > 100) {
        this.logQueue = this.logQueue.slice(-100);
      }
    }
  }
  
  private log(level: LogLevel, message: string, context?: LogContext) {
    const logEntry = {
      timestamp: new Date().toISOString(),
      level,
      service: this.options.service,
      message,
      context: { ...this.options.defaultContext, ...context },
      clientInfo: {
        url: window.location.href,
        userAgent: navigator.userAgent,
        screenWidth: window.innerWidth,
        screenHeight: window.innerHeight,
      }
    };
    
    // Always log to console in development
    if (process.env.NODE_ENV === 'development') {
      console[level.toLowerCase()](message, logEntry);
    }
    
    this.logQueue.push(logEntry);
    
    if (this.logQueue.length >= this.options.batchSize) {
      this.flush();
    }
  }
  
  info(message: string, context?: LogContext) {
    this.log('INFO', message, context);
  }
  
  warn(message: string, context?: LogContext) {
    this.log('WARN', message, context);
  }
  
  error(message: string, context?: LogContext) {
    this.log('ERROR', message, context);
  }
  
  debug(message: string, context?: LogContext) {
    this.log('DEBUG', message, context);
  }
}

export const createClientLogger = (options: ClientLoggerOptions) => {
  return new ClientLogger(options);
};
```

#### Server-Side Logger
```typescript
// lib/logging/server-logger.ts
import { createLogger, format, transports } from 'winston';
import { LogLevel, LogContext } from '@eduspry/types';

interface ServerLoggerOptions {
  service: string;
  defaultContext?: Record<string, any>;
  logLevel?: LogLevel;
}

export const createServerLogger = (options: ServerLoggerOptions) => {
  const logger = createLogger({
    level: options.logLevel?.toLowerCase() || 'info',
    format: format.combine(
      format.timestamp(),
      format.json()
    ),
    defaultMeta: { 
      service: options.service,
      context: options.defaultContext || {}
    },
    transports: [
      new transports.Console({
        format: process.env.NODE_ENV === 'development' 
          ? format.combine(format.colorize(), format.simple())
          : format.json()
      })
    ]
  });
  
  // Add transport for production environment
  if (process.env.NODE_ENV === 'production') {
    // Custom transport to send logs to our centralized logging service
    logger.add(new transports.Http({
      host: process.env.LOGGING_SERVICE_HOST,
      port: parseInt(process.env.LOGGING_SERVICE_PORT || '443'),
      path: '/api/logs',
      ssl: true,
    }));
  }
  
  return logger;
};
```

### Monitoring Setup

#### Health Check Endpoints
```typescript
// app/api/health/route.ts
import { NextResponse } from 'next/server';
import { db } from '@/lib/db';

export async function GET() {
  try {
    // Check database connection
    await db.raw('SELECT 1');
    
    // Check Redis if used
    // const redisClient = await getRedisClient();
    // await redisClient.ping();
    
    return NextResponse.json({
      status: 'healthy',
      timestamp: new Date().toISOString(),
      services: {
        database: 'up',
        // redis: 'up',
      }
    }, { status: 200 });
  } catch (error) {
    console.error('Health check failed:', error);
    
    return NextResponse.json({
      status: 'unhealthy',
      timestamp: new Date().toISOString(),
      error: error.message
    }, { status: 500 });
  }
}
```

#### Performance Monitoring
```typescript
// middleware.ts
import { NextRequest, NextResponse } from 'next/server';
import { performance } from 'perf_hooks';
import { serverLogger } from '@/lib/logging/server-logger';

export async function middleware(request: NextRequest) {
  const start = performance.now();
  const requestId = crypto.randomUUID();
  
  // Store the start time and request ID
  const response = NextResponse.next({
    headers: {
      'X-Request-ID': requestId,
    },
  });
  
  // Process the request
  const result = await response;
  
  // Calculate duration
  const duration = performance.now() - start;
  
  // Log the request metrics
  serverLogger.info('API request processed', {
    requestId,
    method: request.method,
    path: request.nextUrl.pathname,
    statusCode: result.status,
    durationMs: duration.toFixed(2),
    userAgent: request.headers.get('user-agent') || 'unknown',
  });
  
  // Add timing headers to the response
  result.headers.set('X-Response-Time', duration.toFixed(2));
  
  return result;
}

export const config = {
  matcher: '/api/:path*',
};
```

For comprehensive details on our logging architecture, including standardized log formats, log collection, storage strategies, AI-assisted log analysis, and cloud-agnostic implementations, see the [Logging Framework Strategy](./Logging_Framework_Strategy.md) documentation.

---

## Microservices Architecture

### Code Organization

The EduSpry platform follows a monorepo structure for microservices organization:

```
eduspry-platform/
├── services/                  # All microservices
│   ├── auth-service/          # Authentication microservice
│   ├── course-service/        # Course management 
│   ├── logging-service/       # Centralized logging service
│   └── analytics-service/     # Analytics & reporting
├── packages/                  # Shared libraries and utilities
├── infrastructure/            # IaC and deployment configurations
├── supabase/                  # Supabase configuration and edge functions
└── apps/                      # Frontend applications
```

### Hybrid Implementation Approach

The platform employs a hybrid approach for service implementation:

1. **Containerized Microservices**: Core services with complex business logic
2. **Supabase Edge Functions**: Lightweight, globally distributed services
3. **Shared Backend Services**: Leveraging Supabase managed backend

### Cloud Portability Design

The architecture ensures portability across cloud providers (AWS, Azure, GCP, OCI):

```typescript
// Example of cloud-agnostic storage abstraction
// lib/storage/index.ts

import { StorageProvider } from './interfaces';
import { SupabaseStorageProvider } from './providers/supabase';
import { AWSS3Provider } from './providers/aws-s3';
import { AzureBlobProvider } from './providers/azure-blob';

// Factory function to create the appropriate storage provider
export function createStorageProvider(): StorageProvider {
  const provider = process.env.STORAGE_PROVIDER || 'supabase';
  
  switch (provider) {
    case 'aws':
      return new AWSS3Provider({
        region: process.env.AWS_REGION,
        accessKey: process.env.AWS_ACCESS_KEY,
        secretKey: process.env.AWS_SECRET_KEY,
      });
    case 'azure':
      return new AzureBlobProvider({
        connectionString: process.env.AZURE_STORAGE_CONNECTION_STRING,
      });
    case 'supabase':
    default:
      return new SupabaseStorageProvider({
        url: process.env.SUPABASE_URL,
        key: process.env.SUPABASE_KEY,
      });
  }
}
```

### Supabase Edge Functions Example

```typescript
// Example edge function for logging events
import { serve } from 'https://deno.land/std@0.131.0/http/server.ts';
import { createClient } from 'https://esm.sh/@supabase/supabase-js@2.0.0';

serve(async (req) => {
  const supabase = createClient(
    Deno.env.get('SUPABASE_URL') as string,
    Deno.env.get('SUPABASE_SERVICE_ROLE_KEY') as string
  );
  
  try {
    const { events, tenantId } = await req.json();
    
    // Process and store logs
    const { data, error } = await supabase
      .from('event_logs')
      .insert(events.map(event => ({
        tenant_id: tenantId,
        event_type: event.type,
        payload: event.data,
        timestamp: new Date().toISOString()
      })));
      
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

For more detailed information, see the [Microservices Implementation Guide](./Microservices_Implementation_Guide.md).

---

*This technical documentation provides the foundation for implementing the EduSpry platform. Each section should be expanded with specific implementation details as development progresses.*
