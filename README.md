# Wirtschafts-Briefing

Wirtschafts-Briefing generates a German-language economic briefing on Tuesdays. It combines news summaries from RSS feeds with a market snapshot and can save a PDF, send an HTML email, or do both.

## Features

- Collects recent RSS items across general economics, finance, crypto, forex, technology, and commodities. It removes duplicate titles within each category.
- Uses Claude to select up to three stories per category and summarize their main points and possible implications.
- Uses yfinance to report the latest available closing prices and percentage changes from the previous close for configured indices, cryptocurrencies, commodities, and currency pairs.
- Writes a timestamped PDF and sends an HTML briefing via SMTP. Each output can be enabled independently.

## Requirements

- Python 3.11, the version used by GitHub Actions
- Internet access to the RSS feeds, Anthropic API, and Yahoo Finance
- An Anthropic API key and, for email delivery with the default Gmail SMTP settings, a Gmail app password

## Setup

```bash
git clone git@github.com:Bene77-w/wirtschafts-briefing.git
cd wirtschafts-briefing
python3.11 -m venv venv
source venv/bin/activate
python -m pip install -r requirements.txt
```

## Configuration

Set these environment variables in your shell or in a local `.env` file in the repository root.

| Variable | Purpose |
| --- | --- |
| `ANTHROPIC_API_KEY` | Required for Claude ranking and summarization. |
| `EMAIL_PASSWORD` | SMTP password for the configured sender. Required when email delivery is enabled. |

Edit [config.yaml](config.yaml) before running. Both outputs are enabled in the supplied configuration. For email delivery, set `email.sender` and `email.recipient` and adjust the SMTP host and port as needed. For PDF-only runs, set `output.send_email` to `false`.

Use `rss_feeds`, `news_hours_filter`, and `news_count.raw` to change the feed URLs, article age window, and candidate limit per category. The supplied settings use a 24-hour window and up to six candidates per category. Use `market_symbols` to change the yfinance tickers and `output.generate_pdf` and `output.pdf_path` to control PDF generation and its destination.

## Usage

From the repository root, run:

```bash
python main.py
```

The script uses the machine's local date and exits without generating a briefing on days other than Tuesday. With PDF generation enabled, the default output is `briefings/wirtschafts-briefing_YYYY-MM-DD_HH-MM.pdf`.

### GitHub Actions

The [workflow](.github/workflows/daily-briefing.yml) is scheduled daily at 18:00 UTC. You can also start it manually under **Actions → Daily Economic Briefing → Run workflow**. Both triggers follow the script's Tuesday check.

Set `ANTHROPIC_API_KEY` and, if email is enabled, `EMAIL_PASSWORD` as repository Actions secrets. Set the sender and recipient in `config.yaml`. The workflow's `GMAIL_SENDER` secret only replaces the literal placeholder `deine.email@gmail.com` when that placeholder is used as the sender.

## Project structure

| Path | Purpose |
| --- | --- |
| `main.py` | Entry point and Tuesday check |
| `config.yaml` | Feed, market, email, and output settings |
| `modules/` | RSS and market data retrieval, Claude processing, and PDF and email output |
| `templates/email_template.html` | HTML email layout |
| `.github/workflows/daily-briefing.yml` | Scheduled and manual runs |

## License

[MIT License](LICENSE).
