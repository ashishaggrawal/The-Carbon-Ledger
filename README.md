# The Carbon Ledger

An AI auditor that reads a business's transaction ledger and estimates the carbon impact of each expense. Built for the Devpost Gemini 3 Hackathon.

Given a CSV of transactions, it identifies the highest carbon liabilities, such as air-freighted produce or large-vehicle travel, and returns a **Carbon Correction Plan** in JSON with concrete alternatives.

## How It Works

The script is structured as a three-stage agent loop:

1. **Sense:** uploads the transaction CSV to the Gemini Files API with an explicit `text/csv` MIME type.
2. **Reason:** sends the file to `gemini-3-pro-preview` with thinking enabled (`ThinkingConfig(include_thoughts=True)`), instructing it to audit with strict honesty and no sugar-coating.
3. **Act:** prints the audit result and the JSON correction plan.

The script resolves file paths relative to itself and stops immediately with a clear error if the API key or input file is missing.

## Sample Input

[`test_audit.csv`](test_audit.csv) contains a small ledger mixing high- and low-impact purchases:

| Date | Description | Amount | Category |
|------|-------------|--------|----------|
| 2025-12-01 | Uber - Airport Trip (XL SUV) | 85.00 | Mobility |
| 2025-12-05 | Amazon - 2lb Fresh Strawberries (Air-Shipped) | 24.99 | Procurement |
| 2025-12-10 | Local Farmer Market - Seasonal Veggies | 45.00 | Sustenance |
| 2025-12-15 | Generic Industrial Part #102 | 150.00 | Procurement |

## Running It

Requires Python 3.9+ and a [Gemini API key](https://aistudio.google.com/apikey).

```bash
git clone https://github.com/ashishaggrawal/The-Carbon-Ledger.git
cd The-Carbon-Ledger
pip install google-genai python-dotenv
echo "GEMINI_API_KEY=your-key-here" > .env
python main.py
```

The `.env` file is listed in `.gitignore`, so the API key is never committed.

## Tech Stack

- Python
- Google Gemini 3 (`google-genai` SDK) with extended thinking
- Gemini Files API for document upload
