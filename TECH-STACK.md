# Recommended Technology Stack

## Infrastructure (Cloudflare)
- **Hosting**: Cloudflare Pages (Frontend) + Workers (API)
- **Database**: Cloudflare D1 (SQLite at edge)
- **Storage**: Cloudflare R2 (Property images)
- **Security**: Built-in DDoS protection, rate limiting, SSL
- **Estimated Cost**: $20-50/month

## External Services

### Communication
- **SMS**: Plivo - $0.0055/message + $10/month phone number
- **Email**: Resend - $20/month for 50K emails (both transactional and marketing)

### AI & Data
- **LLM**: GPT-5 Mini - $0.25 input / $2.00 output per 1M tokens
- **MLS Data**: TBD based on MVP testing approach

### Business Services
- **Payments**: Lemon Squeezy - ~5% all-inclusive (handles global tax)
- **CRM Integration**: Zapier webhooks - connects to 5000+ systems

## MVP Data Strategy

### Phase 1: Free Testing
- **Austin Board of REALTORS Developer Server** (free, real data, development only)
- **SimplyRETS Trial** (30 days, live data for beta testing)

### Phase 2: Early Customers  
- **Bridge Interactive** (~$60-120/month for 1-2 markets)
- **Pay MLS directly** through Bridge (no markup)

### Phase 3: Scale
- **SimplyRETS** or multiple MLS feeds as revenue justifies

## Development Tools
- **Frontend**: Next.js on Cloudflare Pages
- **API**: Cloudflare Workers with TypeScript
- **Database**: Cloudflare D1 with Drizzle ORM
- **Deployment**: Wrangler CLI

## Estimated Monthly Costs

### MVP Stage
- Infrastructure: $20-50
- External services: $50-100
- MLS data: $0-120 (trials then Bridge)
- **Total**: $70-270/month

### Growth Stage (1000+ users)
- Infrastructure: $50-100
- External services: $200-400
- MLS data: $300-600
- **Total**: $550-1100/month + 5% revenue