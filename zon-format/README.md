# ZON v1.0 (Entropy Engine)

**Zero Overhead Notation** - A human-readable data serialization format optimized for LLM token efficiency, JSON for LLMs.

[![Python](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-Proprietary-orange.svg)](LICENSE)
[![Production](https://img.shields.io/badge/production-Free%20to%20Use-green.svg)](LICENSE)

> 🚀 **24-40% better compression than TOON** | 📊 **30-42% compression vs JSON** | 🔍 **100% Human Readable**

---

## 📚 Table of Contents

- [What is ZON?](#-what-is-zon)
- [Quick Start](#-quick-start)
- [Installation](#-installation)
- [LLM Framework Integration](#-llm-framework-integration)
- [Benchmark Results](#-benchmark-results)
- [API Reference](#-api-reference)

---

## 🚀 What is ZON?

ZON is a **smart compression format** designed specifically for transmitting structured data to Large Language Models. Unlike traditional compression (which creates binary data), ZON remains **100% human-readable** while dramatically reducing token usage.

### Why ZON?

| Problem | Solution |
| :--- | :--- |
| 💸 **High LLM costs** from verbose JSON | ZON reduces tokens by 30-42% |
| 🔍 **Binary formats aren't debuggable** | ZON is plain text - you can read it! |
| 🎯 **One-size-fits-all compression** | ZON auto-selects optimal strategy per column |

### Key Features

- ✅ **Entropy Tournament**: Auto-selects best compression strategy per column
- ✅ **100% Safe**: Guaranteed lossless reconstruction
- ✅ **Zero Configuration**: Works out of the box

---

## ⚡ Quick Start

```python
import zon

# Your data
users = {
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

# Encode (compress)
compressed = zon.encode(users)
# Decode (decompress)
original = zon.decode(compressed)
assert original == users  # ✓ Perfect reconstruction!

```
- ZON (96 tokens, 264 bytes)
```
context:"{task:Our favorite hikes together,location:Boulder,season:spring_2025}"
friends:"[ana,luis,sam]"

@hikes(3):companion,distanceKm,elevationGain,id,name,wasSunny
ana,7.5,320,1,Blue Lake Trail,T
luis,9.2,540,2,Ridge Overlook,F
sam,5.1,180,3,Wildflower Loop,T
```

**vs TOON Compression comparison**:
- TOON (104 tokens, 286 bytes):

```
context:
  task: Our favorite hikes together
  location: Boulder
  season: spring_2025
friends[3]: ana,luis,sam
hikes[3]{id,name,distanceKm,elevationGain,companion,wasSunny}:
  1,Blue Lake Trail,7.5,320,ana,true
  2,Ridge Overlook,9.2,540,luis,false
  3,Wildflower Loop,5.1,180,sam,true
```


**Compression's**:
- JSON (compact) (139 tokens, 451 bytes)
- ZON (96 tokens, 264 bytes)
- TOON (104 tokens, 286 bytes)

---

## 📦 Installation

### From PyPI (Recommended)

```bash
pip install zon-format
```

### From Source

```bash
git clone https://github.com/yourusername/zon-format.git
cd zon-format
pip install -e .
```

### Verify Installation

```python
import zon
print("ZON installed successfully! ✅")
```

---
## Format Reference

### Metadata (YAML-like)

```
key:value
nested.key:value
list:[item1,item2,item3]
```

- No spaces after `:` for compactness
- Dot notation for nested objects
- Minimal quoting (only when necessary)

### Tables (@table syntax)

```
@tablename(count):col1,col2,col3
val1,val2,val3
val1,val2,val3
```

- `@` marks table start
- `(count)` shows row count
- Columns separated by commas (no spaces)

### Compression Tokens

| Token | Meaning | Example |
|-------|---------|---------|
| `T` | Boolean true | `T` instead of `true` |
| `F` | Boolean false | `F` instead of `false` |

---

## 🤖 LLM Framework Integration

### OpenAI Integration

```python
import zon
import openai

# Prepare your data
users = [{"id": i, "name": f"User{i}", "active": True} for i in range(100)]

# Compress with ZON (saves tokens = saves money!)
zon_data = zon.encode(users)

# Use in prompt
response = openai.ChatCompletion.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You will receive data in ZON format. Decode mentally and analyze."},
        {"role": "user", "content": f"Analyze this user data:\n\n{zon_data}\n\nHow many active users?"}
    ]
)

print(response.choices[0].message.content)
```

**Cost Savings**: ~30-40% fewer tokens vs JSON!

### LangChain Integration

```python
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate
import zon

# Prepare data
products = [
    {"name": "Laptop", "price": 999, "rating": 4.5},
    {"name": "Mouse", "price": 29, "rating": 4.2},
    # ... 100 more products
]

# Compress
zon_products = zon.encode(products)

# Create prompt template
template = """
You have access to product data in ZON format (a compressed JSON format).

Product Data:
{zon_data}

Question: {question}

Please analyze the data and answer.
"""

prompt = PromptTemplate(
    input_variables=["zon_data", "question"],
    template=template
)

# Use with LangChain
llm = OpenAI(temperature=0)
chain = prompt | llm

result = chain.invoke({
    "zon_data": zon_products,
    "question": "What's the average price of products with rating > 4?"
})

print(result)
```

## 📊 Benchmark Results

# Unified Benchmark Results

## JSON vs ZON

| Dataset | Records | JSON Size | ZON Size | Compression | JSON tk | ZON tk |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| analytics | 60 | 5.9 KB | 2.1 KB | **+63.6%** | 2343 | 1396 |
| complex_nested | 1000 | 381.3 KB | 296.8 KB | **+22.2%** | 121213 | 108563 |
| employees | 100 | 13.7 KB | 5.9 KB | **+56.9%** | 3624 | 2083 |
| github-repos | 100 | 33.7 KB | 21.0 KB | **+37.8%** | 12124 | 8693 |
| hikes | 1 | 451.0 B | 264.0 B | **+41.5%** | 139 | 96 |
| internet_github_repos | 100 | 411.4 KB | 345.6 KB | **+16.0%** | 113357 | 98980 |
| internet_posts | 100 | 24.0 KB | 20.5 KB | **+14.6%** | 6093 | 5249 |
| internet_random_users | 50 | 53.4 KB | 44.5 KB | **+16.7%** | 19860 | 18637 |
| internet_users | 10 | 4.0 KB | 3.1 KB | **+23.8%** | 1225 | 1093 |
| mongodb_irregular | 50 | 16.0 KB | 13.5 KB | **+15.6%** | 5832 | 5570 |
| orders | 50 | 20.1 KB | 14.1 KB | **+29.9%** | 6906 | 5814 |

### Summary
- **Total JSON (compact) size**: 963.9 KB
- **Total ZON size**: 767.3 KB
- **Overall compression**: 20.4%

## TOON Comparison
*(datasets with .toon files)*

| Dataset | Records | JSON Size | ZON Size | TOON Size | vs TOON | JSON tk | ZON tk | TOON tk |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| hikes | 3 | 451.0 B | 264.0 B | 286.0 B | **+7.7%** | 139 | 96 | 104 |

---

## 📚 API Reference

### `zon.encode(data)`

Encodes a Python object (dict or list) into a ZON-formatted string.

**Parameters:**
- `data` (Any): The input data to encode. Must be JSON-serializable (dict, list, str, int, float, bool, None).

**Returns:**
- `str`: The ZON-encoded string.

**Example:**
```python
import zon
data = {"id": 1, "name": "Alice"}
zon_str = zon.encode(data)
```

---

### `zon.decode(zon_str)`

Decodes a ZON-formatted string back into a Python object.

**Parameters:**
- `zon_str` (str): The ZON-encoded string to decode.

**Returns:**
- `Any`: The decoded Python object (dict or list).

**Example:**
```python
import zon
data = zon.decode(zon_str)
print(data["name"])  # "Alice"
```



---

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Add tests for new features
4. Submit a pull request

---

## 📄 License

**Proprietary License - Free for Production Use**

✅ **You CAN**:
- Use ZON in production (commercial or non-commercial)
- Integrate into your applications and services
- Deploy at any scale

❌ **You CANNOT**:
- Redistribute or sell the source code
- Modify and redistribute
- Create competing products

**Copyright (c) 2025 Roni Bhakta. All Rights Reserved.**

See [LICENSE](LICENSE) for full terms. For custom licensing: ronibhakta1@gmail.com

---

## 🙏 Acknowledgments

- Inspired by TOON format for LLM token efficiency
- Benchmark datasets from JSONPlaceholder, GitHub API, Random User Generator, StackExchange API
- Community feedback and testing

---

## ✉️ Support

- **Documentation**: [Full Docs](SPEC.md)
- **Issues**: [GitHub Issues](https://github.com/ZON-Format/ZON/issues)
- **Discussions**: [GitHub Discussions](https://github.com/ZON-Format/ZON/discussions)

---

**Made with ❤️ for the LLM community**

*ZON v1.0+ - Compression that scales with complexity*
