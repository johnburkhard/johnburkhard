# John Burkhard

Principal AI Engineer in Pittsburgh. I've spent nine years building hotel technology at one company through two mergers (Travel Tripper, then Pegasus, now Cendyn). I started as a frontend developer, ended up leading 30+ engineers across the US, India, Japan, France and Singapore, and I still write code every day.

Almost everything I build is private, so this page is the tour.

<p>
  <img src="https://skillicons.dev/icons?i=elixir,ts,python,rails,rust,react,nextjs,postgres,redis,docker,aws&perline=11" alt="Elixir, TypeScript, Python, Rails, Rust, React, Next.js, PostgreSQL, Redis, Docker, AWS" />
</p>

---

## Hotel booking and connectivity

A hotel doesn't run on one system. Rates and inventory live in a CRS or a PMS, guest accounts in an identity provider, cards with a payment gateway, content in a CMS, and points in a loyalty program. Every booking site, mobile app and partner channel needs all of them at once, and every vendor has its own API, its own rate limits and its own ways of failing.

I built the layer in the middle: one booking API and one data model, with every outside system plugged in behind a connector. Add a new PMS, CRS, payment or identity provider, and everything on top of it (booking sites, native apps, the partner API, metasearch) works with it unchanged. It runs our booking platform and a luxury brand's custom booking engine and mobile apps in production today.

I built it on Elixir because a single guest search fans out to several slow, rate-limited systems at the same time. Each request and each upstream call gets its own lightweight process. A slow PMS times out on its own instead of tying up a thread pool, supervisors restart anything that crashes without taking the node down, and thousands of live booking sessions run on the same nodes as the API.

What the platform does:

- **One contract for every supplier.** Connectors translate each vendor's API into one model. Tokens, connection pools, timeouts, circuit breakers and call budgets belong to the platform, so a new connector is just the translation.
- **Fast when suppliers are slow.** A layered cache (node memory, a shared cluster cache, then durable snapshots) answers most searches without calling the supplier. Guests keep browsing on the last known data when a supplier is down, and checkout always confirms price and availability live.
- **Stays inside vendor limits.** Every connection has a call budget. Bookings get first claim on it, and background refreshes only use what's left, which matters when a PMS caps every vendor on a gateway at a few dozen calls a second.
- **Ready for machine traffic.** Partners, metasearch and AI assistants search thousands of times per booking, so the partner API has its own per-client limits and cached answers.
- **Multi-tenant from the start.** Brands, hotels, environments and connections are isolated by design, with guest booking sites, customer and staff portals, and a site editor on the same platform.
- **Built to survive bad days.** A sandbox imitates real booking and payment systems and injects timeouts, rate limits and outages, and load tests have to pass before a release ships.

`Elixir` `Phoenix LiveView` `Ash` `PostgreSQL` `Valkey` `Oban` `AWS`

---

## Wayfinder: generative engine optimization (GEO) for hotels

I built [Cendyn Wayfinder](https://www.cendyn.com/cendyn-web-wayfinder-lp), our GEO platform for hotels. GEO is SEO for AI answers. Travelers are starting to plan trips by asking AI instead of searching, and most hotels have no idea what it's saying about them, let alone how to change it.

- **Polls all of AI.** Every model, on a schedule, asked the questions travelers actually ask.
- **Connects to anything.** Wayfinder isn't tied to a CMS or any one system. It connects to wherever a hotel's data lives and builds a verified picture of the hotel from it.
- **Catches drift and hallucinations.** It flags wrong numbers, missing details, contradictions and made-up claims, and notices when answers change after a model update.
- **Benchmarks the competition.** It tracks share of voice against each hotel's comp set, which property AI recommends, and keyword rank over time.
- **Agent Directives.** Hotels decide what AI should say about them: the facts that matter, what it should never say, and their brand voice. Directives pass down from brand to property.
- **Publishes for AI.** It generates llms.txt and structured data from verified facts, so AI crawlers read the right information.
- **Experiments that hold up.** Prompts run many times per model, with A/B tests and confidence intervals, so a change counts as a win only when the numbers back it.
- **One GEO health score** with a prioritized list of fixes and AI-written recommendations.

`Python` `FastAPI` `PostgreSQL + pgvector` `Redis` `Next.js`

---

## AI development lifecycle platform

I built our internal platform for putting AI agents to work. It's open to everyone in our 500+ person company, not just engineers, and it doesn't care which model or provider you use. People bring their own coding agent and account, and the platform runs it wherever they work.

- **Workflows anyone can build.** Draw the steps on a canvas, start from a template, describe it in a sentence, or turn a chat into one. Agents do the work, and people approve at the steps that matter. There are branches, parallel paths, loops, for-each over lists and sub-workflows, and runs can start from Jira, pull requests, webhooks, schedules or other workflows.
- **Templates for every team.** Jira ticket to finished fix. Multi-part projects with a spec and plan you approve. Code review against a team's own rules. QA that writes browser tests and files the bugs. Feature intake to PRD. "Build a tool" for people who don't code.
- **Apps.** Describe an app and it draws mockups for review, then builds it, previews it and publishes it for your team with its own database, versions and one-click rollback. Later changes only redraw the screens they touch.
- **Cloud spaces and live previews.** Everyone gets a private, isolated cloud space, so agents keep working when the laptop is closed, and every run gets a live preview of what it built.
- **Works where people already work.** Nobody has to change how they work. Developers stay in their own editor and terminal, and the platform plugs into the tools the company already runs instead of replacing them.
- **Knowledge and Insights.** Shared document collections that agents read from and workflows save to, and leadership views of cost, time, where work waits on people, and how people feel about it.
- **Safe by design.** Code merges only through the git host's own protections, agents get only the platform's tools, and nothing is ever deleted.

`TypeScript` `Node.js` `React` `PostgreSQL` `Docker` `MCP`

---

## Accessibility compliance

A platform for keeping a whole portfolio of websites accessible, and proving it.

- Scans every page in a real browser against WCAG 2.0, 2.1 or 2.2, and checks PDFs too
- Checks hotel room pages against the ADA reservations rule, a hotel-specific requirement that general accessibility tools don't look at
- Turns automated scans and manual review into signed conformance statements and VPAT reports
- Gives plain-language fix guidance, AI-suggested alt text, and action items that export to GitHub
- Runs scheduled scans, regression alerts and branded client reports across departments and products
- Hardened so it can safely scan sites it doesn't control

`Elixir` `Oban` `Playwright` `axe-core` `Node.js`

---

## For fun

- **Starfall.** A persistent multiplayer sci-fi RPG in the browser. Rust server, TypeScript client, isometric pixel art, and one shared universe with a player economy.
- **A web MUD** in Elixir and Phoenix LiveView, with AI-generated world building and procedural dungeons.
- **[POE2TradeHelper](https://github.com/johnburkhard/POE2TradeHelper).** A small Windows app that watches Path of Exile 2 trade messages and alerts you by sound or Discord.

---

[LinkedIn](https://www.linkedin.com/in/johnburkhard)
