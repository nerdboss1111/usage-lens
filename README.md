# Usage Lens

See where your AI credits actually go.

Every LLM platform shows you a bar going down and never tells you what moved it. Usage Lens breaks down your consumption by category, automation, and conversation, so you can stop guessing and start saving.

## What it shows

- **Total usage** with live countdown to your next reset
- **Per-category breakdown**: automations, side chats, main chat, model calls
- **Per-automation costs**: which scheduled jobs and background tasks burn the most, with token-hog flagging
- **Per-chat context tax**: how much history each conversation re-reads on every call (this is the silent killer)
- **Model comparison**: burn rate by model tier, so you know what switching actually costs
- **Recommendations**: concrete actions with projected weekly savings

## The finding that started this

One side chat re-reading 182.4k tokens of history per call. 30.1M tokens per week. Just re-reading old context. Background automations eating 67.7% of total usage. Nobody's dashboard showed any of this.

## Supported platforms

| Platform | Data source | Status |
|----------|-------------|--------|
| Muse | Manual entry + estimation | Working |
| ChatGPT | Manual entry + estimation | Working |
| Claude | Manual entry + estimation | Working |
| Gemini | Manual entry + estimation | Working |

No platform offers a consumer usage API with per-task data. Every platform requires manual entry for now. A browser extension that reads your own usage panel is on the roadmap. Contributions welcome.

## Quick start

1. Clone the repo
2. Open `index.html` in your browser (no build step, no dependencies)
3. Enter your current usage numbers from your platform's usage panel
4. Add your automations and active chats
5. See where it is all going

Your data never leaves your browser. Everything is stored in LocalStorage.

## How the estimates work

See [ESTIMATION.md](ESTIMATION.md) for the full math. Every estimate is labeled as an estimate, and you can override any number.

## Pro tier

Usage Lens is free and open source forever. A hosted pro tier is planned with:

- Historical trends (week over week, month over month)
- Usage alerts before you hit limits
- Auto-optimization suggestions
- Team and organization views

Pro funds continued development of the free tier.

## Contributing

Issues and PRs welcome. The data estimation heuristics especially need real-world calibration across platforms.

## License

MIT. See [LICENSE](LICENSE).
