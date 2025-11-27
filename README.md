Join the Skool - https://www.skool.com/iss-ai-automation-school-6342/about

# Claude Code SaaS App Generator v3

**From Idea to Production SaaS with Payments in One Automated Workflow**

Transform any Google AI Studio project OR your requirements into a complete, production-ready SaaS application with authentication, AI features, **Stripe payments & subscriptions**, real-time database, 70+ SEO landing pages, and deployment - all automated with Claude Code agents.

## What's New in v3

- **Stripe Integration** - Automatic payment setup via CLI (products, prices, webhooks)
- **Planner Agent (Opus)** - Architecture decisions before building complex features
- **Context Management** - Optimized to use less of your 5h/weekly limits
- **No Fallbacks Rule** - Every agent escalates problems instead of guessing
- **Pricing & Billing Pages** - Complete subscription management UI
- **70+ Landing Pages** - Including pricing-focused SEO pages

## What Does This Do?

**Give it:**
- An **AI Studio project folder** (exported from Google AI Studio)
- OR a **description of what you want to build**
- Your **API keys** (Jina, AI provider, Clerk, **Stripe**)

**Get:**
- Complete **Next.js SaaS application**
- **Clerk authentication** (sign-up, sign-in, protected routes)
- **Convex serverless backend** (real-time database)
- **Stripe payments** (checkout, subscriptions, billing portal)
- **AI features** using verified model names from real docs
- **70+ SEO landing pages** (features, use cases, pricing, comparisons)
- **User dashboard** with usage tracking and billing
- Ready to **deploy** to Vercel

## The Agent System

| Agent | Model | Purpose |
|-------|-------|---------|
| **Orchestrator** | Your context | 200k context, manages todos, coordinates agents |
| **Planner** | Opus | Architecture decisions for complex features |
| **Design Generator** | Sonnet | Beautiful SaaS UI (dashboard, landing, pricing, billing) |
| **Research Agent** | Sonnet | Scrapes real docs via Jina - never guesses |
| **Convex Builder** | Sonnet | Backend + subscriptions + webhooks |
| **AI Implementor** | Sonnet | AI features with usage limits |
| **Stripe Builder** | Sonnet | Automatic Stripe setup via CLI |
| **Landing Page Generator** | Sonnet | 10-15 SEO pages (spawned in parallel!) |
| **Next.js Builder** | Sonnet | Frontend + checkout + billing |
| **Coder** | Sonnet | Individual todo implementation |
| **Tester** | Sonnet | Playwright visual testing + payment flows |
| **Stuck** | Sonnet | Human escalation - no fallbacks |

## Prerequisites

### 1. Clone This Repository

```bash
git clone https://github.com/IncomeStreamSurfer/claude-code-agents-wizard-v3.git
cd claude-code-agents-wizard-v3

# Create the Convex project INSIDE this folder
npx create-convex@latest my-saas-app

# Select:
# - Framework: Next.js (App Router)
# - Auth: Clerk

cd my-saas-app

# Copy the .claude folder from parent
cp -r ../.claude ./

# Start once to initialize Convex
npm run dev
# Wait for Convex dashboard, then Ctrl+C
```

### 2. Set Up Clerk Authentication

