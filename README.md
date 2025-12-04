# LegiScan Bill Search Tool

This tool searches the LegiScan API for bills across all US states based on keywords and date ranges.

## Features

- **Two search methods**: Bulk Dataset API (fast) or Live Search API (up-to-date)
- Search all 50 states for bills matching your keywords
- Filter for passed/enacted bills only
- Interactive date range filtering
- Downloads bill texts in parsable formats (prefers HTML over PDF)
- Outputs results to CSV with all requested fields
- De-duplicates bills found by multiple keywords

## Search Methods

### Bulk Dataset API (Recommended)
- Downloads entire legislative sessions as datasets
- Searches locally for matching bills
- **Much faster** for searches spanning multiple years
- Caches datasets for repeated searches
- Best for: Historical searches (2010-present)

### Live Search API
- Searches the live LegiScan API
- Fetches each bill individually
- More up-to-date but slower
- Best for: Recent bills only (current year)

## Installation

This tool uses a self-contained virtual environment. You don't need to worry about dependencies!

**One-time setup:**
```bash
./setup.sh
```

That's it! The setup script creates a virtual environment and installs all dependencies.

## Usage

1. **Prepare your keywords file**: Create a text file with one keyword/phrase per line (see `keywords_example.txt`)

2. **Run the tool**:
   ```bash
   ./run.sh
   ```

3. **Choose your search method**:
   - Option 1: Bulk Dataset API (recommended for most searches)
   - Option 2: Live Search API (for very recent bills)

4. **Follow the prompts**:
   - Enter your LegiScan API key
   - Enter the path to your keywords file
   - Enter starting year (e.g., 2020) - will search from that year to present

5. **Wait for results**: The script will:
   - Search for each keyword across all states
   - Download full bill details
   - Download bill texts (preferring HTML format)
   - Save everything to the `output` directory

## Output

The tool creates an `output` directory containing:

- **CSV file**: `bill_search_results_TIMESTAMP.csv` with columns:
  - State
  - Bill Number
  - Title
  - LegiScan Bill Main Page URL
  - LegiScan Text Page URL
  - Status
  - Last Action Date (ISO format: YYYY-MM-DD)
  - Bill Text File

- **bill_texts/** subdirectory: Contains downloaded bill texts named as `STATE_BILLNUMBER.ext`

## Keywords File Format

```
# Lines starting with # are comments
artificial intelligence
data privacy
education funding
```

## Notes

- The API has rate limiting - the script includes delays to be respectful
- Public API keys have a 30,000 query/month limit
- The script filters results by the last action date within your specified range
- Bill texts are downloaded in the most parsable format available (HTML preferred)
- Duplicate bills found by multiple keywords are only included once

### Date Range Handling

The LegiScan API has limitations on historical data:
- Bills are available from **2010 onwards** with reliable data
- For date ranges spanning 10+ years, the script searches the entire database
- For narrower ranges, it searches year-by-year for better results
- Results are then filtered to your exact start/end dates

**Recommended date ranges:**
- Recent bills: 2020-present
- Comprehensive search: 2010-present
- Specific year: e.g., 2023-01-01 to 2023-12-31

## Example

**First time:**
```bash
$ ./setup.sh
==================================
LegiScan Tool Setup
==================================
Using Python: Python 3.11.5
Creating virtual environment...
✓ Virtual environment created
Installing dependencies...
✓ Dependencies installed
==================================
Setup complete!
==================================
```

**Every time you run it:**
```bash
$ ./run.sh
Enter your LegiScan API key: your_key_here
Enter path to keywords file: keywords_example.txt

Enter starting year (will search from that year to present)
Starting year (e.g., 2020): 2020
Searching from 2020-01-01 to 2025-11-24

Searching for 5 keywords in all states...
...
✓ Wrote 150 bills to output/bill_search_results_20251124_143022.csv
```

## Troubleshooting

- **"API Error"**: Check that your API key is valid
- **"No bills found"**: Try widening your date range or using broader keywords
- **Rate limiting**: The script includes delays; if you hit limits, wait and retry
- **Missing bill texts**: Some bills may not have HTML versions; PDF will be downloaded instead
