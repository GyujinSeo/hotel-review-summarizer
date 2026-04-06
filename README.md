# Yanolja Review Summarization Service

An AI-powered service that crawls hotel reviews from Yanolja and automatically summarizes positive and negative feedback using OpenAI's GPT model.

---

## Features

- **Review Crawling**: Collects reviews from Yanolja hotel pages using Selenium.
- **Review Preprocessing**: Filters reviews from the past 6 months and excludes short reviews (under 30 characters).
- **AI Summarization**: Summarizes positive and negative reviews separately using OpenAI `gpt-3.5-turbo` with Few-shot prompting.
- **Gradio Demo**: A web UI where you can select a hotel and instantly view summarized results.

---

## Project Structure

```
yanolja-summarization/
├── crawler.py          # Yanolja review crawler
├── demo.py             # Gradio demo app
├── sample_demo.py      # Basic Gradio example
├── model.ipynb         # Model experimentation notebook
└── res/
    ├── reviews.json            # Insadong hotel review data
    ├── ninetree_pangyo.json    # Ninetree Pangyo review data
    ├── ninetree_yongsan.json   # Ninetree Yongsan review data
    ├── prompt_1shot.pickle     # 1-shot prompt
    └── prompt_2shot.pickle     # 2-shot prompt
```

---

## Installation & Setup

### Requirements

- Python 3.8+
- Chrome browser and ChromeDriver

### Install Packages

```bash
pip install selenium beautifulsoup4 openai gradio python-dateutil
```

### Set Environment Variable

```bash
export OPENAI_API_KEY="your-api-key-here"
```

---

## Usage

### 1. Crawl Reviews

```bash
python crawler.py <output_filename> <yanolja_hotel_url>
```

**Example**

```bash
python crawler.py myhotel https://www.yanolja.com/hotels/...
```

Crawled reviews will be saved to `./res/<output_filename>.json`.

### 2. Run the Demo

```bash
python demo.py
```

Then open your browser at `http://localhost:7860`:

1. Select a hotel (Insadong / Pangyo / Yongsan)
2. Click **Submit**
3. View the **High Rating Summary** and **Low Rating Summary**

---

## Data Format

Crawled review JSON files follow this structure:

```json
[
    {
        "review": "Staff were very kind and I had a great stay.",
        "stars": 5,
        "date": "6 days ago"
    },
    ...
]
```

| Field | Description |
|-------|-------------|
| `review` | Review text |
| `stars` | Star rating (1–5) |
| `date` | Date of review |

---

## Pipeline Overview

```
Yanolja Hotel URL
      ↓
  crawler.py (Selenium + BeautifulSoup)
      ↓
  Save reviews as JSON
      ↓
  demo.py preprocessing
  (last 6 months / 30+ chars / split by rating)
      ↓
  OpenAI GPT (Few-shot prompting)
      ↓
  Positive Summary / Negative Summary (Gradio UI)
```

---

## Notes

- The crawler requires Chrome and ChromeDriver installed on your system.
- CSS selectors may need to be updated if Yanolja changes their website structure.
- `prompt_1shot.pickle` and `prompt_2shot.pickle` contain Few-shot prompt examples and can be replaced as needed.
