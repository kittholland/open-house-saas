# Research Findings Summary

## Service Provider Recommendations

### SMS Provider: Plivo ⭐
- **Cost**: $0.0055/message + $10/month phone number
- **Why**: Best price/performance balance, built-in compliance
- **Alternatives**: Twilio (premium), AWS SNS (complex setup)

### Email Provider: Resend ⭐
- **Cost**: $20/month for 50K emails
- **Why**: Modern developer experience, handles both transactional and marketing
- **Rejected**: AWS SES + Resend hybrid (unnecessary complexity)

### LLM Provider: GPT-5 Mini ⭐
- **Cost**: $0.25 input / $2.00 output per 1M tokens
- **Why**: Latest architecture, excellent quality, reasonable pricing
- **Alternatives**: Gemini 2.5 Flash-Lite (cheapest), Claude 4 Sonnet (premium)

### Payment Processor: Lemon Squeezy ⭐
- **Cost**: ~5% all-inclusive
- **Why**: Handles global tax compliance as Merchant of Record
- **Switch to**: Stripe at $50K+ MRR for lower fees

### MLS Data: Bridge Interactive → SimplyRETS ⭐
- **MVP**: Bridge Interactive (~$60-120/month, pay MLS directly)
- **Scale**: SimplyRETS ($300-500/month, better developer experience)
- **Free trials**: Austin Board of REALTORS, SimplyRETS demo accounts

## Key Insights

### What We Learned
1. **Cloudflare native services** are limited - stick to hosting/security
2. **MLS verification complexity** is low since you need it anyway for listings
3. **CRM integration via Zapier** is much simpler than custom APIs
4. **Restricted MLS licensing** may not apply since you're displaying data publicly
5. **Free MLS access** is extremely limited but good for MVP testing

### Rejected Approaches
- Multiple LLM providers (unnecessary complexity)
- AWS SES + Resend email hybrid (premature optimization)
- Cloudflare Workers AI (unproven for production)
- Manual property data entry (defeats core value prop)
- Complex custom CRM integrations (Zapier handles this)

## MVP Strategy

### Development Approach
1. **Start free**: Austin Developer Server for development
2. **Beta test**: SimplyRETS trial with real agents
3. **Early customers**: Bridge Interactive for 1-2 markets
4. **Scale**: Add markets as revenue grows

### Launch Strategy
- Focus on 1-2 major metro areas initially
- Target tech-savvy realtors first
- Prove lead generation value before expanding features
- Keep infrastructure costs under $300/month until product-market fit

## Total Estimated Costs

### MVP (0-10 customers)
- **Infrastructure**: $70-270/month
- **Key insight**: Can validate concept for under $300/month

### Growth (100+ customers)  
- **Infrastructure**: $550-1100/month + 5% revenue
- **Break-even**: ~$1000 MRR covers all costs