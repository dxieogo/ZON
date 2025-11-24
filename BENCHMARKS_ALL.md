# Unified Benchmark Results

This file contains the consolidated benchmark results (JSON vs ZON) and a focused comparison with TOON for the `hikes` sample used during analysis.

The token counts were computed using `tiktoken` with the `o200k_base` tokenizer (GPT-5 style) and `cl100k_base` for cross-checking TOON's published numbers.

Below is the raw token analysis output used for the documentation and comparison.

```
====================================================================================================
  TOKEN ANALYSIS - Using GPT-5 Tokenizer (o200k_base)
====================================================================================================

This matches TOON's official benchmark methodology
Reference: https://github.com/toon-format/toon#benchmarks

────────────────────────────────────────────────────────────────────────────────────────────────────
Format               |    Bytes |   o200k_base |  cl100k_base |    p50k_base
────────────────────────────────────────────────────────────────────────────────────────────────────
JSON (formatted)     |      680 |          229 |          235 |          266
JSON (compact)       |      451 |          139 |          142 |          148
ZON                  |      264 |           96 |           98 |          111
TOON                 |      286 |          104 |          106 |          115

====================================================================================================
  TOON'S CLAIMED NUMBERS vs ACTUAL
====================================================================================================

JSON (formatted):
  TOON claims: 235 tokens
  o200k_base:  229 tokens
  cl100k_base: 235 tokens
  p50k_base:   266 tokens

TOON:
  TOON claims: 106 tokens
  o200k_base:  104 tokens
  cl100k_base: 106 tokens
  p50k_base:   115 tokens

ZON:
  o200k_base:  96 tokens
  cl100k_base: 98 tokens
  p50k_base:   111 tokens

====================================================================================================
  COMPRESSION ANALYSIS (using o200k_base)
====================================================================================================

Format               |   Tokens |    Reduction
──────────────────────────────────────────────────
JSON (formatted)     |      229 |         0.0%
JSON (compact)       |      139 |        39.3%
ZON                  |       96 |        58.1%
TOON                 |      104 |        54.6%

====================================================================================================
  ZON vs TOON
====================================================================================================

Token count:
  TOON: 104
  ZON:  96
  Difference: 8 tokens
  ZON is 7.7% fewer tokens

====================================================================================================
  FULL TEXT OUTPUT
====================================================================================================

JSON (formatted) (229 tokens, 680 bytes):
────────────────────────────────────────────────────────────────────────────────────────────────────
{
  "context": {
    "task": "Our favorite hikes together",
    "location": "Boulder",
    "season": "spring_2025"
  },
  "friends": [
    "ana",
    "luis",
    "sam"
  ],
  "hikes": [
    {
      "id": 1,
      "name": "Blue Lake Trail",
      "distanceKm": 7.5,
      "elevationGain": 320,
      "companion": "ana",
      "wasSunny": true
    },
    {
      "id": 2,
      "name": "Ridge Overlook",
      "distanceKm": 9.2,
      "elevationGain": 540,
      "companion": "luis",
      "wasSunny": false
    },
    {
      "id": 3,
      "name": "Wildflower Loop",
      "distanceKm": 5.1,
      "elevationGain": 180,
      "companion": "sam",
      "wasSunny": true
    }
  ]
}

JSON (compact) (139 tokens, 451 bytes):
────────────────────────────────────────────────────────────────────────────────────────────────────
{"context":{"task":"Our favorite hikes together","location":"Boulder","season":"spring_2025"},"friends":["ana","luis","sam"],"hikes":[{"id":1,"name":"Blue Lake Trail","distanceKm":7.5,"elevationGain":320,"companion":"ana","wasSunny":true},{"id":2,"name":"Ridge Overlook","distanceKm":9.2,"elevationGain":540,"companion":"luis","wasSunny":false},{"id":3,"name":"Wildflower Loop","distanceKm":5.1,"elevationGain":180,"companion":"sam","wasSunny":true}]}

ZON (96 tokens, 264 bytes):
────────────────────────────────────────────────────────────────────────────────────────────────────
context:"{task:Our favorite hikes together,location:Boulder,season:spring_2025}"
friends:"[ana,luis,sam]"

@hikes(3):companion,distanceKm,elevationGain,id,name,wasSunny
ana,7.5,320,1,Blue Lake Trail,T
luis,9.2,540,2,Ridge Overlook,F
sam,5.1,180,3,Wildflower Loop,T

TOON (104 tokens, 286 bytes):
────────────────────────────────────────────────────────────────────────────────────────────────────
context:
  task: Our favorite hikes together
  location: Boulder
  season: spring_2025
friends[3]: ana,luis,sam
hikes[3]{id,name,distanceKm,elevationGain,companion,wasSunny}:
  1,Blue Lake Trail,7.5,320,ana,true
  2,Ridge Overlook,9.2,540,luis,false
  3,Wildflower Loop,5.1,180,sam,true

====================================================================================================

```

Summary (hikes sample):

| Format | Bytes | o200k_base tokens | Reduction vs JSON (formatted) |
|---|---:|---:|---:|
| JSON (formatted) | 680 B | 229 | 0.0% |
| JSON (compact) | 451 B | 139 | 39.3% |
| ZON | 264 B | 96 | 58.1% |
| TOON | 286 B | 104 | 54.6% |

Notes:
- Token counts use `o200k_base` unless otherwise noted. TOON's published numbers align with `cl100k_base` in this example.
- The ZON encoding for the hikes sample is smaller than TOON by 22 bytes and 8 tokens (7.7% fewer tokens).
