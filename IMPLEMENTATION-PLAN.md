# Open House SaaS MVP: Atomic AI-Assisted Migration Plan

## Overview
Transform the existing beautiful design mockup into a production-ready MVP using atomic migration patterns optimized for AI-assisted implementation. This approach breaks down the complex transformation into dependency-ordered, independently implementable tasks that can be executed in a single session.

## Core Architecture Decisions

### Frontend Framework Migration
- **From**: Static export Next.js + Zustand localStorage
- **To**: Next.js 15 App Router + TanStack Query v5
- **Rationale**: Preserve exact UI/UX while enabling real server-state management

### Backend Infrastructure  
- **Target**: Cloudflare Workers + D1 + R2 + KV edge-native stack
- **Database**: D1 with Drizzle ORM for type safety
- **Multi-tenancy**: Agent-based isolation with proper data separation
- **Communication**: Mock system with full preview capabilities

## Atomic Migration Sequence

### Phase 1: Foundation Refactor (Infrastructure)
**Tasks in dependency order:**

1. **Remove Static Export** (`next.config.js:3`)
   - Delete `output: 'export'` line
   - Enable API routes and SSR capabilities
   - **Validation**: `npm run dev` starts successfully

2. **Install Production Dependencies**
   - Add TanStack Query v5, Drizzle ORM, Cloudflare types
   - Remove unused static-export related packages
   - **Validation**: `npm install` completes without conflicts

3. **Configure Cloudflare Environment**
   - Create `wrangler.toml` with D1 + KV + R2 bindings
   - Set up local development environment
   - **Validation**: `wrangler dev` connects to local services

4. **Database Schema Design**
   - Multi-tenant D1 schema with proper agent isolation
   - Migration files for properties, attendees, communications
   - **Validation**: `wrangler d1 migrations apply` executes successfully

### Phase 2: State Management Migration (Data Layer)
**Tasks in dependency order:**

5. **Replace Zustand Store** (`lib/store.ts`)
   - Remove localStorage-based Zustand implementation
   - Install TanStack Query provider and configuration
   - **Validation**: Build succeeds without store references

6. **API Route Implementation** 
   - Transform stub routes (`app/api/properties/route.ts`) to real D1 integration
   - Add multi-tenant data filtering by agent
   - **Validation**: API calls return properly structured data

7. **Query Hooks Implementation**
   - Create type-safe React Query hooks for all data operations
   - Replace direct store calls in components
   - **Validation**: All components compile and display data correctly

### Phase 3: Component Integration (UI Layer)
**Tasks in dependency order:**

8. **Property Generator Refactor** (`components/demo/property-generator.tsx`)
   - Replace client-side property creation with server mutations
   - Integrate with real MLS-style data generation
   - **Validation**: Property creation triggers server-side persistence

9. **Attendee Wizard Integration** (`components/demo/attendee-wizard.tsx`)
   - Connect form submission to TanStack Query mutations
   - Add proper loading states and error handling
   - **Validation**: Form submission creates database records

10. **Communication System Mock**
    - Template rendering with real data (`lib/templates.ts`)
    - Preview system for emails/SMS/calls
    - **Validation**: Generated communications display properly

### Phase 4: Production Features (Business Logic)
**Tasks in dependency order:**

11. **Multi-Agent System**
    - Agent onboarding and authentication flows
    - Data isolation and tenant management
    - **Validation**: Multiple agents see only their data

12. **QR Code Integration**
    - Dynamic QR generation linked to properties
    - Mobile-optimized attendee capture flows
    - **Validation**: QR codes resolve to property-specific forms

13. **Analytics and Scoring**
    - Real-time attendee scoring calculations
    - Agent dashboard with actionable insights
    - **Validation**: Score changes reflect immediately in UI

## Validation Strategy

### Automated Checkpoints
- **Build Validation**: `npm run build` succeeds after each phase
- **Type Safety**: `npm run type-check` passes with zero errors  
- **Linting**: `npm run lint` maintains code quality
- **Tests**: Existing functionality preserved throughout migration

### Manual Verification Points
- **UI Integrity**: All components render identically to original mockup
- **Data Flow**: Complete user journeys work end-to-end
- **Performance**: No regression in loading times or responsiveness
- **Mobile Experience**: QR flows work seamlessly on mobile devices

## Key Technical Patterns

### TanStack Query Integration
```typescript
// Replace Zustand pattern:
const properties = useAppStore((state) => state.properties)

// With TanStack Query pattern:  
const { data: properties } = useQuery({
  queryKey: ['properties', agentId],
  queryFn: () => fetchProperties(agentId)
})
```

### Multi-Tenant D1 Schema
```sql
-- Agent-isolated data model
CREATE TABLE properties (
  id TEXT PRIMARY KEY,
  agent_id TEXT NOT NULL,
  address TEXT NOT NULL,
  price INTEGER NOT NULL,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### Mock Communication Preview
```typescript
// Generate and preview without sending
const previewEmail = useMutation({
  mutationFn: (template: EmailTemplate) => 
    generateEmailPreview(template, attendeeData),
  onSuccess: (preview) => setEmailPreview(preview)
})
```

## Success Criteria
1. **Visual Parity**: Pixel-perfect match to existing design mockup
2. **Functional MVP**: All core user flows work with real data persistence
3. **Agent Demo Ready**: Multi-tenant system supports realistic demos
4. **Performance**: Sub-200ms response times for all user interactions
5. **Type Safety**: 100% TypeScript coverage with strict mode enabled

This atomic approach ensures each step is independently implementable and validatable, enabling reliable AI-assisted execution while preserving the beautiful existing UI design.