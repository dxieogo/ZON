---
description: Run ZON format benchmarks
---

# Run ZON Format Benchmarks

This workflow runs performance benchmarks comparing ZON format against JSON and TOON formats.

## Prerequisites

Ensure you have the ZON package installed in your environment.

## Steps

1. Navigate to the ZON format directory:
```bash
cd /Users/roni/Developer/ZON/zon-format
```

// turbo
2. Run the benchmark script:
```bash
python benchmarks/run.py
```

## What Gets Benchmarked

The benchmark tests the following datasets from `benchmarks/data/`:
- `github-repos.json` - GitHub repository data
- `employees.json` - Employee records
- `analytics.json` - Analytics data
- `orders.json` - Order data
- `complex_nested.json` - Complex nested structures

## Output

The benchmark will display:
- **JSON Size** - Original JSON serialization size (bytes)
- **ZON Size** - ZON serialization size (bytes)
- **TOON Size** - TOON serialization size (bytes)
- **ZON vs JSON** - Percentage size reduction compared to JSON
- **ZON vs TOON** - Percentage size comparison with TOON format

## Running Benchmarks on Custom Data

If you want to benchmark custom JSON files:

1. Place your JSON files in `benchmarks/temp/` directory
2. Run the alternative benchmark script:
```bash
python benchmarks/run_temp.py
```

This will benchmark all `.json` files found in the `benchmarks/temp/` directory.
