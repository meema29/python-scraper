# Python Web Scraper

A collaborative Python web scraping project focused on collecting, structuring, and preparing e-commerce product data from an Algerian online pharmacy and parapharmacy platform.

The project explores the website's structure, product listings, pagination mechanisms, and data extraction techniques to build a structured product dataset for further analysis.

## Project Objectives

The main objectives are to:

- Explore and understand the structure of the website.
- Identify product categories and individual product pages.
- Extract product information such as names, brands, prices, and descriptions.
- Investigate dynamically loaded product listings and AJAX requests.
- Collect product URLs across multiple pages and categories.
- Organize and clean the extracted data using Python and Pandas.

## Website Analysis

Before implementing the scraping workflow, the website structure was investigated to understand how product information is displayed and loaded.

The analysis included:

- Inspecting HTML elements and CSS selectors.
- Identifying product cards and product links.
- Understanding relative and absolute URLs.
- Investigating pagination and dynamically loaded content.
- Examining the website's AJAX mechanism and request parameters.

## Scraping Approach

The project uses a combination of HTTP requests and HTML parsing.

**1. Static HTML extraction**

- `Requests` is used to send HTTP requests.
- `BeautifulSoup` parses the returned HTML.
- CSS selectors identify product cards and relevant information.

**2. Dynamic content investigation**

The website uses AJAX requests to load additional product listings.

The investigation focuses on the WordPress AJAX endpoint and JetEngine's `listing_load_more` handler to retrieve products beyond the initially displayed listings.

**3. Product information extraction**

The scraper is designed to collect:

| Field | Description |
|---|---|
| `product_name` | Product name |
| `brand` | Product brand |
| `categories` | Associated product categories |
| `original_price` | Original price, when available |
| `current_price` | Current selling price |
| `short_description` | Short product description |
| `full_description` | Detailed product description |
| `image_url` | Main product image |
| `product_url` | Product page URL |

## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| Requests | HTTP requests |
| BeautifulSoup | HTML parsing and data extraction |
| Pandas | Data manipulation and structuring |
| lxml | HTML parsing backend |
| tqdm | Progress visualization |
| Jupyter Notebook | Development and experimentation |

## Repository Structure

```text
python-scraper/
│
├── notebooks/
│   ├── scraper.ipynb
│   └── cleaning.ipynb
│
├── .gitignore
├── requirements.txt
└── README.md
```

### Notebooks

- **scraper.ipynb:** Website exploration, product URL collection, AJAX pagination investigation, and product information extraction.
- **cleaning.ipynb:** Notebook reserved for data cleaning and preparation.

## Getting Started

### Prerequisites

- Python 3.10 or later
- Git
- Jupyter Notebook

### 1. Clone the repository

```bash
git clone https://github.com/meema29/python-scraper.git
cd python-scraper
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

### 3. Activate the environment

**Windows PowerShell:**

```powershell
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
source .venv/bin/activate
```

### 4. Install dependencies

```bash
pip install requests pandas beautifulsoup4 lxml tqdm jupyter
```

### 5. Launch Jupyter Notebook

```bash
jupyter notebook
```

Open the notebooks directory and start with `scraper.ipynb`.


## Disclaimer

This project is developed for educational and academic purposes. Data collection should respect the website's terms of use, applicable regulations, and reasonable request rates.

## Contributors

Collaborative academic project developed using Python and web scraping techniques.

**Repository:** https://github.com/meema29/python-scraper