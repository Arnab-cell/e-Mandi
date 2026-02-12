# e-Mandi - System Design Document

## Document Information

**Project Name:** e-Mandi - Digital Agricultural Marketplace  
**Version:** 1.0  
**Last Updated:** February 12, 2026  
**Status:** Active Development

---

## Table of Contents

1. [System Overview](#1-system-overview)
2. [System Architecture](#2-system-architecture)
3. [Technology Stack](#3-technology-stack)
4. [Database Design](#4-database-design)
5. [Component Architecture](#5-component-architecture)
6. [Authentication & Authorization](#6-authentication--authorization)
7. [API Design](#7-api-design)
8. [State Management](#8-state-management)
9. [Routing Architecture](#9-routing-architecture)
10. [Security Architecture](#10-security-architecture)
11. [Deployment Architecture](#11-deployment-architecture)

---

## 1. System Overview

### 1.1 Purpose
e-Mandi is a digital marketplace platform designed to connect farmers directly with buyers, eliminating intermediaries and enabling transparent agricultural produce transactions.

### 1.2 Architecture Style
- **Frontend:** Single Page Application (SPA) with React
- **Backend:** Backend-as-a-Service (BaaS) using Supabase
- **Database:** PostgreSQL with Row Level Security (RLS)
- **Architecture Pattern:** Client-Server with RESTful API communication

### 1.3 Key Design Principles
- Component-based architecture for reusability
- Type-safe development with TypeScript
- Responsive and mobile-first design
- Security-first approach with RLS policies
- Real-time data synchronization
- Optimistic UI updates for better UX

---

## 2. System Architecture

### 2.1 High-Level Architecture


```
┌─────────────────────────────────────────────────────────────────┐
│                         Client Layer                             │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │              React SPA (Vite + TypeScript)               │   │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────────────┐ │   │
│  │  │   Pages    │  │ Components │  │  Custom Hooks      │ │   │
│  │  │  - Index   │  │  - Farmer  │  │  - useAuth         │ │   │
│  │  │  - Auth    │  │  - Buyer   │  │  - use-toast       │ │   │
│  │  │  - Dashboard│  │  - Admin   │  │  - use-mobile      │ │   │
│  │  └────────────┘  └────────────┘  └────────────────────┘ │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              ↕ HTTPS
┌─────────────────────────────────────────────────────────────────┐
│                    Supabase Backend Layer                        │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                  Supabase Services                       │   │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────────────┐ │   │
│  │  │    Auth    │  │  Database  │  │   Storage (Future) │ │   │
│  │  │   - JWT    │  │ PostgreSQL │  │   - Images         │ │   │
│  │  │   - RLS    │  │   - RLS    │  │   - Documents      │ │   │
│  │  └────────────┘  └────────────┘  └────────────────────┘ │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────────┐
│                      Data Persistence Layer                      │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │              PostgreSQL Database                         │   │
│  │  ┌────────────┐  ┌────────────┐  ┌────────────────────┐ │   │
│  │  │  Profiles  │  │   Crops    │  │      Orders        │ │   │
│  │  │   Table    │  │   Table    │  │      Table         │ │   │
│  │  └────────────┘  └────────────┘  └────────────────────┘ │   │
│  └──────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### 2.2 Component Interaction Flow

```
User Action → React Component → Supabase Client → Supabase API
                    ↓                                    ↓
              State Update ← React Query Cache ← Database Response
                    ↓
              UI Re-render
```

### 2.3 Data Flow Architecture

**Read Operations:**
1. User interacts with UI component
2. Component calls Supabase client method
3. Supabase validates JWT token
4. RLS policies filter data based on user role
5. Data returned to client
6. React Query caches response
7. Component renders updated data

**Write Operations:**
1. User submits form/action
2. Client-side validation (Zod schema)
3. Supabase client sends mutation request
4. Server validates JWT and RLS policies
5. Database transaction executed
6. Success/error response returned
7. React Query invalidates relevant cache
8. UI updates with toast notification

---

## 3. Technology Stack

### 3.1 Frontend Technologies

#### Core Framework
- **React 18.3.1** - UI library for building component-based interfaces
- **TypeScript 5.5.3** - Type-safe JavaScript superset
- **Vite 5.4.1** - Fast build tool and development server
- **React Router DOM 6.26.2** - Client-side routing

#### UI Framework & Styling
- **Tailwind CSS 3.4.11** - Utility-first CSS framework
- **shadcn/ui** - Accessible component library built on Radix UI
- **Radix UI** - Unstyled, accessible component primitives
  - @radix-ui/react-dialog
  - @radix-ui/react-dropdown-menu
  - @radix-ui/react-select
  - @radix-ui/react-tabs
  - @radix-ui/react-toast
  - And 20+ other component primitives
- **Lucide React 0.462.0** - Icon library
- **tailwindcss-animate** - Animation utilities

#### State Management & Data Fetching
- **@tanstack/react-query 5.56.2** - Server state management and caching
- **React Context API** - Global state for authentication

#### Form Handling & Validation
- **React Hook Form 7.53.0** - Performant form library
- **Zod 3.23.8** - TypeScript-first schema validation
- **@hookform/resolvers 3.9.0** - Validation resolver for React Hook Form

#### Additional Libraries
- **date-fns 3.6.0** - Date utility library
- **class-variance-authority 0.7.1** - CSS class composition
- **clsx 2.1.1** - Conditional className utility
- **tailwind-merge 2.5.2** - Merge Tailwind classes intelligently
- **sonner 1.5.0** - Toast notification system
- **recharts 2.12.7** - Charting library for analytics

### 3.2 Backend Technologies

#### Backend-as-a-Service
- **Supabase 2.52.1** - Complete backend solution
  - PostgreSQL database
  - Authentication & authorization
  - Row Level Security (RLS)
  - RESTful API auto-generation
  - Real-time subscriptions (future)

#### Database
- **PostgreSQL** - Relational database
  - ACID compliance
  - Advanced indexing
  - Full-text search capabilities
  - JSON support

### 3.3 Development Tools

#### Build & Development
- **Vite** - Development server with HMR
- **@vitejs/plugin-react-swc** - Fast React refresh with SWC compiler
- **PostCSS 8.4.47** - CSS transformation
- **Autoprefixer 10.4.20** - CSS vendor prefixing

#### Code Quality
- **ESLint 9.9.0** - JavaScript/TypeScript linter
- **TypeScript ESLint 8.0.1** - TypeScript-specific linting rules
- **eslint-plugin-react-hooks** - React Hooks linting
- **eslint-plugin-react-refresh** - React Fast Refresh linting

#### Type Definitions
- **@types/react 18.3.3**
- **@types/react-dom 18.3.0**
- **@types/node 22.5.5**

### 3.4 Configuration Files

```typescript
// vite.config.ts - Build configuration
{
  server: { host: "::", port: 8080 },
  plugins: [react(), componentTagger()],
  resolve: { alias: { "@": "./src" } }
}

// tailwind.config.ts - Styling configuration
{
  darkMode: ["class"],
  theme: { extend: { colors, animations, keyframes } }
}

// tsconfig.json - TypeScript configuration
{
  paths: { "@/*": ["./src/*"] },
  strictNullChecks: false
}
```

---

## 4. Database Design

### 4.1 Database Schema Overview

The database consists of three main tables with relationships enforced through foreign keys and protected by Row Level Security policies.

### 4.2 Entity Relationship Diagram

```
┌─────────────────────┐
│   auth.users        │
│  (Supabase Auth)    │
│─────────────────────│
│ id (UUID) PK        │
│ email               │
│ encrypted_password  │
│ created_at          │
└─────────────────────┘
          │
          │ 1:1
          ↓
┌─────────────────────┐
│   profiles          │
│─────────────────────│
│ user_id (UUID) PK   │───────┐
│ email               │       │
│ full_name           │       │
│ user_type (ENUM)    │       │
│ phone               │       │
│ address             │       │
│ city                │       │
│ state               │       │
│ pincode             │       │
│ created_at          │       │
│ updated_at          │       │
└─────────────────────┘       │
          │                   │
          │ 1:N               │
          ↓                   │
┌─────────────────────┐       │
│   crops             │       │
│─────────────────────│       │
│ id (UUID) PK        │       │
│ farmer_id (UUID) FK │───────┘
│ name                │
│ category (ENUM)     │
│ description         │
│ quantity_available  │
│ unit                │
│ price_per_unit      │
│ harvest_date        │
│ expiry_date         │
│ location            │
│ image_url           │
│ is_available        │
│ is_organic          │
│ created_at          │
│ updated_at          │
└─────────────────────┘
          │
          │ 1:N
          ↓
┌─────────────────────┐
│   orders            │
│─────────────────────│
│ id (UUID) PK        │
│ buyer_id (UUID) FK  │───────┐
│ farmer_id (UUID) FK │───────┤
│ crop_id (UUID) FK   │───────┘
│ quantity            │
│ total_price         │
│ status (ENUM)       │
│ delivery_address    │
│ delivery_date       │
│ notes               │
│ created_at          │
│ updated_at          │
└─────────────────────┘
```


### 4.3 Table Specifications

#### 4.3.1 profiles Table

```sql
CREATE TABLE public.profiles (
  user_id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  email TEXT UNIQUE NOT NULL,
  full_name TEXT NOT NULL,
  user_type user_type DEFAULT 'buyer',
  phone TEXT,
  address TEXT,
  city TEXT,
  state TEXT,
  pincode TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT now(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT now()
);
```

**Purpose:** Stores extended user profile information beyond authentication data.

**Key Fields:**
- `user_id`: Links to Supabase auth.users table
- `user_type`: ENUM ('farmer', 'buyer', 'admin') - determines user role
- `phone`, `address`, `city`, `state`, `pincode`: Contact and location information

**Indexes:**
- Primary key on `user_id`
- Unique constraint on `email`

#### 4.3.2 crops Table

```sql
CREATE TABLE public.crops (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  farmer_id UUID NOT NULL REFERENCES public.profiles(user_id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  category crop_category NOT NULL,
  description TEXT,
  quantity_available DECIMAL(10,2) NOT NULL DEFAULT 0,
  unit TEXT NOT NULL DEFAULT 'kg',
  price_per_unit DECIMAL(10,2) NOT NULL,
  harvest_date DATE,
  expiry_date DATE,
  location TEXT,
  image_url TEXT,
  is_available BOOLEAN DEFAULT true,
  is_organic BOOLEAN DEFAULT false,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT now(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT now()
);
```

**Purpose:** Stores agricultural produce listings created by farmers.

**Key Fields:**
- `farmer_id`: Foreign key to profiles table
- `category`: ENUM ('vegetables', 'fruits', 'grains', 'pulses', 'spices', 'dairy', 'other')
- `quantity_available`: Decimal for precise quantity tracking
- `price_per_unit`: Decimal for accurate pricing
- `is_available`: Boolean flag for active/inactive listings

**Indexes:**
- `idx_crops_farmer_id` on `farmer_id`
- `idx_crops_category` on `category`
- `idx_crops_available` on `is_available`

**Triggers:**
- `update_crops_updated_at`: Automatically updates `updated_at` on row modification

#### 4.3.3 orders Table

```sql
CREATE TABLE public.orders (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  buyer_id UUID NOT NULL REFERENCES public.profiles(user_id) ON DELETE CASCADE,
  farmer_id UUID NOT NULL REFERENCES public.profiles(user_id) ON DELETE CASCADE,
  crop_id UUID NOT NULL REFERENCES public.crops(id) ON DELETE CASCADE,
  quantity DECIMAL(10,2) NOT NULL,
  total_price DECIMAL(10,2) NOT NULL,
  status order_status DEFAULT 'pending',
  delivery_address TEXT NOT NULL,
  delivery_date DATE,
  notes TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT now(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT now()
);
```

**Purpose:** Tracks purchase orders between buyers and farmers.

**Key Fields:**
- `buyer_id`, `farmer_id`, `crop_id`: Foreign keys establishing relationships
- `status`: ENUM ('pending', 'confirmed', 'shipped', 'delivered', 'cancelled')
- `total_price`: Calculated as quantity × price_per_unit
- `delivery_address`: Buyer's delivery location

**Indexes:**
- `idx_orders_buyer_id` on `buyer_id`
- `idx_orders_farmer_id` on `farmer_id`
- `idx_orders_status` on `status`

**Triggers:**
- `update_orders_updated_at`: Automatically updates `updated_at` on row modification

### 4.4 Enumerations

```sql
-- User role types
CREATE TYPE public.user_type AS ENUM ('farmer', 'buyer', 'admin');

-- Order lifecycle status
CREATE TYPE public.order_status AS ENUM (
  'pending',    -- Order placed, awaiting farmer confirmation
  'confirmed',  -- Farmer confirmed the order
  'shipped',    -- Order dispatched for delivery
  'delivered',  -- Order successfully delivered
  'cancelled'   -- Order cancelled by buyer or farmer
);

-- Agricultural product categories
CREATE TYPE public.crop_category AS ENUM (
  'vegetables',
  'fruits',
  'grains',
  'pulses',
  'spices',
  'dairy',
  'other'
);
```

### 4.5 Row Level Security (RLS) Policies

#### Crops Table Policies

```sql
-- SELECT: Anyone can view available crops
CREATE POLICY "Anyone can view available crops" 
ON public.crops FOR SELECT 
USING (is_available = true);

-- INSERT: Farmers can create their own crops
CREATE POLICY "Farmers can create their own crops" 
ON public.crops FOR INSERT 
WITH CHECK (auth.uid() = farmer_id);

-- UPDATE: Farmers can update their own crops
CREATE POLICY "Farmers can update their own crops" 
ON public.crops FOR UPDATE 
USING (auth.uid() = farmer_id);

-- DELETE: Farmers can delete their own crops
CREATE POLICY "Farmers can delete their own crops" 
ON public.crops FOR DELETE 
USING (auth.uid() = farmer_id);
```

#### Orders Table Policies

```sql
-- SELECT: Users can view their own orders (as buyer or farmer)
CREATE POLICY "Users can view their own orders" 
ON public.orders FOR SELECT 
USING (auth.uid() = buyer_id OR auth.uid() = farmer_id);

-- INSERT: Buyers can create orders
CREATE POLICY "Buyers can create orders" 
ON public.orders FOR INSERT 
WITH CHECK (auth.uid() = buyer_id);

-- UPDATE: Both buyers and farmers can update orders
CREATE POLICY "Users can update their own orders" 
ON public.orders FOR UPDATE 
USING (auth.uid() = buyer_id OR auth.uid() = farmer_id);
```

### 4.6 Database Relationships

**One-to-Many Relationships:**
- `profiles` → `crops`: One farmer can list many crops
- `profiles` → `orders` (as buyer): One buyer can place many orders
- `profiles` → `orders` (as farmer): One farmer can receive many orders
- `crops` → `orders`: One crop can have many orders

**Cascade Behavior:**
- Deleting a user cascades to their profile, crops, and orders
- Deleting a crop cascades to its orders
- Ensures referential integrity

### 4.7 Data Integrity Constraints

- **NOT NULL constraints** on critical fields (name, category, price, quantity)
- **DEFAULT values** for timestamps, booleans, and enums
- **Foreign key constraints** with CASCADE DELETE
- **DECIMAL precision** (10,2) for monetary and quantity values
- **Automatic timestamp management** via triggers

---

## 5. Component Architecture

### 5.1 Component Hierarchy

```
App.tsx (Root)
├── AuthProvider (Context)
├── QueryClientProvider (React Query)
├── TooltipProvider (UI)
├── Toaster (Notifications)
└── BrowserRouter (Routing)
    └── Routes
        ├── Index (Landing Page)
        ├── Auth (Login/Signup)
        ├── Dashboard (Protected)
        │   ├── FarmerDashboard
        │   │   ├── AppHeader
        │   │   ├── Stats Cards
        │   │   ├── CropCard (multiple)
        │   │   ├── OrdersTable
        │   │   └── AddCropDialog
        │   └── BuyerDashboard
        │       ├── AppHeader
        │       ├── Stats Cards
        │       ├── Search & Filters
        │       ├── CropCard (multiple)
        │       └── OrdersTable
        ├── AdminDashboard (Protected)
        │   ├── AppHeader
        │   ├── Analytics Cards
        │   └── Tabs
        │       ├── Users Management
        │       ├── Crops Management
        │       └── Orders Management
        └── NotFound (404)
```

### 5.2 Component Categories

#### 5.2.1 Page Components (`src/pages/`)
- **Index.tsx**: Landing page with hero section and features
- **Auth.tsx**: Authentication page with login/signup tabs
- **Dashboard.tsx**: Route handler that renders role-specific dashboard
- **AdminDashboard.tsx**: Admin control panel with analytics
- **NotFound.tsx**: 404 error page

#### 5.2.2 Feature Components (`src/components/`)
- **FarmerDashboard.tsx**: Complete farmer interface
- **BuyerDashboard.tsx**: Complete buyer interface
- **CropCard.tsx**: Reusable crop display card with actions
- **OrdersTable.tsx**: Reusable order list with status management
- **AddCropDialog.tsx**: Modal form for creating crop listings
- **AppHeader.tsx**: Navigation header component
- **UserTypeSelector.tsx**: Role selection component

#### 5.2.3 UI Components (`src/components/ui/`)
Shadcn/ui components (50+ components):
- Form elements: Button, Input, Textarea, Select, Checkbox, Radio
- Layout: Card, Tabs, Dialog, Sheet, Accordion
- Feedback: Toast, Alert, Progress, Skeleton
- Navigation: Dropdown Menu, Context Menu, Navigation Menu
- Data Display: Table, Badge, Avatar, Tooltip
- And many more...

### 5.3 Component Design Patterns

#### Composition Pattern
```typescript
<Card>
  <CardHeader>
    <CardTitle>Title</CardTitle>
  </CardHeader>
  <CardContent>
    Content
  </CardContent>
</Card>
```

#### Render Props Pattern
```typescript
<Select onValueChange={handleChange}>
  <SelectTrigger>
    <SelectValue />
  </SelectTrigger>
  <SelectContent>
    {items.map(item => <SelectItem key={item.id} />)}
  </SelectContent>
</Select>
```

#### Custom Hooks Pattern
```typescript
const { user, signIn, signOut } = useAuth();
const { toast } = useToast();
const isMobile = useMobile();
```

### 5.4 Component Communication

**Props Down:**
- Parent components pass data and callbacks to children
- Type-safe props with TypeScript interfaces

**Events Up:**
- Child components emit events via callback props
- Parent handles state updates

**Context for Global State:**
- AuthContext for user authentication state
- Shared across entire component tree

**React Query for Server State:**
- Automatic caching and synchronization
- Optimistic updates for better UX

---

## 6. Authentication & Authorization

### 6.1 Authentication Flow

```
┌─────────────┐
│   User      │
└──────┬──────┘
       │
       ↓ (Email + Password)
┌─────────────────────────┐
│  Auth.tsx Component     │
│  - Sign Up Form         │
│  - Sign In Form         │
└──────┬──────────────────┘
       │
       ↓ useAuth Hook
┌─────────────────────────┐
│  AuthProvider Context   │
│  - signUp()             │
│  - signIn()             │
│  - signOut()            │
└──────┬──────────────────┘
       │
       ↓ Supabase Client
┌─────────────────────────┐
│  Supabase Auth Service  │
│  - Create User          │
│  - Verify Credentials   │
│  - Generate JWT         │
└──────┬──────────────────┘
       │
       ↓ JWT Token
┌─────────────────────────┐
│  Client Storage         │
│  - localStorage         │
│  - Auto Refresh Token   │
└─────────────────────────┘
```


### 6.2 Authentication Implementation

#### Supabase Client Configuration

```typescript
// src/integrations/supabase/client.ts
export const supabase = createClient<Database>(
  SUPABASE_URL, 
  SUPABASE_PUBLISHABLE_KEY,
  {
    auth: {
      storage: localStorage,        // Persist session
      persistSession: true,          // Maintain login across refreshes
      autoRefreshToken: true,        // Automatic token renewal
    }
  }
);
```

#### Auth Context Provider

```typescript
// src/hooks/useAuth.tsx
export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);
  const [session, setSession] = useState<Session | null>(null);
  
  useEffect(() => {
    // Listen for auth state changes
    const { data: { subscription } } = supabase.auth.onAuthStateChange(
      (event, session) => {
        setSession(session);
        setUser(session?.user ?? null);
      }
    );
    
    // Check existing session
    supabase.auth.getSession().then(({ data: { session } }) => {
      setSession(session);
      setUser(session?.user ?? null);
    });
    
    return () => subscription.unsubscribe();
  }, []);
  
  return (
    <AuthContext.Provider value={{ user, session, signUp, signIn, signOut }}>
      {children}
    </AuthContext.Provider>
  );
};
```

### 6.3 Authorization Mechanisms

#### Role-Based Access Control (RBAC)

**User Roles:**
1. **Farmer**: Can create/edit crops, manage orders received
2. **Buyer**: Can browse crops, place orders, track purchases
3. **Admin**: Full platform access, user/crop/order management

#### Route Protection

```typescript
// Protected Route Component
const ProtectedRoute = ({ children }) => {
  const { user, loading } = useAuth();
  
  if (loading) return <LoadingScreen />;
  if (!user) return <Navigate to="/auth" replace />;
  
  return <>{children}</>;
};

// Usage in routing
<Route 
  path="/dashboard" 
  element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  } 
/>
```

#### Database-Level Authorization (RLS)

Row Level Security policies ensure:
- Users only see their own data
- Farmers can only modify their own crops
- Buyers can only create orders for themselves
- Admin access controlled by email verification

### 6.4 Session Management

**Token Lifecycle:**
1. JWT token issued on successful login
2. Token stored in localStorage
3. Token included in all API requests (Authorization header)
4. Automatic refresh before expiration
5. Token cleared on logout

**Session Persistence:**
- Sessions persist across browser refreshes
- Automatic re-authentication on app load
- Secure token storage in localStorage

---

## 7. API Design

### 7.1 API Architecture

e-Mandi uses Supabase's auto-generated RESTful API based on PostgreSQL schema.

**Base URL:** `https://pizkmnmscixczavtqiwk.supabase.co`

**Authentication:** Bearer token in Authorization header

### 7.2 API Endpoints

#### Authentication Endpoints

```
POST /auth/v1/signup
POST /auth/v1/token?grant_type=password
POST /auth/v1/logout
GET  /auth/v1/user
```

#### Profiles Endpoints

```
GET    /rest/v1/profiles?user_id=eq.{id}
POST   /rest/v1/profiles
PATCH  /rest/v1/profiles?user_id=eq.{id}
DELETE /rest/v1/profiles?user_id=eq.{id}
```

#### Crops Endpoints

```
GET    /rest/v1/crops?select=*
GET    /rest/v1/crops?farmer_id=eq.{id}
GET    /rest/v1/crops?category=eq.{category}
GET    /rest/v1/crops?is_available=eq.true
POST   /rest/v1/crops
PATCH  /rest/v1/crops?id=eq.{id}
DELETE /rest/v1/crops?id=eq.{id}
```

#### Orders Endpoints

```
GET    /rest/v1/orders?buyer_id=eq.{id}
GET    /rest/v1/orders?farmer_id=eq.{id}
GET    /rest/v1/orders?status=eq.{status}
POST   /rest/v1/orders
PATCH  /rest/v1/orders?id=eq.{id}
```

### 7.3 API Request Examples

#### Fetch Available Crops

```typescript
const { data, error } = await supabase
  .from('crops')
  .select('*')
  .eq('is_available', true)
  .gt('quantity_available', 0)
  .order('created_at', { ascending: false });
```

#### Create Order

```typescript
const { data, error } = await supabase
  .from('orders')
  .insert({
    buyer_id: userId,
    farmer_id: crop.farmer_id,
    crop_id: cropId,
    quantity: orderQuantity,
    total_price: orderQuantity * crop.price_per_unit,
    delivery_address: address,
    status: 'pending'
  });
```

#### Update Order Status

```typescript
const { error } = await supabase
  .from('orders')
  .update({ status: 'confirmed' })
  .eq('id', orderId);
```

### 7.4 API Response Format

**Success Response:**
```json
{
  "data": [...],
  "error": null,
  "count": 10,
  "status": 200,
  "statusText": "OK"
}
```

**Error Response:**
```json
{
  "data": null,
  "error": {
    "message": "Error message",
    "details": "Detailed error information",
    "hint": "Suggestion to fix",
    "code": "ERROR_CODE"
  }
}
```

### 7.5 API Features

- **Automatic API Generation**: Based on database schema
- **Query Filtering**: Support for eq, neq, gt, lt, like, in, etc.
- **Sorting**: Order by any column, ascending/descending
- **Pagination**: Limit and offset support
- **Joins**: Select related data with foreign keys
- **Full-Text Search**: PostgreSQL text search capabilities
- **Real-time Subscriptions**: WebSocket support (future feature)

---

## 8. State Management

### 8.1 State Management Strategy

e-Mandi uses a hybrid state management approach:

1. **Server State**: React Query (@tanstack/react-query)
2. **Global Client State**: React Context API
3. **Local Component State**: useState, useReducer
4. **Form State**: React Hook Form

### 8.2 React Query Configuration

```typescript
// App.tsx
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 1000 * 60 * 5,      // 5 minutes
      cacheTime: 1000 * 60 * 10,     // 10 minutes
      refetchOnWindowFocus: false,
      retry: 1,
    },
  },
});

<QueryClientProvider client={queryClient}>
  <App />
</QueryClientProvider>
```

### 8.3 State Categories

#### Server State (React Query)
- Crops listings
- Orders data
- User profiles
- Analytics data

**Benefits:**
- Automatic caching
- Background refetching
- Optimistic updates
- Loading and error states
- Request deduplication

#### Global Client State (Context)
- Authentication state (user, session)
- Theme preferences (future)
- Language settings (future)

#### Local Component State
- Form inputs
- UI toggles (modals, dropdowns)
- Search filters
- Pagination state

### 8.4 Data Fetching Patterns

#### Fetch on Mount

```typescript
useEffect(() => {
  fetchCrops();
  fetchOrders();
}, []);
```

#### Optimistic Updates

```typescript
const handleUpdateStatus = async (orderId, status) => {
  // Optimistically update UI
  setOrders(prev => 
    prev.map(order => 
      order.id === orderId ? { ...order, status } : order
    )
  );
  
  // Send request to server
  const { error } = await supabase
    .from('orders')
    .update({ status })
    .eq('id', orderId);
  
  // Revert on error
  if (error) {
    fetchOrders(); // Refetch to get correct state
  }
};
```

#### Cache Invalidation

```typescript
// After creating a crop
onSuccess: () => {
  fetchCrops(); // Refetch to update list
  setShowAddCrop(false);
}
```

---

## 9. Routing Architecture

### 9.1 Route Configuration

```typescript
// App.tsx
<BrowserRouter>
  <Routes>
    <Route path="/" element={<Index />} />
    <Route path="/auth" element={<Auth />} />
    <Route 
      path="/dashboard" 
      element={
        <ProtectedRoute>
          <Dashboard />
        </ProtectedRoute>
      } 
    />
    <Route 
      path="/admin" 
      element={
        <ProtectedRoute>
          <AdminDashboard />
        </ProtectedRoute>
      } 
    />
    <Route path="*" element={<NotFound />} />
  </Routes>
</BrowserRouter>
```

### 9.2 Route Types

#### Public Routes
- `/` - Landing page
- `/auth` - Login/Signup

#### Protected Routes
- `/dashboard` - Role-based dashboard (Farmer/Buyer)
- `/admin` - Admin control panel

#### Fallback Route
- `*` - 404 Not Found page

### 9.3 Navigation Flow

```
Landing Page (/)
    ↓
Auth Page (/auth)
    ↓ (After Login)
Dashboard (/dashboard)
    ├── Farmer Dashboard (if user_type = 'farmer')
    └── Buyer Dashboard (if user_type = 'buyer')

Admin Dashboard (/admin)
    └── Only accessible to admin users
```

### 9.4 Route Guards

**Authentication Guard:**
- Checks if user is logged in
- Redirects to `/auth` if not authenticated
- Shows loading state during auth check

**Role-Based Guard:**
- Dashboard component checks user_type
- Renders appropriate dashboard based on role
- Admin route verifies admin email

---

## 10. Security Architecture

### 10.1 Security Layers

```
┌─────────────────────────────────────────┐
│     Application Layer Security          │
│  - Input Validation (Zod)               │
│  - XSS Prevention                        │
│  - CSRF Protection                       │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│     Authentication Layer                 │
│  - JWT Tokens                            │
│  - Secure Password Hashing (bcrypt)     │
│  - Session Management                    │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│     Authorization Layer                  │
│  - Row Level Security (RLS)              │
│  - Role-Based Access Control             │
│  - Policy Enforcement                    │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│     Database Layer Security              │
│  - Encrypted Connections (SSL/TLS)       │
│  - Parameterized Queries                 │
│  - Foreign Key Constraints               │
└─────────────────────────────────────────┘
```

### 10.2 Security Measures

#### Input Validation
- Client-side validation with Zod schemas
- Server-side validation via RLS policies
- Type checking with TypeScript

#### Authentication Security
- Passwords hashed with bcrypt
- JWT tokens with expiration
- Secure token storage (localStorage with HTTPS)
- Automatic token refresh

#### Authorization Security
- Row Level Security on all tables
- User can only access their own data
- Role-based permissions enforced at DB level

#### Data Protection
- HTTPS for all communications
- SQL injection prevention (parameterized queries)
- XSS prevention (React's built-in escaping)
- CORS configuration

#### API Security
- Bearer token authentication
- Rate limiting (Supabase default)
- Request validation
- Error message sanitization


### 10.3 Security Best Practices Implemented

- ✅ Environment variables for sensitive data
- ✅ No hardcoded credentials in code
- ✅ HTTPS-only communication
- ✅ Secure session management
- ✅ Input sanitization
- ✅ SQL injection prevention
- ✅ XSS protection
- ✅ CSRF tokens (Supabase handles)
- ✅ Role-based access control
- ✅ Audit trails (created_at, updated_at)

---

## 11. Deployment Architecture

### 11.1 Deployment Overview

```
┌─────────────────────────────────────────────────────────┐
│                   CDN / Edge Network                     │
│              (Static Asset Delivery)                     │
└────────────────────┬────────────────────────────────────┘
                     │
                     ↓
┌─────────────────────────────────────────────────────────┐
│              Frontend Hosting Platform                   │
│         (Vercel / Netlify / Railway)                     │
│  ┌───────────────────────────────────────────────────┐  │
│  │         React SPA (Production Build)              │  │
│  │  - Minified JavaScript                            │  │
│  │  - Optimized CSS                                  │  │
│  │  - Compressed Assets                              │  │
│  └───────────────────────────────────────────────────┘  │
└────────────────────┬────────────────────────────────────┘
                     │ HTTPS
                     ↓
┌─────────────────────────────────────────────────────────┐
│                  Supabase Cloud                          │
│  ┌───────────────────────────────────────────────────┐  │
│  │              Supabase Services                    │  │
│  │  - PostgreSQL Database                            │  │
│  │  - Authentication Service                         │  │
│  │  - RESTful API                                    │  │
│  │  - Storage (Future)                               │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

### 11.2 Build Process

#### Development Build

```bash
npm run dev
# Starts Vite dev server on port 8080
# Hot Module Replacement (HMR) enabled
# Source maps for debugging
```

#### Production Build

```bash
npm run build
# 1. TypeScript compilation
# 2. Vite bundling and optimization
# 3. CSS minification
# 4. Asset optimization
# 5. Code splitting
# 6. Tree shaking
# Output: dist/ directory
```

#### Build Configuration

```typescript
// vite.config.ts
export default defineConfig({
  plugins: [react()],
  build: {
    outDir: 'dist',
    sourcemap: false,
    minify: 'terser',
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
          ui: ['@radix-ui/react-*']
        }
      }
    }
  }
});
```

### 11.3 Environment Configuration

#### Environment Variables

```bash
# .env.local (not committed to git)
VITE_SUPABASE_URL=https://pizkmnmscixczavtqiwk.supabase.co
VITE_SUPABASE_ANON_KEY=your_anon_key_here
```

#### Environment-Specific Builds

```bash
# Development
npm run dev

# Production
npm run build

# Development build (with dev mode)
npm run build:dev
```

### 11.4 Hosting Recommendations

#### Frontend Hosting Options

**Option 1: Vercel (Recommended)**
- Automatic deployments from Git
- Edge network for fast delivery
- Preview deployments for PRs
- Environment variable management
- Zero configuration

**Option 2: Netlify**
- Similar features to Vercel
- Built-in form handling
- Split testing capabilities
- Serverless functions

**Option 3: Railway**
- Full-stack deployment
- Database hosting
- Automatic HTTPS
- Custom domains

#### Backend Hosting

**Supabase Cloud (Current)**
- Managed PostgreSQL database
- Built-in authentication
- Automatic backups
- Scalable infrastructure
- Free tier available

### 11.5 Deployment Workflow

```
Developer Push to Git
        ↓
GitHub Repository
        ↓
Automatic Trigger
        ↓
CI/CD Pipeline
    ├── Install Dependencies
    ├── Run Linting
    ├── Run Type Checking
    ├── Build Production Bundle
    └── Run Tests (if available)
        ↓
Deploy to Hosting Platform
        ↓
CDN Distribution
        ↓
Live Application
```

### 11.6 Database Migrations

#### Migration Management

```bash
# Supabase CLI commands
supabase migration new <migration_name>
supabase db push
supabase db reset
```

#### Migration Files

```
supabase/migrations/
├── 20250725154519_initial_schema.sql
└── 20250727084655_add_foreign_keys.sql
```

**Migration Strategy:**
1. Create migration file locally
2. Test in local Supabase instance
3. Commit migration to Git
4. Apply to production via Supabase dashboard or CLI

### 11.7 Monitoring & Logging

#### Application Monitoring
- Error tracking (Sentry - future)
- Performance monitoring (Web Vitals)
- User analytics (Google Analytics - future)

#### Database Monitoring
- Supabase dashboard metrics
- Query performance
- Connection pooling
- Storage usage

#### Logging Strategy
- Console logs in development
- Structured logging in production
- Error boundaries for React errors
- API error logging

### 11.8 Scalability Considerations

#### Frontend Scalability
- Static asset caching via CDN
- Code splitting for faster loads
- Lazy loading of components
- Image optimization
- Gzip/Brotli compression

#### Backend Scalability
- Supabase auto-scaling
- Connection pooling
- Database indexing
- Query optimization
- Caching strategies

#### Performance Optimization
- React.memo for expensive components
- useMemo/useCallback for computations
- Virtual scrolling for large lists
- Debouncing search inputs
- Optimistic UI updates

---

## 12. Design Patterns & Best Practices

### 12.1 React Patterns

#### Component Composition
```typescript
// Composable UI components
<Dialog>
  <DialogTrigger>Open</DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Title</DialogTitle>
    </DialogHeader>
  </DialogContent>
</Dialog>
```

#### Custom Hooks
```typescript
// Reusable logic extraction
const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) throw new Error('useAuth must be used within AuthProvider');
  return context;
};
```

#### Render Props
```typescript
// Flexible component rendering
<FormField
  render={({ field }) => (
    <Input {...field} />
  )}
/>
```

### 12.2 Code Organization

```
src/
├── components/          # Reusable components
│   ├── ui/             # UI primitives (shadcn)
│   ├── FarmerDashboard.tsx
│   ├── BuyerDashboard.tsx
│   └── ...
├── pages/              # Route components
│   ├── Index.tsx
│   ├── Auth.tsx
│   └── Dashboard.tsx
├── hooks/              # Custom React hooks
│   ├── useAuth.tsx
│   └── use-toast.ts
├── integrations/       # External service integrations
│   └── supabase/
│       ├── client.ts
│       └── types.ts
├── lib/                # Utility functions
│   └── utils.ts
├── App.tsx             # Root component
├── main.tsx            # Entry point
└── index.css           # Global styles
```

### 12.3 Naming Conventions

- **Components**: PascalCase (e.g., `FarmerDashboard.tsx`)
- **Hooks**: camelCase with 'use' prefix (e.g., `useAuth.tsx`)
- **Utilities**: camelCase (e.g., `utils.ts`)
- **Constants**: UPPER_SNAKE_CASE
- **Types/Interfaces**: PascalCase with descriptive names

### 12.4 TypeScript Best Practices

```typescript
// Explicit interface definitions
interface CropCardProps {
  crop: Crop;
  isOwner: boolean;
  onUpdate: () => void;
  buyerId?: string;
}

// Type-safe database queries
const { data, error } = await supabase
  .from('crops')
  .select<'*', Crop>('*');

// Enum types for status
type OrderStatus = 'pending' | 'confirmed' | 'shipped' | 'delivered' | 'cancelled';
```

### 12.5 Error Handling

```typescript
// Consistent error handling pattern
try {
  const { data, error } = await supabase
    .from('crops')
    .insert(cropData);
  
  if (error) throw error;
  
  toast({
    title: 'Success',
    description: 'Crop added successfully',
  });
} catch (error) {
  console.error('Error adding crop:', error);
  toast({
    title: 'Error',
    description: 'Failed to add crop',
    variant: 'destructive',
  });
}
```

---

## 13. Future Enhancements

### 13.1 Technical Improvements

#### Performance Optimization
- Implement React Server Components (when stable)
- Add service worker for offline support
- Implement virtual scrolling for large lists
- Add image lazy loading and optimization
- Implement request batching

#### Real-time Features
- WebSocket integration for live updates
- Real-time order notifications
- Live crop availability updates
- Chat functionality between farmers and buyers

#### Advanced Features
- Payment gateway integration (Razorpay/Stripe)
- Geolocation-based search
- Advanced analytics dashboard
- Mobile app (React Native)
- Progressive Web App (PWA)

### 13.2 Infrastructure Improvements

- Implement CI/CD pipeline
- Add automated testing (Jest, React Testing Library)
- Set up error tracking (Sentry)
- Implement performance monitoring
- Add database backup automation
- Set up staging environment

### 13.3 Security Enhancements

- Implement rate limiting
- Add two-factor authentication
- Implement audit logging
- Add data encryption at rest
- Implement CAPTCHA for forms
- Add security headers

---

## 14. Conclusion

### 14.1 Architecture Summary

e-Mandi is built on a modern, scalable architecture that leverages:
- **React + TypeScript** for type-safe, component-based UI
- **Supabase** for managed backend services
- **PostgreSQL** with RLS for secure data management
- **Tailwind CSS + shadcn/ui** for consistent, accessible design
- **React Query** for efficient server state management

### 14.2 Key Strengths

✅ **Type Safety**: Full TypeScript coverage  
✅ **Security**: Multi-layer security with RLS policies  
✅ **Scalability**: Cloud-native architecture  
✅ **Developer Experience**: Modern tooling and hot reload  
✅ **User Experience**: Responsive design and optimistic updates  
✅ **Maintainability**: Clean code organization and patterns  

### 14.3 Design Philosophy

The architecture prioritizes:
1. **Security First**: RLS policies and authentication at every layer
2. **Developer Productivity**: Modern tooling and clear patterns
3. **User Experience**: Fast, responsive, and intuitive interface
4. **Scalability**: Cloud-native design for growth
5. **Maintainability**: Clean code and documentation

---

**Document Version:** 1.0  
**Last Updated:** February 12, 2026  
**Maintained By:** Development Team  
**Status:** Active Development

---

## Appendix A: Technology References

### Official Documentation
- [React Documentation](https://react.dev)
- [TypeScript Documentation](https://www.typescriptlang.org/docs)
- [Supabase Documentation](https://supabase.com/docs)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [shadcn/ui Documentation](https://ui.shadcn.com)
- [React Query Documentation](https://tanstack.com/query/latest)
- [Vite Documentation](https://vitejs.dev)

### Database
- [PostgreSQL Documentation](https://www.postgresql.org/docs)
- [Row Level Security Guide](https://supabase.com/docs/guides/auth/row-level-security)

### Deployment
- [Vercel Documentation](https://vercel.com/docs)
- [Netlify Documentation](https://docs.netlify.com)

---

## Appendix B: Glossary

- **RLS**: Row Level Security - PostgreSQL feature for data access control
- **JWT**: JSON Web Token - Authentication token format
- **SPA**: Single Page Application - Client-side rendered web app
- **BaaS**: Backend as a Service - Managed backend infrastructure
- **RBAC**: Role-Based Access Control - Permission system based on user roles
- **HMR**: Hot Module Replacement - Live code updates without refresh
- **CDN**: Content Delivery Network - Distributed asset delivery
- **CRUD**: Create, Read, Update, Delete - Basic data operations
- **ORM**: Object-Relational Mapping - Database abstraction layer
- **API**: Application Programming Interface - Service communication protocol
