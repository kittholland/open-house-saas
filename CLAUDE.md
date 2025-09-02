# Claude Code Configuration

## Project Context
Open House SaaS MVP - Transform design mockup into production system using atomic AI-assisted migration patterns.

## Key Commands & Patterns

### Build & Validation Commands
```bash
# Always run after each phase to validate changes
npm run dev          # Development server validation
npm run build        # Production build validation  
npm run type-check   # TypeScript validation (if available)
npm run lint         # Code quality validation
```

### Cloudflare Commands
```bash
# Local development
wrangler dev --local

# Database operations
wrangler d1 create <db-name>
wrangler d1 migrations create <db-name> "<description>"
wrangler d1 migrations apply <db-name> --local
wrangler d1 execute <db-name> --local --command="<SQL>"

# Deploy
wrangler deploy
```

## Critical Migration Principles

### 1. Atomic Task Execution
- Each task must be independently implementable
- Validate after each change before proceeding
- Preserve UI/UX exactly during backend changes

### 2. Dependency Order Enforcement
```typescript
// Phase 1: Infrastructure (next.config.js, dependencies, wrangler.toml)
// Phase 2: Data Layer (store → TanStack Query, API routes)
// Phase 3: UI Layer (component integration, form connections)  
// Phase 4: Business Logic (multi-tenancy, QR codes, analytics)
```

### 3. TanStack Query Migration Pattern
```typescript
// FROM: Zustand localStorage pattern
const items = useAppStore((state) => state.items)
const addItem = useAppStore((state) => state.addItem)

// TO: TanStack Query pattern
const { data: items } = useQuery({
  queryKey: ['items', agentId],
  queryFn: () => fetchItems(agentId)
})

const addItemMutation = useMutation({
  mutationFn: createItem,
  onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ['items'] })
  }
})
```

### 4. Multi-Tenant D1 Schema Pattern
```sql
-- All tables include agent_id for data isolation
CREATE TABLE properties (
  id TEXT PRIMARY KEY,
  agent_id TEXT NOT NULL,
  -- other fields
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Always filter by agent_id in queries
SELECT * FROM properties WHERE agent_id = ?
```

### 5. File Change Protocol
- Read file first to understand current structure
- Make targeted changes preserving existing functionality
- Test immediately after each change
- Never change more than one component per iteration

## Deployment & Testing Methodology

### GitHub Integration Deployment
- **Strategy**: Use Cloudflare GitHub integration for automatic deployments
- **Workflow**: Push to main branch → Cloudflare agent builds and deploys → Test live URL with Playwright
- **Benefits**: Real production environment testing, automatic deployment pipeline

#### Key Considerations
- **Platform**: Cloudflare Pages (not Workers) - optimized for Next.js with automatic detection
- **API Routes**: Next.js API routes become Pages Functions automatically
- **Database Migrations**: Apply via `wrangler d1 migrations apply --remote` after deployment
- **Environment Variables**: Production bindings configured in Cloudflare Pages dashboard
- **Build Time**: ~2-3 minutes per deployment cycle
- **Free Tier**: 500 deployments/month + 100k requests/month included

#### Prerequisites (User Setup Required)
1. Cloudflare account with Workers/Pages enabled
2. GitHub repository connected to Cloudflare Pages
3. Production D1 database, KV namespace, R2 bucket created
4. Binding IDs updated in wrangler.toml or Cloudflare dashboard

### Validation Checkpoints

#### After Each Task (Local Development)
1. `npm run build` - Production build validation
2. TypeScript - No new type errors  
3. Local functionality verification

#### After Each Phase (Production Testing)
1. **Git Commit & Push**: Commit working changes to trigger deployment
2. **Wait for Deployment**: Monitor Cloudflare dashboard for successful build
3. **Playwright Validation**: Test live production URL with full user flows
4. **Performance Check**: Verify sub-200ms response times on live site
5. **Visual Regression**: Ensure UI matches original mockup pixel-perfect

#### Emergency Rollback Process
1. **Git Revert**: Revert problematic commits if deployment fails
2. **Manual Rollback**: Use Cloudflare dashboard to rollback to previous working version
3. **Local Fix**: Fix issues locally before next deployment attempt

## Current Architecture State

### Frontend (design-mockup/)
- Next.js with static export (to be removed)
- shadcn/ui components (preserve exactly)
- Zustand + localStorage state (to be replaced)
- Beautiful responsive design (must preserve)

### Target Backend
- Cloudflare Workers (serverless functions)
- D1 database (serverless SQLite)
- Drizzle ORM (type-safe database access)
- Multi-tenant agent isolation

## Emergency Rollback Strategy
- Each atomic change is reversible
- Git commit after each successful task
- Keep original mockup files as reference
- Maintain backup of working states

## Success Metrics
- Visual: Pixel-perfect match to original mockup
- Functional: All user flows work with real data
- Performance: Sub-200ms response times
- Type Safety: 100% TypeScript coverage
- Demo Ready: Multi-agent system functional