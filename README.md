# ⬡ TokenScope — AI Prompt Token Analyzer

> **Estimate token consumption across ChatGPT, Claude, and Gemini — before you submit.**

A single-file, fully client-side web dashboard that analyzes your prompt in real-time and visualizes token usage, context window utilization, and cost projections across three major AI models — with zero dependencies to install.

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Zero Backend](https://img.shields.io/badge/backend-none-green.svg)
![Single File](https://img.shields.io/badge/build-single%20HTML%20file-orange.svg)
![Chart.js](https://img.shields.io/badge/charts-Chart.js%204.4-red.svg)

---

## ✨ Features

### 📝 Prompt Input Panel
- Large textarea with real-time analysis on every keystroke
- Live **character count**, **word count**, **sentence count**, **paragraph count**
- Adjustable **estimated output token slider** (0–8,000 tokens)

### 🔢 Token Estimation Engine
Estimates tokens for three models simultaneously using a BPE-style subword tokenization algorithm — all in-browser, no API calls:

| Model | Context Window | Tokenizer Approach |
|---|---|---|
| **GPT-5.5** | 400,000 tokens | BPE baseline (ratio 1.0×) |
| **Claude 4** | 200,000 tokens | BPE + slight overhead (1.03×) |
| **Gemini 2.5 Pro** | 1,000,000 tokens | SentencePiece approximation (0.94×) |

For each model, the dashboard displays:
- Input Tokens
- Estimated Output Tokens (user-configurable)
- Total Tokens
- Remaining Available Tokens
- **% of Context Window Used**

### 📊 Visualization Dashboard
- **Doughnut charts** — per-model context utilization at a glance
- **Grouped bar chart** — side-by-side input/output/total comparison across all models
- **Token breakdown bars** — input vs. output vs. free context, color-coded
- **Token Density Heatmap** — every word in your prompt is color-coded by estimated token weight (low = cyan, high = purple)
- **Green / Yellow / Red status badges** — safe (<70%), warn (<90%), danger (≥90%)
- **Overall status banner** — single consolidated pass/fail with explanation

### 💰 Cost Estimator
Enter your own pricing values ($ per 1M tokens) and daily request volume to see:
- Cost per request
- Daily usage cost
- Monthly usage cost

### 📤 Export
| Format | Contents |
|---|---|
| **CSV** | Prompt stats + full model breakdown in spreadsheet format |
| **JSON** | Structured data for downstream tooling or logging |
| **PDF** | Printable report via browser print dialog |
| **Clipboard** | One-click plain-text summary for pasting anywhere |

---

## 🚀 Quick Start

No build step. No `npm install`. No backend.

```bash
git clone https://github.com/YOUR_USERNAME/tokenscope.git
cd tokenscope
open token-counter.html   # macOS
# or just double-click the file in Windows/Linux
```

Or host it anywhere static — GitHub Pages, Netlify, Vercel, S3.

---

## 🗂 Project Structure

```
tokenscope/
├── token-counter.html   # Entire application — HTML + CSS + JS in one file
└── README.md
```

---

## 🧠 How Token Estimation Works

TokenScope uses a **client-side BPE (Byte Pair Encoding) approximation** that mirrors how real tokenizers like `tiktoken` (OpenAI) and `@anthropic-ai/tokenizer` behave:

```
Short words (1–4 chars)   → ~1 token
Medium words (5–8 chars)  → ~1.3 tokens
Long words (9–12 chars)   → ~1.8 tokens
Very long / technical     → ceil(length / 4.5) tokens
Numbers                   → ceil(digit_count / 3) tokens
Punctuation               → +0.4 tokens each
Newlines                  → +0.5 tokens each
```

Per-model ratios are then applied to match real-world divergence between tokenizers:
- GPT-5.5: `1.0×` (baseline)
- Claude 4: `1.03×` (slightly higher for non-ASCII / markdown)
- Gemini 2.5 Pro: `0.94×` (SentencePiece is more efficient on common English)

**Note:** This is an estimation. For exact counts, use the official tokenizer APIs:
- OpenAI: [`tiktoken`](https://github.com/openai/tiktoken)
- Anthropic: [`@anthropic-ai/tokenizer`](https://www.npmjs.com/package/@anthropic-ai/tokenizer)
- Google: SentencePiece via Vertex AI

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Markup | HTML5 |
| Styling | Vanilla CSS (custom design system, no Tailwind needed) |
| Logic | Vanilla JavaScript (ES2020+) |
| Charts | [Chart.js 4.4](https://www.chartjs.org/) via CDN |
| Fonts | Space Grotesk + JetBrains Mono via Google Fonts |
| Backend | **None** — fully client-side |

---

## 📐 Context Window Reference

| Model | Context Window |
|---|---|
| GPT-5.5 | 400,000 tokens |
| Claude 4 | 200,000 tokens |
| Gemini 2.5 Pro | 1,000,000 tokens |

---

## 🎨 Design System

TokenScope uses a dark-mode-first design with a custom token system:

```css
--bg:        #070C1A   /* Deep navy background      */
--surface:   #0F1628   /* Card surfaces              */
--cyan:      #00D4FF   /* Primary accent (input tok) */
--purple:    #9B59FF   /* Output token accent        */
--green:     #00E5A0   /* Safe status                */
--amber:     #FFB800   /* Warning status             */
--red:       #FF3B5C   /* Danger / over-limit        */
```

---

## 🖥 Screenshots

### Main Dashboard
- Left: Prompt input + live statistics + output slider
- Right: Overall status + grouped comparison bar chart

### Model Cards (GPT-5.5 / Claude 4 / Gemini 2.5 Pro)
Each card shows doughnut chart, token breakdown, progress bar, and status badge.

### Token Density Heatmap
Words in your prompt are color-weighted by estimated token complexity — helping you identify where token "weight" is concentrated.

---

## 🙌 Contributing

Pull requests welcome. Ideas for contributions:
- Add more models (Llama 3, Mistral, DeepSeek, Grok)
- Integrate actual `tiktoken` WASM build for GPT exact counts
- Add conversation history mode (multi-turn token accumulation)
- Dark/light mode toggle
- Prompt history / session storage

---

## 📄 License

MIT — free to use, modify, and distribute.

---

## 🔗 Related Projects

- [opencode-tokenscope](https://github.com/ramtinJ95/opencode-tokenscope) — Token tracking plugin for OpenCode sessions
- [Tokdash](https://github.com/JingbiaoMei/tokdash) — LLM API usage analytics dashboard
- [tiktoken](https://github.com/openai/tiktoken) — Official OpenAI tokenizer

---

*Built with ⬡ TokenScope — All calculations performed locally in-browser. No data leaves your device.*
