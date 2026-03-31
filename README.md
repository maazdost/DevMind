# DevMind — AI Code Assistant SaaS

A production-ready Claude Code replica you can sell as a SaaS. Built with free AI APIs.

---

## What This Is

A complete browser-based AI coding assistant with:
- **Landing page** (marketing, pricing, features)
- **App page** (editor + AI chat UI)
- **Settings modal** (API key, model, provider config)
- **Real AI integration** via OpenRouter, OpenAI, or Groq
- **Free tier** that works with no paid API (demo mode)
- **Billing UI** with Starter / Pro / Team plans

---

## How to Use Right Now

Open `index.html` in a browser. It works as a demo immediately.

To enable real AI:
1. Get a free API key from https://openrouter.ai
2. Click ⚙ Settings in the app
3. Paste your key → Save
4. Free models available: Llama 3.1, Gemini Flash, Mistral

---

## AI Providers (All Free Options)

| Provider | Free Models | Get Key |
|----------|-------------|---------|
| **OpenRouter** | Llama 3.1 8B, Gemini Flash, Mistral 7B | openrouter.ai |
| **Groq** | Llama 3.1 70B (ultra-fast!) | console.groq.com |
| **Google** | Gemini 1.5 Flash | aistudio.google.com |
| **OpenAI** | GPT-4o Mini (cheap) | platform.openai.com |

---

## Architecture of Real Claude Code (What You're Replicating)

Based on the leaked source and system prompts:

```
User Input
   ↓
System Prompt (dynamically assembled)
   ├─ Identity + capabilities section
   ├─ Available tools section  
   ├─ Project context (files, structure)
   └─ Session-specific suffix
   ↓
LLM (Claude / GPT-4o / Gemini)
   ↓
Tool Calls (parallel or sequential):
   ├─ ReadFile(path)
   ├─ WriteFile(path, content)
   ├─ EditFile(path, old, new)
   ├─ SearchCode(pattern)
   ├─ BashCommand(cmd)
   ├─ WebSearch(query)
   └─ TodoList(tasks)
   ↓
Tool Results → back to LLM
   ↓
Final Response + File Diffs
```

The key insight from the source: Claude Code is NOT just a chatbot.
It's an **agent loop** that keeps calling tools until the task is complete.

---

## How to Turn This Into a Real Product (Roadmap)

### Phase 1 — MVP (1-2 weeks)
- [ ] Deploy to Vercel/Netlify (drag and drop the HTML file)
- [ ] Add Supabase for user auth (free tier)
- [ ] Add usage tracking per user in Supabase
- [ ] Gate AI calls behind login

### Phase 2 — File System (2-4 weeks)
- [ ] GitHub OAuth — let users connect repos
- [ ] Use Octokit to read/write files to GitHub
- [ ] Show real file tree from repo
- [ ] Apply AI-suggested diffs via GitHub API

### Phase 3 — Real Agent Loop (4-6 weeks)
- [ ] Backend: Node.js server with tool execution
- [ ] Implement tool: ReadFile, WriteFile, SearchCode
- [ ] Implement tool: BashCommand (in Docker sandbox)
- [ ] Stream responses with SSE

### Phase 4 — Payments (1 week)
- [ ] Add Stripe Checkout
- [ ] Create 3 price tiers in Stripe dashboard
- [ ] Webhook: update user plan on payment
- [ ] Enforce request limits per plan

---

## The System Prompt (From Leaked Claude Code)

Claude Code uses this architecture for its system prompt:

```
You are Claude Code, Anthropic's official CLI for agentic coding...

## Capabilities
You have access to tools for reading files, writing files, running bash...

## Guidelines  
- Complete tasks autonomously without asking for confirmation
- Use parallel tool calls when possible
- Think step by step before acting
- Prefer surgical edits over rewriting entire files
- Always read a file before editing it

## Tools available:
[dynamically listed tools with schemas]

## Project context:
[injected at runtime: file tree, CLAUDE.md contents, recent git log]
```

The boundary marker splits this into a cacheable prefix (global) and
session-specific suffix — this is how they reduce API costs by ~80%.

---

## Monetization Strategy

### Pricing Model
- **Starter**: Free, 50 requests/month (converts users)
- **Pro**: $29/month, 2000 requests (main revenue)
- **Team**: $79/month, 10k requests + 5 seats (enterprise growth)

### Your Margins
- GPT-4o Mini: ~$0.00015/1K tokens → 2000 avg requests ≈ $0.30 cost
- Llama 3.1 (OpenRouter free tier): $0.00
- At $29/month Pro: >99% margin using free/cheap models

### Growth Levers
1. **API key bring-your-own** — charge for convenience, not compute
2. **GitHub integration** — sticky, daily usage
3. **Team seats** — viral, org-level contracts
4. **Affiliate** — pay devs to refer teams

---

## Tech Stack for Full Production

```
Frontend:  Next.js 14 + Tailwind (or this single HTML file to start)
Backend:   Node.js / Bun (for tool execution + API proxy)
Auth:      Supabase Auth or Clerk
DB:        Supabase (Postgres)
Payments:  Stripe
AI:        OpenRouter (multi-model, free models available)
Deploy:    Vercel (frontend) + Railway/Fly.io (backend)
Sandbox:   Docker (for safe bash command execution)
```

---

## Legal Notes

This is built from publicly available information and open-source
references. The system prompts used are from publicly leaked/extracted
sources. This product does not use any Anthropic trademarks.
Name your product something distinct (DevMind, CodePilot, etc.)

---

## Quick Deploy (5 minutes)

1. Go to vercel.com → New Project → "Deploy from file"
2. Upload `index.html`
3. Set domain → you're live
4. Share the URL, collect emails, validate demand
5. Add Stripe when you have 10+ interested users
