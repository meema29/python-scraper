# Pharmaly Product Scraper

A collaborative Python web-scraping project that collects and prepares product data from [Pharmaly](https://www.pharmaly-dz.com/), an Algerian online pharmacy and parapharmacy website.

The project follows a practical workflow: **website inspection → scraping strategy → product collection → data cleaning**. The work is documented in three Jupyter notebooks.

## Project goals

- Understand how Pharmaly structures its homepage, category listings, product cards, and product detail pages.
- Identify how the website loads products dynamically.
- Collect product URLs across multiple categories and pagination pages.
- Extract structured product information from individual product pages.
- Clean and validate the resulting dataset so it can be used for further analysis.

## Notebooks

The notebooks are in the [`notebooks/` directory](https://github.com/meema29/python-scraper/tree/main/notebooks).

### 1. `website_analysis.ipynb` Website inspection

This notebook investigates Pharmaly before building the scraper.

It covers:

- Sending HTTP requests with `requests` and parsing HTML with BeautifulSoup using the `lxml` parser.
- Inspecting page titles, links, images, HTML elements, and the homepage's category links.
- Extracting category URLs, including cases where a category name is stored in a parent element rather than directly inside its link.
- Inspecting product links and identifying the shared product-card container, so multiple links belonging to one product are not counted as separate products.
- Examining product-card fields and the different HTML structures used for regular and discounted prices.
- Investigating product images and checking the product detail page for fields that are not present in category cards.
- Identifying JetEngine's AJAX-based infinite-scroll mechanism and inspecting the request parameters and returned HTML.
- Comparing category pages and checking whether the same product selectors and AJAX mechanism can be reused.
- Inspecting purchase-action links, including standard add-to-cart and pre-order buttons.

**Main finding:** a simple `requests.get()` retrieves the initial HTML but does not load the additional products fetched by the website's JavaScript. The notebook identifies the underlying WordPress/JetEngine AJAX request needed to retrieve those additional listing pages.

### 2. `scraper.ipynb` Product collection

This notebook turns the website investigation into a collection workflow.

It covers:

- Configuring the base website URL, request headers, a reusable Requests session, and the product categories.
- Extracting product URLs from category listing HTML with BeautifulSoup and CSS selectors.
- Investigating the category listing's embedded navigation configuration.
- Reconstructing the JetEngine `listing_load_more` AJAX payload, sending POST requests to the WordPress AJAX endpoint, and parsing the HTML returned inside the JSON response.
- Iterating through listing pages and categories to collect products that are not present in the initial HTML.
- Removing duplicate product URLs while preserving the categories associated with each product.
- Tracking failed category or pagination requests and displaying collection summaries.
- Defining product-detail extraction for product name, brand, category, original and current prices, descriptions, image URL, and product URL.
- Using Pandas to organize collected information into tabular data and export CSV files.

The notebook includes both exploratory tests and the consolidated collection logic. The AJAX approach is used because the site's product listings rely on dynamic loading rather than ordinary static pagination.

### 3. `cleaning.ipynb` Data cleaning and validation

This notebook prepares the collected product dataset for downstream use.

It covers:

- Loading the product CSV and inspecting its dimensions, columns, data types, and sample records.
- Checking duplicate rows and duplicate product URLs.
- Normalizing text fields by collapsing repeated whitespace and trimming leading/trailing spaces.
- Converting Algerian price strings (for example, `DA 5.750,00`) into numeric values.
- Investigating zero or invalid current prices and treating zero current prices as missing.
- Measuring missing values and their percentages across columns.
- Identifying full descriptions that merely repeat the product name and treating them as missing descriptions.
- Checking product and image URL formats and exploring category values.
- Applying explicit missing-value decisions: missing brands are labelled `Unknown`, missing descriptions are represented by empty strings in the working dataset, and missing prices are not artificially imputed.
- Removing records without a valid current price and exporting the cleaned dataset.

The notebook also records the cleaning decisions so that the transformations are transparent and reproducible.

## Scraped product fields

The product-detail extraction is designed to collect the following fields:

| Field | Description |
|---|---|
| `product_name` | Product name |
| `brand` | Product brand, when available |
| `categories` | Category or categories associated with the product |
| `original_price` | Previous price, when a discount is displayed |
| `current_price` | Current displayed selling price |
| `short_description` | Short product description |
| `full_description` | Detailed product description |
| `image_url` | Main product image URL |
| `product_url` | Product page URL |

For products without a discount, the original price is left empty; the current price is still extracted from the product page.

## Scraping method and tools

| Tool | Role in the project |
|---|---|
| Python | Main programming language |
| Requests | Sends HTTP GET and POST requests |
| BeautifulSoup | Parses HTML and extracts elements using CSS selectors |
| lxml | HTML parsing backend |
| Pandas | Structures, cleans, validates, and exports the data |
| tqdm | Displays progress during collection |
| Jupyter Notebook | Exploration, implementation, and documentation |

**Why Requests + BeautifulSoup?** The initial HTML and AJAX responses contain the product markup we need. Requests handles the HTTP communication, while BeautifulSoup parses the returned HTML. This approach lets us inspect and reuse the site's AJAX mechanism without relying on browser automation for the collection workflow.

## Repository structure

```text
python-scraper/
├── data/
│   └── processed/
│       └── pharmaly_products_800.csv
├── notebooks/
│   ├── website_analysis.ipynb
│   ├── scraper.ipynb
│   └── cleaning.ipynb
├── .gitignore
├── requirements.txt
└── README.md
```

The repository currently includes the 800-product CSV under `data/processed/`. The notebooks also contain code for generating intermediate URL collections and the cleaned output; generated files may be saved to the notebook's working directory depending on where the notebook is run.

## Getting started

### Prerequisites

- Python 3.10 or later
- Git
- Jupyter Notebook or JupyterLab

### 1. Clone the repository

```bash
git clone https://github.com/meema29/python-scraper.git
cd python-scraper
```

### 2. Create and activate a virtual environment

**Windows PowerShell**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux**

```bash
python -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

Install the packages listed in `requirements.txt`:

```bash
pip install -r requirements.txt
```

### 4. Launch Jupyter

```bash
jupyter notebook
```

Open the `notebooks/` folder. For the full project workflow, read and run the notebooks in this order:

1. `website_analysis.ipynb`
2. `scraper.ipynb`
3. `cleaning.ipynb`

**Note:** Some notebook cells currently use local file paths. If you run the notebooks on another computer, update those paths to match your local repository and data locations.

## Data quality notes

- A product may appear in more than one category. The collection logic keeps one product URL and associates it with its categories.
- Promotional and regular products have different price HTML structures; the original price is therefore optional.
- Missing values are handled according to the meaning of each field rather than filling every missing value indiscriminately.
- An image URL extracted from the website may itself be incorrect if the source page associates the wrong image with a product. Scraping can faithfully extract the page's markup without guaranteeing that the website's content is accurate.
- A displayed purchase button or pre-order link should not be treated as definitive proof of stock availability.

## Disclaimer

This project is developed for educational and academic purposes. Please respect the website's terms of use, applicable regulations, and reasonable request rates when collecting data.

## Contributors

Collaborative academic project developed using Python, web scraping, and data preparation techniques.

[View the GitHub repository](https://github.com/meema29/python-scraper)
