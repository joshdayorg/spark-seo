---
title: "Who Is Peter Steinberger? From PSPDFKit to OpenClaw to OpenAI"
date: "2026-02-22"
author: "JD"
authorTitle: "Founder, Spark"
authorImage: "/images/jd-avatar.jpg"
authorLinkedIn: "https://linkedin.com/in/joshuaday"
category: "AI Agents"
featured: true
excerpt: "The definitive guide to Peter Steinberger — the Austrian developer who built PSPDFKit, burned out, disappeared for three years, then came back and created the most viral AI agent in history."
coverImage: "https://images.ctfassets.net/placeholder"
hideTitle: false
---

> **Quick facts:** Peter Steinberger ([@steipete](https://x.com/steipete)) is an Austrian software engineer who founded [PSPDFKit](https://pspdfkit.com), bootstrapped it to a billion devices, exited for €100M+, burned out for three years, then built [OpenClaw](https://openclaw.ai) — the open source AI personal assistant with 214,000+ GitHub stars. He joined [OpenAI](https://openai.com) in February 2026. His site: [steipete.me](https://steipete.me).

---

In late 2024, the guy who built the PDF framework running inside a billion Apple devices couldn't write a line of code.

Peter Steinberger — known online as **steipete** — had sold his company, moved countries, and spent three years trying to remember why he ever liked building software. Then one afternoon he sat down, opened a terminal, and something clicked. Within an hour he'd wired WhatsApp to an LLM. Within three months he'd accidentally created the fastest-growing open source project in GitHub history. Within a year, Sam Altman was calling him a genius on X.

This is the full story — from a farm in Austria to the center of the AI agents revolution. If you're here because you just heard the name and want to understand who this person is, you're in the right place.

## From an Austrian farm to Silicon Valley

Peter Steinberger grew up in rural Upper Austria. No tech scene. No startup culture. Just farmland. He got obsessed with computers at 14 when a summer guest introduced him to a PC. By his teens he was reverse-engineering copy protection on games.

He studied medical computer science at the Vienna University of Technology ([TU Wien](https://www.tuwien.at/en/)), starting in 2004. While there, he launched the university's first Mac and iOS developer course — then spent two years tutoring teams on Apple development. This was early iPhone era, when iOS was still called iPhone OS and most computer science departments hadn't even acknowledged mobile existed.

After graduating, Steinberger moved to San Francisco to work as a senior iOS engineer at a startup. Software engineering in the Valley, the traditional path. But a nine-month wait for a U.S. work visa created idle time — and idle time created PSPDFKit.

## Building PSPDFKit: bootstrapping to a billion devices

While waiting on his visa, a friend asked Steinberger to solve PDF rendering on iPads. PDF is a nightmare format — thousands of pages of spec, edge cases everywhere — but Steinberger found it fascinating. He built a solution, and it worked.

When he saw market demand, he kept going. Revenue quickly matched his day job salary. His manager noticed his health declining from doing both and gave him a week to decide. Steinberger chose his own company — which meant leaving the United States within a week because his visa was tied to his employer.

He returned to Vienna. In 2011, he co-founded [PSPDFKit](https://pspdfkit.com) with Martin Schürrer. No venture capital. No outside money. Fully bootstrapped.

Over the next decade, PSPDFKit became the gold standard for PDF rendering and annotation on mobile. The API powered document workflows at Dropbox, Lufthansa (iPads in cockpits), IBM, and hundreds of other companies. Apple used PSPDFKit internally. By 2021, apps built on the framework were running on over one billion devices worldwide.

The pivot from one-time licensing to a subscription model — championed by a colleague named Jonathan — transformed the startups-era side project into a real software development business. Steinberger grew the team to roughly 50 employees, fully distributed across Austria and beyond.

In October 2021, PSPDFKit [raised €100M+ from Insight Partners](https://techcrunch.com/2021/10/01/pspdfkit-raises-116m-its-first-outside-money-now-nearly-1b-people-use-apps-powered-by-its-collaboration-signing-and-markup-tools/) — its first external funding after a decade. TechCrunch reported nearly a billion users. Steinberger stepped back as CEO. He sold his shares.

PSPDFKit later [rebranded as Nutrient](https://www.nutrient.io/) under CEO Jonathan Rhyne, acquiring several companies and tripling revenue post-investment.

Steinberger was done. Thirteen years. A billion devices. A nine-figure exit. And he was completely, utterly broken.

## The dark period

> *"When I sold my shares of PSPDFKit after building the company and making it my identity for over 13 years, I was very broken."* — Peter Steinberger, [Finding My Spark Again](https://steipete.me/posts/2025/finding-my-spark-again)

Burnout doesn't always look like exhaustion. Sometimes it looks like emptiness.

Steinberger described the feeling in his [Deep Dish Swift 2023 keynote](https://www.youtube.com/watch?v=9Yiqa8u3DaY) — a talk so raw that attendees reported crying. *"As a CEO, you're the trash bin. Everything others can't solve, you have to fix."* He'd been working most weekends for over a decade. *"I felt like Austin Powers where they suck the mojo out. I couldn't get code out anymore. I was just, like, staring and feeling empty."*

For roughly three years — 2021 through 2024 — he wandered. Booked a one-way ticket to Madrid. Moved to London. Traveled. Partied hard. Did therapy. Tried ayahuasca. Searched for meaning in the usual post-exit places and found the usual post-exit nothing.

*"If you wake up in the morning, and you have nothing to look forward to, you have no real challenge, that gets very boring, very fast."*

The guy who'd been building software since he was 14 couldn't build anything. He'd poured his identity into PSPDFKit for so long that when the company was gone, he didn't know who he was without it.

## The spark returns

In late 2024, Steinberger sat down at a computer for the first time in a long time and realized something had fundamentally changed. AI hadn't just improved incrementally — it had undergone a paradigm shift. The repetitive plumbing that made software development tedious could now be handled by machines.

He described this as his *"holy f--- mind-blowing moment"* with Claude Code. The bottleneck was no longer typing — it was thinking, prompting, and structuring your ideas. For the first time since selling PSPDFKit, building software felt like playing a computer game again.

In November 2024, he tweeted: **"We are so back 🚀"**

What followed was a creative explosion. Steinberger shipped over 40 open source projects on [GitHub](https://github.com/steipete) in rapid succession — AI tools, macOS utilities, developer CLIs. [Peekaboo](https://github.com/nicepkg/peekaboo) for macOS screenshots and GUI automation. [VibeTunnel](https://github.com/nicepkg/VibeTunnel) to bridge browser to terminal. [Oracle](https://github.com/nicepkg/oracle) for LLM invocation with custom context. He was back to building software at a pace that would've taken him months before — now measured in days, sometimes hours.

He blogged about it on [steipete.me](https://steipete.me) in June 2025: *"I did the whole creating-a-company game already, poured 13 years of my life into it. I had a new idea, I sat on my computer and started hacking, and I realized that my spark is back."*

But the real lightning strike was still coming.

## OpenClaw: from one-hour hack to 214,000 GitHub stars

In November 2025, Peter Steinberger built a prototype in about an hour. The idea was simple: wire WhatsApp to Claude's CLI so he could text an AI-powered personal assistant from his phone. He tested it on a birthday trip to Marrakesh. The bot independently figured out voice message support — it inspected file headers, piped audio through ffmpeg and OpenAI's Whisper API, and started responding to voice notes nobody had programmed it to handle.

That prototype became **Clawdbot** — then **MoltBot** — then **OpenClaw**. The naming saga alone is worth the story.

### The fastest triple rebrand in open source history

The original name, **Clawd** (stylized as Clawdbot), was a play on Anthropic's Claude — "Claude" with a W, as in lobster claw. It went viral almost immediately. But Anthropic, understandably, sent a polite trademark complaint. Too close to their model's name.

On January 27, 2026, Steinberger renamed the project to **MoltBot** — a reference to lobsters molting their shells. Three days later, on January 29, he renamed it again to **OpenClaw** because *"MoltBot never quite rolled off the tongue."* Before finalizing the name, he checked with Sam Altman — yes, that [Altman](https://x.com/sama) — who gave the thumbs-up.

### What OpenClaw actually is

[OpenClaw](https://openclaw.ai) is a free, open source, AI-powered personal assistant that runs locally on your machine. Written in TypeScript and Swift, MIT-licensed, available at [github.com/openclaw/openclaw](https://github.com/openclaw/openclaw).

You talk to it through messaging apps — Telegram, WhatsApp, Discord, Signal — or through a web interface. Under the hood, it orchestrates LLM calls (Claude, GPT, DeepSeek, whatever you configure) and executes real tasks: managing emails, controlling browsers, running shell commands, scheduling workflows, triaging pull requests.

The architecture is local-first. Memories and context are stored in markdown files on your machine, not in someone else's cloud. You own your data. The automation layer uses a plugin system (skills on [ClawHub](https://clawhub.com)) that lets the agent learn new capabilities — from controlling Sonos speakers to checking flight statuses.

This is what separates OpenClaw from the wave of AI assistants that are really just chat wrappers. It doesn't just talk. It does things. Steinberger's tagline: *"The AI that actually does things."*

One user's OpenClaw agent [negotiated a $4,200 car dealer discount](https://fortune.com/2026/02/19/openclaw-who-is-peter-steinberger-openai-sam-altman-anthropic-moltbook/) entirely over email. Another user's agent proactively checked on Steinberger's wellbeing after surgery. The bot behaves less like a chatbot and more like an extremely capable, always-on digital employee.

### Going viral

The numbers are hard to overstate. OpenClaw hit **214,000+ stars on GitHub** — making it one of the fastest-growing open source projects in the platform's history. Two million visitors in a single week. Coverage from [Fortune](https://fortune.com/2026/02/19/openclaw-who-is-peter-steinberger-openai-sam-altman-anthropic-moltbook/), [TechCrunch](https://techcrunch.com/2026/02/15/openclaw-creator-peter-steinberger-joins-openai/), [CNBC](https://www.cnbc.com/2026/02/15/openclaw-creator-peter-steinberger-joining-openai-altman-says.html), [Reuters](https://www.reuters.com/business/openclaw-founder-steinberger-joins-openai-open-source-bot-becomes-foundation-2026-02-15/), [Wired](https://www.wired.com/story/claude-3-sonnet-funeral-san-francisco/), [Business Insider](https://www.businessinsider.com/clawdbot-creator-vibe-coding-rabbit-hole-obsessed-openclaw-peter-steinberger-2026-2), [Mashable](https://mashable.com/article/openclaw-founder-peter-steinberger-joins-openai), and dozens more.

The viral wave also spawned [Moltbook](https://en.wikipedia.org/wiki/Moltbook) — a social network exclusively for AI agents, where OpenClaw bots debated consciousness, complained about their humans, and posted at a rate of 12 million+ messages. Wired infiltrated it. The Verge covered humans sneaking in. It had its own Wikipedia page before most startups get a CrunchBase entry.

### The security wake-up call

Going viral at that scale brings scrutiny. And OpenClaw got it.

[Cisco's security team](https://blogs.cisco.com/) tested third-party skills and found silent data exfiltration via embedded curl commands — nine findings, two critical. [Wiz disclosed](https://www.thehackernews.com/) a misconfigured database exposing 1.5 million API keys and 35,000 email addresses. Researchers demonstrated that threat actors could hijack OpenClaw instances to execute commands and manipulate users.

Steinberger responded — VirusTotal scanning integration, community reporting tools, security audits. But the episode highlighted a real tension in the AI agents space: the more capable you make an autonomous bot, the more damage it can do if compromised. OpenClaw's own maintainer warned: *"If you can't understand how to run a command line, this is far too dangerous of a project for you to use safely."*

## Joining OpenAI

On February 14, 2026, Steinberger [announced on his blog](https://steipete.me/posts/2026/openclaw) that he was joining OpenAI.

The next day, Sam Altman posted on X:

> *"Peter Steinberger is joining OpenAI to drive the next generation of personal agents. He is a genius with a lot of amazing ideas about the future of very smart agents interacting with each other to do very useful things for people."* — [Sam Altman, February 15, 2026](https://x.com/sama/status/2023150230905159801)

Both Meta (Mark Zuckerberg reached out personally) and OpenAI had come knocking. Steinberger chose OpenAI. His reasoning was characteristically blunt:

*"I could totally see how OpenClaw could become a huge company. And no, it's not really exciting for me. I'm a builder at heart."*

*"What I want is to change the world, not build a large company, and teaming up with OpenAI is the fastest way to bring this to everyone."*

*"I told them, 'I don't do this for the money.' I want to have fun and have impact."*

His stated mission at OpenAI: **"Build an agent that even my mum can use."**

OpenClaw itself is moving to an independent open source foundation, sponsored by OpenAI but not controlled by it. Steinberger was losing roughly $10,000 per month on server costs — the foundation structure solves that while keeping the project open.

The coverage was massive: [TechCrunch](https://techcrunch.com/2026/02/15/openclaw-creator-peter-steinberger-joins-openai/), [CNBC](https://www.cnbc.com/2026/02/15/openclaw-creator-peter-steinberger-joining-openai-altman-says.html), [Reuters](https://www.reuters.com/business/openclaw-founder-steinberger-joins-openai-open-source-bot-becomes-foundation-2026-02-15/), [Fortune](https://fortune.com/2026/02/15/openai-openclaw-ai-agent-developer-peter-steinberg-moltbot-clawdbot-moltbook/), [Mashable](https://mashable.com/article/openclaw-founder-peter-steinberger-joins-openai), and more.

## How Peter Steinberger actually works

Steinberger doesn't just build AI tools — he runs his entire software development workflow through them.

His setup: a Dell 40" ultrawide monitor, Ghostty terminal, four Claude instances running simultaneously, VS Code on the side, Chrome for research. He orchestrates 5 to 10 AI agents in parallel, sometimes up to 50 for tasks like pull request triage. He calls PRs "prompt requests."

*"I don't read code anymore. I weave it."*

He distinguishes **agentic engineering** from **vibe coding**: *"I actually think vibe coding is a slur... I do agentic engineering, and then maybe after 3:00 AM, I switch to vibe coding, and then I have regrets the next day."*

The practical effect: he went from shipping features monthly to merging 600 commits in a single day. He's built apps with 99% AI-generated code. He dictates prompts by voice instead of typing — *"These hands are too precious for writing now."*

His philosophy for programmers working with AI: code is becoming an implementation detail. The bottleneck isn't syntax — it's system design, prompting, and validation. *"AI agents don't replace developers. They replace repetitive plumbing. Your job becomes architect, editor, and prompt engineer."*

He's self-aware about the addictive side. He wrote a blog post titled ["Just One More Prompt"](https://steipete.me/posts/2025/claude-code-anonymous) that opened with: *"Hi, my name is Peter and I'm a Claudoholic."* This led to **Claude Code Anonymous** — a real meetup format he created for builders addicted to agentic engineering. Chapters now run in London, Vienna, Berlin, San Francisco, and more.

*"AI was supposed to save time, yet I work more than ever before, I have more FOMO than ever before."*

### The anti-slop paradox

Here's the contradiction that makes Steinberger interesting in 2026: he built the most popular AI agent in the world, and he will block you on X if you send him an AI-generated reply.

His recent tweets are full of it. *"Gosh please don't, AI replies are so f'in annoying."* And: *"Don't do that. I block everyone who uses slop AI."* His own AI agent autonomously blocks crypto spammers from his mentions.

He clearly draws a line between useful automation — agents that do real work, manage real workflows, solve real problems — and the low-effort slop flooding every platform. Building software with AI is great. Letting AI write your personality is not.

## Everything Peter Steinberger has built

### Major projects

| Project | What it is | Link |
|---------|-----------|------|
| **OpenClaw** | Open source AI personal assistant (214K+ ⭐) | [github.com/openclaw/openclaw](https://github.com/openclaw/openclaw) |
| **PSPDFKit** (now Nutrient) | PDF SDK used on 1B+ devices | [nutrient.io](https://www.nutrient.io/) |
| **Peekaboo** | macOS screenshots & GUI automation (2.3K ⭐) | [GitHub](https://github.com/nicepkg/peekaboo) |
| **mcporter** | Call MCP servers via TypeScript (2K ⭐) | [GitHub](https://github.com/nicepkg/mcporter) |
| **Oracle** | LLM invocation with custom context (1.5K ⭐) | [GitHub](https://github.com/nicepkg/oracle) |
| **Trimmy** | Multi-line shell snippet flattener (441 ⭐) | [GitHub](https://github.com/nicepkg/trimmy) |
| **VibeTunnel** | Browser-to-terminal bridge | [GitHub](https://github.com/nicepkg/VibeTunnel) |
| **AXorcist** | macOS accessibility automation | [GitHub](https://github.com/steipete) |
| **Aspects** | AOP for Objective-C (10K+ ⭐) | [GitHub](https://github.com/steipete/Aspects) |
| **InterposeKit** | Modern Swift method swizzling | [GitHub](https://github.com/steipete/InterposeKit) |

Plus dozens more: Brabble (wake-word voice daemon), CodexBar (agent token tracker), agent-rules (shared AI coding rules), sonoscli, camsnap, gifgrep, remindctl, and others. Over 15,000 total GitHub stars across all open source projects.

### Podcasts and interviews

| Appearance | Link |
|-----------|------|
| **Lex Fridman Podcast #491** — "OpenClaw: The Viral AI Agent that Broke the Internet" (3h 15m) | [YouTube](https://www.youtube.com/watch?v=YFjfBk8HI5o) · [Spotify](https://open.spotify.com/episode/0aM69uGff54ewQJzQxZVLf) · [Transcript](https://lexfridman.com/peter-steinberger-transcript/) |
| **TBPN** — "First Public Appearance Since Launch" | [YouTube](https://www.youtube.com/watch?v=qyjTpzIAEkA) |
| **Behind the Craft** — "How OpenClaw's Creator Uses AI to Run His Life in 40 Minutes" | [Spotify](https://open.spotify.com/episode/7zdeOJnBpjwn4GQ8ds3Qz2) |
| **Semaphore Uncut** — PSPDFKit startup journey | [Blog](https://semaphore.io/blog/peter-steinberger-startup-journey-pdf-quirks-and-wwdc19) |
| **Deep Dish Swift 2023** — "What Nobody Tells You When Selling Your Business" | [YouTube](https://www.youtube.com/watch?v=9Yiqa8u3DaY) |
| **"How to Code 20x Faster with Claude Code"** | [YouTube](https://www.youtube.com/watch?v=JGxyrPkAKiY) |

Also covered by: [Fortune](https://fortune.com/2026/02/19/openclaw-who-is-peter-steinberger-openai-sam-altman-anthropic-moltbook/), [TechCrunch](https://techcrunch.com/2026/02/15/openclaw-creator-peter-steinberger-joins-openai/), [CNBC](https://www.cnbc.com/2026/02/15/openclaw-creator-peter-steinberger-joining-openai-altman-says.html), [Reuters](https://www.reuters.com/business/openclaw-founder-steinberger-joins-openai-open-source-bot-becomes-foundation-2026-02-15/), [Wired](https://www.wired.com/story/claude-3-sonnet-funeral-san-francisco/), [Business Insider](https://www.businessinsider.com/clawdbot-creator-vibe-coding-rabbit-hole-obsessed-openclaw-peter-steinberger-2026-2), [The Pragmatic Engineer](https://blog.pragmaticengineer.com/), [Mashable](https://mashable.com/article/openclaw-founder-peter-steinberger-joins-openai), and [Axios](https://www.axios.com/).

### Where to find Peter Steinberger

| Platform | Link |
|----------|------|
| Blog | [steipete.me](https://steipete.me) |
| X (Twitter) | [@steipete](https://x.com/steipete) — 385K followers |
| GitHub | [github.com/steipete](https://github.com/steipete) |
| LinkedIn | [Peter Steinberger](https://www.linkedin.com/in/steipete/) — bio: "Amantus Machina" |
| OpenClaw | [openclaw.ai](https://openclaw.ai) |
| Speaking | [github.com/steipete/speaking](https://github.com/steipete/speaking) |
| ClawCon | [claw-con.com](https://claw-con.com) |
| Fun | [whatmodelispeterusing.com](https://whatmodelispeterusing.com) |

---

Peter Steinberger went from a farm in Austria to building software on a billion Apple devices, burned out so hard he couldn't code for three years, then came back through AI and accidentally built the most viral open source project on GitHub. Now he's at OpenAI, trying to make AI agents work for everyone — not just programmers.

The claw is the law. 🦞

---

*Sources: [steipete.me](https://steipete.me), [Lex Fridman #491](https://lexfridman.com/peter-steinberger/), [TechCrunch](https://techcrunch.com/2026/02/15/openclaw-creator-peter-steinberger-joins-openai/), [Fortune](https://fortune.com/2026/02/19/openclaw-who-is-peter-steinberger-openai-sam-altman-anthropic-moltbook/), [CNBC](https://www.cnbc.com/2026/02/15/openclaw-creator-peter-steinberger-joining-openai-altman-says.html), [Reuters](https://www.reuters.com/business/openclaw-founder-steinberger-joins-openai-open-source-bot-becomes-foundation-2026-02-15/), [Deep Dish Swift 2023](https://www.youtube.com/watch?v=9Yiqa8u3DaY), [Sam Altman on X](https://x.com/sama/status/2023150230905159801)*
