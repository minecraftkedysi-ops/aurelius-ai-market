# Market Watch AI Bot

**A configurable, AI-powered market analysis bot that watches the tickers you choose and sends them to your chosen model on a schedule.**

Set your own provider, API key, model, symbols, analysis prompt, and interval. Gemini 3.6 Flash is the default. The bot fetches recent market data, asks the model to interpret it, and prints the result in your terminal.

> **No order execution.** This project is for educational research only. It does not buy or sell assets, provide reliable predictions, or replace professional financial advice. AI output can be wrong or stale.

## Features

- Gemini and OpenAI-compatible chat API providers
- Configurable model, symbols, interval, and analysis prompt
- Recent OHLCV price data for context
- API keys read from environment variables; never put secrets in `config.json`
- One-shot mode for testing and continuous scheduled mode
- Clear errors and graceful handling of API rate limits
- A Claude Code skill for configuring and using the bot

## Quick start

Requires Python 3.10 or newer.

```bash
git clone https://github.com/minecraftkedysi/market-watch-ai-bot.git
cd market-watch-ai-bot
python -m venv .venv
```

Activate the environment:

```bash
# macOS / Linux
source .venv/bin/activate

# Windows PowerShell
.venv\Scripts\Activate.ps1
```

Install dependencies and prepare configuration:

```bash
pip install -r requirements.txt
cp config.example.json config.json
```

On Windows, use `copy config.example.json config.json` instead of `cp`.

Set the key for your selected provider:

```bash
# Gemini (default provider)
export GEMINI_API_KEY="your-key"

# Windows PowerShell
$env:GEMINI_API_KEY="your-key"
```

For the OpenAI-compatible provider, set `OPENAI_API_KEY` (and edit `base_url` if needed). Then run one analysis:

```bash
python bot.py --once
```

Or keep watching at the configured interval:

```bash
python bot.py
```

## Configuration

Edit `config.json` (copy it from `config.example.json` first):

| Setting | Purpose |
| --- | --- |
| `provider` | `gemini` or `openai_compatible` |
| `model` | Model identifier. Default: `gemini-3.6-flash` |
| `base_url` | OpenAI-compatible chat completions URL; ignored by Gemini |
| `symbols` | Yahoo Finance ticker symbols, such as `AAPL`, `MSFT`, or `BTC-USD` |
| `interval_minutes` | Delay between analyses; default is 5 minutes |
| `lookback_period` | Market history window, for example `5d` or `1mo` |
| `prompt` | Your instructions for the model |

Change the interval to respect your provider's current quota. A five-minute interval means **288 model requests per day per symbol batch**, not one request per day. Free-tier rate and daily limits vary by account, model, and region; this schedule does not guarantee that you will stay within them. Rate-limit responses are reported and the bot waits for the next scheduled cycle.

Ticker data is retrieved through `yfinance` and may be delayed, incomplete, or unavailable. Verify prices with your broker or exchange before making decisions.

## CLI options

```text
python bot.py --once            Run a single analysis and exit
python bot.py --config FILE      Use another JSON config file
python bot.py --interval 10      Override interval (minutes)
```

Press `Ctrl+C` to stop continuous mode.

## Environment variables

| Variable | Used by |
| --- | --- |
| `GEMINI_API_KEY` | Gemini provider |
| `OPENAI_API_KEY` | OpenAI-compatible provider |
| `OPENAI_BASE_URL` | Optional override for the compatible API URL |

## Claude Code skill

This repo includes `.claude/skills/market-watch/SKILL.md`. Claude Code can use it to help configure the bot, explain its output, and troubleshoot setup. It does not authorize or enable trade execution.

## Safety and limitations

- No brokerage integration or order placement exists in this project.
- Model output is not a forecast guarantee or individualized financial advice.
- Never paste API keys into prompts, commit them, or share `config.json` if you add secrets to it.
- Check the model provider's current quotas and terms. The project cannot prevent provider-side limits.
- Market data availability and accuracy are outside this project's control.

## Contributing

Bug reports and focused pull requests are welcome. Keep secrets out of commits, document provider-specific configuration, and preserve the no-order-execution behavior.

## License

MIT. See [LICENSE](LICENSE).

---

Created by **minecraftkedysi**.
