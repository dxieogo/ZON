# Supplementary Benchmark: Hikes Sample

This supplement contains the compact comparison used in the QA process for the `hikes` dataset.

Summary (o200k_base tokenizer):

| Format | Bytes | Tokens | Reduction vs JSON (formatted) |
|---|---:|---:|---:|
| JSON (formatted) | 680 B | 229 | 0.0% |
| JSON (compact) | 451 B | 139 | 39.3% |
| ZON | 264 B | 96 | 58.1% |
| TOON | 286 B | 104 | 54.6% |

Conclusion: ZON is smaller than TOON for the `hikes` sample by 22 bytes and 8 tokens (7.7% fewer tokens using `o200k_base`).

Place this content into `QA_REPORT.md` under the Benchmarks section if you want the report updated in-place. I can apply the edit if you confirm.
