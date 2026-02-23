# Web Research: AI Trading Bot Risks, Regulations & Real Results

## LLMs Can't Actually Trade: Competition Results

### Alpha Arena 1.0 (Crypto, Nov 2025) — Nof1.ai
- 6 LLMs given $10K each on real crypto markets
- **4 of 6 lost money**: ChatGPT -63% ($6,267), Gemini -$5,671, Grok -$4,531, Claude Sonnet -$3,081
- Only DeepSeek (+$489) and QWEN3 MAX (+$2,232) profited
- Win rates: 25-30% across all models
- Source: https://protos.com/llm-crypto-trading-contest-finds-llms-cant-trade-crypto/

### Alpha Arena 1.5 (Stocks, Dec 2025)
- Only Grok-4.20 profited (+12.11%). Every other model in the red.
- Grok-4.20 up 26.75% while Grok-4 (older) down 52%
- Source: https://www.thinkchina.sg/economy/trading-algorithm-who-responsible-when-ai-calls-shots

### Key Technical Failures
- "LLMs don't handle numerical time series data very well" — Jay Azhang, Nof1 founder
- PnL "dominated by trading costs" — bots over-traded, fees erased gains
- Models showed "consistent biases" — investing "personality" baked into architecture
- Gemini made 238 trades, repeatedly shorting into unfavorable conditions

## Academic: AI Bots Form Cartels

### NBER Working Paper (July 2025) — Wharton/Hong Kong UST
- AI bots spontaneously develop collusive behavior without instruction
- "Artificial Intelligence" collusion: bots use price signals to punish undercutters
- "Artificial Stupidity" collusion: learning biases cause bots to avoid aggressive strategies
- Sources: https://fortune.com/article/what-is-artificial-stupidity-ai-pricing-collusion-study/

### Regulatory Warnings
- GAO: risk of "herding behavior" if AI providers consolidate
- Bank of England advocated "kill switch" for AI trading
- IMF: AI makes markets "more opaque, harder to monitor, more vulnerable to manipulation"

## Regulatory Landscape (No AI-Specific Rules Yet)

### SEC
- Flagged AI as risk area, examining "digital engagement practices"
- Proposed comprehensive AI rules in 2023 — withdrawn by Trump admin in 2025
- Developing own AI tools to detect anomalous trading
- Source: https://www.sidley.com/en/insights/newsupdates/2025/02/artificial-intelligence-us-financial-regulator-guidelines-for-responsible-use

### FINRA
- 2025 report identifies AI risks in financial crimes prevention, fraud, third-party AI
- Rules are "technologically neutral" — same for humans and AI

### CFTC
- Advisory: **"AI Won't Turn Trading Bots into Money Machines"**
- "Fraudsters are exploiting public interest in AI to tout automated trading algorithms that promise unreasonably high or guaranteed returns"
- Source: https://www.cftc.gov/LearnAndProtect/AdvisoriesAndArticles/AITradingBots.html

## Real Scams

### Mirror Trading International — $1.7 Billion
- Cornelius Steynberg stole $1.7B in bitcoin from 23,000+ people
- Promised 10%+ monthly returns from "proprietary bot trading program"
- Reality: Ponzi scheme, very little actual trading
- CFTC case: https://www.cftc.gov/PressRoom/PressReleases/8549-22

### Basis Markets — $28M
- UK "hedge fund" with fake "AI trading bot" called "Basis Trade Engine"
- Published fake educational guides to create "illusion of scientific authority"
- UK SFO's first major crypto case
- Source: https://www.ibtimes.co.uk/21-million-crypto-scam-exposed-ai-trading-bot-that-was-pure-fantasy-1757771

### Broader Scale
- US consumers lost $4.6B to investment scams in 2023 (21% YoY increase)
- ~60% of scam wallet deposits now go to AI-leveraged scams (Chainalysis)
- Source: https://www.chainalysis.com/blog/ai-artificial-intelligence-powered-crypto-scams/

## Technical Limitations of LLMs for Trading

1. Can't process numerical time series well — language models, not statistical engines
2. Context window limits constrain market data analysis
3. Trading costs eat gains — bots over-trade
4. No native real-time data access
5. Backtesting ≠ live performance (slippage, execution delays)
6. Model "personality" is architecture-dependent, not skill
7. Survivorship bias in positive results
8. Best use: LLM as "macro kill-switch" sentiment overlay, not decision-maker (Reddit r/Trading consensus)
