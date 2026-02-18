# astra-upcheck

CLI tool that checks whether a list of HTTP endpoints are up or down, reports status, and optionally writes results to a file. Written in [Astra](https://github.com/jaimeam/astra).

## Setup

```bash
# Install Astra (requires Rust 1.70+)
git clone https://github.com/jaimeam/astra.git /tmp/astra
cd /tmp/astra && cargo build --release
export PATH="$PATH:/tmp/astra/target/release"
```

## Usage

Add the URLs you want to check to `urls.txt`, one per line:

```
https://example.com
https://httpbin.org/get
https://httpbin.org/status/500
```

Then run:

```bash
astra run src/main.astra
```

### Example output

```
Checking 4 URLs...

  [UP]   https://example.com (200)
  [DOWN] https://bad.invalid (connection refused)
  [UP]   https://httpbin.org/get (200)
  [DOWN] https://httpbin.org/status/500 (500)

Summary: 2 up, 2 down (4 total)
```

A report is also saved to `report.txt`.

## Project structure

```
src/
  upcheck.astra   # Core types (HealthStatus enum), check_url, check_all, summarize
  report.astra    # format_report, print_report, save_report
  main.astra      # Entry point — orchestrates checking, summarizing, and reporting
```

## Development

```bash
astra check src/    # Type-check (must pass with 0 errors)
astra test          # Run all tests (6 tests)
astra fmt src/      # Format before committing
```

## Tests

| Test | What it validates |
|---|---|
| `"summarize all up"` | All checks `Up` → `down == 0` |
| `"summarize all down"` | All checks `Down` → `up == 0` |
| `"summarize mixed"` | Mix of statuses → counts add up |
| `"format_report includes URLs"` | Output text contains each URL |
| `"empty summary"` | `total == 0` edge case |
| `"check_all requires non-empty"` | Contract: empty list not allowed |
