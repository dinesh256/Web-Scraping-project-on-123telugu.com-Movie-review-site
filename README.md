# 🎬 Telugu Movie & Web Series Review Scraper — 123telugu.com


## 📌 Project Overview

This project scrapes **400+ movie and web series reviews** from [123telugu.com](https://www.123telugu.com/category/reviews/) — one of the most popular Telugu entertainment review portals. The scraped data is cleaned, structured, and analyzed to uncover insights about ratings, content types, and release trends across Theatrical and OTT platforms.

The project covers the **full data pipeline**:
> **Web Scraping → Data Cleaning → Exploratory Data Analysis → Visualization**

---

## 🗂️ Dataset

| Field | Description |
|---|---|
| `Title` | Movie or Web Series name |
| `Content_Type` | Movie / Web Series |
| `Review_Type` | Theatrical / OTT |
| `Release_Date` | Release or Streaming date |
| `Rating` | Numeric rating (out of 5) |
| `Rating_Out_Of_5` | Rating string (e.g., 3.5/5) |
| `Starring` | Lead cast members |
| `Director` | Director name |
| `Producer` | Producer name |
| `Music_Director` | Music director name |
| `Cinematographer` | Cinematographer name |
| `Editor` | Editor name |
| `Review_URL` | Source review link |

- **Pages scraped:** 1–20 (123telugu.com/category/reviews/)
- **Total records:** 400+ reviews (after deduplication)

---

## ⚙️ How It Works

### 1. 🔗 Link Collection — `get_links(url)`
- Fetches all review page links from each paginated category page
- Filters out gallery links, comic/funky review slides, and duplicates
- Iterates across pages 1–20 to collect 400+ unique review URLs

### 2. 🕷️ Review Scraping — `scrape_review(url)`
- Visits each review URL and parses the HTML using **BeautifulSoup**
- Extracts fields by targeting `<strong>` tags and their siblings using regex
- Detects content type (Movie vs Web Series) from HTML structure
- Detects review type (Theatrical vs OTT) from `<h1>` headings and streaming platform fields
- Handles exceptions gracefully — returns `NaN` for any missing fields
- Includes a `0.3s` delay between requests to avoid overloading the server

### 3. 🧹 Data Cleaning
- Filled missing cast/crew fields with `'Unknown'`
- Filled missing ratings with the **median rating**
- Standardized `Rating_Out_Of_5` format (e.g., `0/5` for unrated)
- Removed duplicate entries using `Review_URL` as the unique key
- Reset index after concatenating all 3 page batches

### 4. 📊 Data Analysis & Visualizations

| Analysis                       | Description                                            |
|:-------------------------------|:-------------------------------------------------------|
| Content Type Distribution      | Movie vs Web Series count                              |
| Review Type Distribution       | Theatrical vs OTT count                                |
| Rating Distribution            | Histogram of all ratings (out of 5)                    |
| Average Rating by Content Type | Bar chart comparing Movies vs Web Series               |
| Average Rating by Review Type  | Bar chart comparing Theatrical vs OTT                  |
| Rating Spread                  | Boxplot showing rating variance across review types     |
| Highest Rated Movie            | Top-rated movie in the dataset                         |
| Highest Rated Web Series       | Top-rated OTT/Web Series                               |
| Low Rated Content              | Titles with rating < 2/5                               |
| GroupBy Stats                  | Min, Mean, Max rating by Content Type and Review Type  |

---

## 📈 Sample Visualizations

- `Distribution of Ratings` — Histogram (Rating out of 5)
- `Movie vs Web Series Count` — Bar chart
- `Theatrical vs OTT Count` — Bar chart
- `Average Rating: Movie vs Web Series` — Bar chart
- `Average Rating: Theatrical vs OTT` — Bar chart
- `Rating Spread: Theatrical vs OTT` — Boxplot

---

## 💡 Key Insights

- Majority of reviews scraped are **Movies**, with Web Series being a growing segment
- **OTT titles** show a slightly different rating distribution compared to Theatrical releases
- Most titles fall in the **2.5 – 3.5 / 5** rating range
- Several titles rated **below 2/5** highlight critically panned content
- `groupby` analysis reveals differences in average ratings across content and release type

---

## 🛠️ Tech Stack

| Library | Purpose |
|---|---|
| `requests` | HTTP requests to fetch web pages |
| `BeautifulSoup4` | HTML parsing and data extraction |
| `pandas` | Data structuring, cleaning, and analysis |
| `numpy` | Handling missing values (NaN) |
| `re` | Regex-based field extraction and text cleaning |
| `time` | Polite crawl delays between requests |
| `matplotlib` | Plotting charts and visualizations |
| `seaborn` | Statistical visualizations (histogram, boxplot) |

---

## 🚀 How to Run

### 1. Clone the repository
```bash
git clone https://github.com/dineshkumar2512/movie-web-scraping-python.git
cd movie-web-scraping-python
```

### 2. Install dependencies
```bash
pip install requests beautifulsoup4 pandas numpy matplotlib seaborn
```

### 3. Run the notebook
```bash
jupyter notebook webscrapingfinalsub.ipynb
```

> ⚠️ **Note:** Running all 20 pages of scraping takes several minutes due to polite request delays. The notebook is structured in batches (Page 1, Page 2, Pages 3–20) for easy partial runs.

---

## 📁 Project Structure

```
movie-web-scraping-python/
│
├── webscrapingfinalsub.ipynb     # Main Jupyter Notebook (scraping + analysis)
├── data/
│   └── telugu_reviews.csv        # Exported cleaned dataset (optional)
├── screenshots/
│   ├── rating_distribution.png
│   ├── content_type_count.png
│   ├── avg_rating_comparison.png
│   └── rating_boxplot.png
└── README.md
```