1. Go to [clerk.com](https://clerk.com) and create an account
2. Create a new application
3. Configure providers (Google + Email recommended)
4. Copy your keys:
   - `NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY`
   - `CLERK_SECRET_KEY`
5. Go to **Configure** → **JWT Templates** → **Add new template**
6. Name: `convex`, Type: `Convex`

### 3. Set Up Stripe

1. Go to [stripe.com](https://stripe.com) and create an account
2. Get your **Secret Key** from Developers → API Keys
   - Test mode: `sk_test_...`
   - Live mode: `sk_live_...`
3. Install Stripe CLI: `brew install stripe/stripe-cli/stripe` (Mac)

### 4. Get Your API Keys

| Service | Purpose | Get it at |
|---------|---------|-----------|
| **Jina AI** | Documentation research | [jina.ai](https://jina.ai) |
| **Google AI** | AI features | [makersuite.google.com](https://makersuite.google.com) |
| **OpenAI** | AI features | [platform.openai.com](https://platform.openai.com) |
| **Anthropic** | AI features | [console.anthropic.com](https://console.anthropic.com) |

## Usage

### Required Inputs

```
1. APP DESCRIPTION:
   - What should your app do?
   - Which AI model to use?

2. CLERK CREDENTIALS:
   - NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_...
   - CLERK_SECRET_KEY=sk_...
   - JWT Issuer Domain: https://your-app.clerk.accounts.dev

3. STRIPE API KEY:
   - sk_test_... (test mode) or sk_live_... (production)

4. AI PROVIDER API KEY (at least one):
   - Google: AIzaSy...
   - OpenAI: sk-...
   - Anthropic: sk-ant-...

5. JINA API KEY:
   - jina_...

6. PROJECT DIRECTORY:
   - /path/to/your-convex-app
```

### Example: Complete Input

```
Build me an AI thumbnail generator where users upload images and
get 8 variations using Google Gemini.

Clerk info:
- Domain: https://my-app.clerk.accounts.dev
- NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_xxxxx
- CLERK_SECRET_KEY=sk_test_xxxxx

Stripe API key: sk_test_xxxxx

Google AI API key: AIzaSyxxxxxxxxxxxxxxxxx

Jina API key: jina_xxxxxxxxxxxxxxxxxxxxxxxxxxxxx

Project directory: /Users/me/my-thumbnail-app
```

### Start Building

```bash
cd my-saas-app
claude
# Paste your requirements with all credentials
```

## The Automated Workflow

```
Step 0:   Collect inputs (App, Clerk, Stripe, AI keys, Jina, directory)
Step 0.5: [Complex?] → Planner (Opus) → Architecture plan
Step 1:   Design → Beautiful SaaS UI (dashboard, pricing, billing)
Step 2:   Environment → All env vars including Stripe
Step 3:   Research → Scrape real docs, verify model names
Step 4:   Backend → Convex schema + subscriptions + webhooks
Step 5:   AI Features → Implementation with usage limits
Step 5.5: Stripe → Auto-create products, prices, webhooks via CLI
Step 6:   Landing Pages → 70+ pages in parallel (6 agents)
Step 7:   Frontend → Next.js + checkout + billing dashboard
Step 8:   Testing → Playwright + payment flow verification
Step 9:   GitHub → Push to repository
Step 10:  Report → Summary + deployment instructions
```

## What Gets Built

### Stripe Integration (Automatic!)

The **stripe-builder** agent automatically:
- Creates Stripe products (Pro, Enterprise)
- Creates prices (monthly + yearly)
- Sets up webhook endpoint → Convex
- Configures billing portal
- Sets all environment variables

**Default Pricing:**
| Tier | Monthly | Yearly | Features |
|------|---------|--------|----------|
| Free | $0 | $0 | 10 generations/mo |
| Pro | $29 | $290 | 500 generations/mo |
| Enterprise | $99 | $990 | Unlimited |

### 70+ SEO Landing Pages

Generated in parallel by 6 agents:
- 12 feature pages
- 12 use case pages
- 12 industry pages
- 12 comparison pages ("Alternative to X")
- 12 problem/solution pages
- 10 pricing-focused pages

### Output Structure

```
your-saas-app/
├── design/                     # Generated designs
│   ├── design-system.css
│   ├── dashboard.html
│   ├── landing.html
│   ├── auth.html
│   ├── pricing.html           # NEW: Pricing page
│   └── billing.html           # NEW: Billing dashboard
├── landing-pages/              # 70+ page JSON files
├── research/                   # Scraped documentation
├── lib/stripe/                 # NEW: Stripe configuration
│   ├── config.ts              # Product/price IDs
│   └── plans.ts               # Plan features & limits
├── app/
│   ├── pricing/               # NEW: Pricing page
│   ├── checkout/              # NEW: Success/cancel pages
│   ├── dashboard/
│   │   └── billing/           # NEW: Billing dashboard
│   ├── api/stripe/            # NEW: Checkout & portal APIs
│   └── (marketing)/           # 70+ landing pages
├── convex/
│   ├── schema.ts              # + subscriptions, payments tables
│   ├── stripe.ts              # NEW: Subscription functions
│   ├── billing.ts             # NEW: Usage tracking
│   └── http.ts                # NEW: Stripe webhooks
└── components/
    ├── pricing/               # NEW: Pricing components
    └── billing/               # NEW: Billing components
```

## Context Management

This system is optimized to use less of your 5h and weekly limits:

### Key Strategies

1. **Subagent Isolation** - Agents burn tokens in their context, return only summaries
2. **Manual /compact** - Use at 70% capacity with preservation instructions
3. **Scratchpad Files** - Track progress in files, not conversation memory
4. **Concise Returns** - Agents return ~500 tokens, not full output

### Commands

| Command | When to Use |
|---------|-------------|
| `/context` | Check current usage |
| `/compact preserve [what]` | At 70% capacity |
| `/clear` | Switching to unrelated task |
| `/mcp` | Disable unused MCP servers |

See `.claude/CONTEXT_MANAGEMENT.md` for full guide.

## The "No Fallbacks" Rule

Every agent is **HARDWIRED** to invoke the stuck agent when problems occur:

```
Traditional AI: Error → Workaround → Silent failure
This System:    Error → Stuck agent → Ask YOU → Correct solution
```

You're never surprised by bad decisions.

## Environment Variables

Your `.env.local` will include:

```bash
# Convex
CONVEX_DEPLOYMENT=your-deployment
NEXT_PUBLIC_CONVEX_URL=https://your-project.convex.cloud

# Clerk
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
CLERK_SECRET_KEY=sk_test_...

# Stripe
STRIPE_SECRET_KEY=sk_test_...
STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# AI (at least one)
GOOGLE_GENERATIVE_AI_API_KEY=AIzaSy...
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
```

**After building, set Convex env vars:**
```bash
npx convex env set CLERK_JWT_ISSUER_DOMAIN=https://your-app.clerk.accounts.dev
npx convex env set STRIPE_SECRET_KEY=sk_...
npx convex env set STRIPE_WEBHOOK_SECRET=whsec_...
```

## Deployment

### Vercel (Recommended)
```bash
vercel deploy
```

### Production Checklist

1. Update Stripe webhook URL in Dashboard to production URL
2. Set production `STRIPE_WEBHOOK_SECRET` in Convex
3. Switch Stripe to live mode keys
4. Verify all Convex env vars are set

## Files Reference

| File | Purpose |
|------|---------|
| `.claude/CLAUDE.md` | Orchestrator instructions |
| `.claude/CONTEXT_MANAGEMENT.md` | Context optimization guide |
| `.claude/agents/*.md` | All agent configurations |
| `.mcp.json` | MCP server configurations |

## Resources

- [AI SDK Documentation](https://ai-sdk.dev/docs)
- [Convex Documentation](https://docs.convex.dev)
- [Clerk Documentation](https://clerk.com/docs)
- [Stripe Documentation](https://stripe.com/docs)
- [ISS AI Automation School](https://www.skool.com/iss-ai-automation-school-6342/about)

## Contributing

Improvements welcome:
- New agent types
- Better prompts
- Additional providers
- UI templates
- Context optimizations

## License

MIT - Use it, modify it, profit from it!

## Credits

Built by [Income Stream Surfer](https://www.youtube.com/incomestreamsurfers)

Powered by:
- Claude Code's agent system
- Jina AI for documentation research
- Convex serverless backend
- Clerk authentication
- **Stripe payments**
- Next.js App Router

---

**Ready to build a SaaS with payments?**

```bash
git clone https://github.com/IncomeStreamSurfer/claude-code-agents-wizard-v3.git
cd claude-code-agents-wizard-v3
npx create-convex@latest my-saas-app
cd my-saas-app
cp -r ../.claude ./
claude
```

Then tell Claude what you want to build with your credentials!

From idea to production SaaS with AI + Payments + 70 landing pages in one automated workflow!
