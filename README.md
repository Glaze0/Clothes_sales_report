# Retail Business — Exploratory Data Analysis (EDA)

An end-to-end exploratory data analysis project using four related retail datasets: **customers, orders, products, and sales**. The project uses Python, Pandas, Matplotlib, and Seaborn to examine customer demographics, order and delivery patterns, product catalogue and inventory, and sales performance.

The repository includes the analysis notebook and a presentation summarising the main findings and visualisations.

---

## Project deliverables

| File | Description |
|---|---|
| `report.ipynb` | Main Jupyter Notebook containing data inspection, cleaning checks, feature engineering, joins, aggregations, and visual analysis. |
| `retail_eda_business_report.pptx` | PowerPoint summary with 15 slides and approximately 12 charts. |
| `customers.csv` | Customer demographic and location data. |
| `orders.csv` | Order, payment, and delivery-date data. |
| `products.csv` | Product catalogue and recorded inventory data. |
| `sales.csv` | Sales transaction-line data. |

## Business objectives

The analysis is designed to answer questions such as:

- What does the customer base look like by age, gender, and geography?
- How are orders and recorded payment amounts distributed over time?
- How long do deliveries typically take?
- How is the product catalogue structured by type, size, colour, and price?
- Which product types, styles, sizes, and colours account for the most recorded units and sales-line value?
- How do customer and product attributes relate to transaction-level sales?
- Which data-quality issues should be resolved before using the findings for operational decisions?

## Dataset overview

The supplied CSV files contain the following records and fields:

| Dataset | Rows | Columns | Key fields |
|---|---:|---:|---|
| Customers | 1,000 | 9 | `customer_id`, `age`, `gender`, `city`, `state`, `country` |
| Orders | 1,000 | 5 | `order_id`, `customer_id`, `payment`, `order_date`, `delivery_date` |
| Products | 1,260 | 8 | `product_ID`, `product_type`, `product_name`, `size`, `colour`, `price`, `quantity` |
| Sales | 5,000 | 6 | `sales_id`, `order_id`, `product_id`, `price_per_unit`, `quantity`, `total_price` |

### Table relationships

The tables connect through these keys:

```text
customers.customer_id  1 ──── many  orders.customer_id
orders.order_id        1 ──── many  sales.order_id
products.product_ID    1 ──── many  sales.product_id
```

The analysis follows the path **customer → order → sales line**, while each sales line also links to a product variant. In the notebook, the product key is standardised from `product_ID` to `product_id` before merging.

## Analysis workflow

The notebook is organised around the following stages:

1. **Load and inspect the data** — read the four CSV files and review dimensions, column names, data types, descriptive statistics, and category frequencies.
2. **Check data quality** — inspect nulls, duplicate rows, primary-key uniqueness, and relationships between foreign keys.
3. **Prepare fields** — convert order and delivery dates to datetime and derive fields such as delivery duration, order month, and order year.
4. **Univariate analysis** — examine individual customer, order, product, and sales variables.
5. **Group and aggregate** — calculate customer counts, order metrics, inventory totals, units sold, and sales-line value by relevant categories.
6. **Merge related tables** — connect sales to orders, customers, and products to support cross-table analysis.
7. **Advanced EDA** — compare sales across age groups, states, product types, product names, sizes, colours, and months.
8. **Communicate results** — summarise patterns, data limitations, and follow-up actions in the PowerPoint report.

## Visual analysis

The notebook and presentation include charts covering these topics (approximately 12 charts in the presentation):

- Customer age distribution
- Customer distribution by gender
- Customer distribution by state / territory
- Order payment distribution and spread
- Delivery duration distribution and outliers
- Product catalogue price distribution
- Product-type inventory and price comparisons
- Sales value by product type
- Top product styles by sales-line value and units sold
- Monthly order volume and sales-line value trends
- Units sold by product type and size (heatmap)
- Customer sales value by age group and geography

## Selected findings from the notebook

These are descriptive findings from the supplied dataset; they are not causal explanations.

- The customer table contains **1,000 customers**. All records list **Australia** as the country, and the data covers eight states and territories.
- Customer age ranges from **20 to 80**; the mean age is approximately **49.86 years** and the median is **50 years**.
- **South Australia** has the highest customer count (**139**) and **Tasmania** the lowest (**104**).
- The order-date range is **1 January 2021 to 24 October 2021**. March has the most orders in the raw orders table (**118**). October is an incomplete month in this dataset.
- Average delivery duration is approximately **14.08 days**, with a median of **14 days** and a range of **1–27 days**.
- The product catalogue contains **1,260 product variants**, evenly divided across Shirt, Jacket, and Trousers (**420 rows each**).
- Recorded catalogue inventory totals **75,789 units**. Catalogue prices range from **90 to 119 dataset units**; the currency is not specified in the files.
- The sales table contains **5,000 sales lines** and **9,962 units**. The sum of `total_price` is **1,031,800 dataset units**.
- Jackets have the highest sales-line value by product type in the notebook (**357,026**), followed by Trousers (**341,174**) and Shirts (**333,600**).
- South Australia has the highest recorded sales-line value by state (**147,816**) in the notebook's merged transaction analysis.

## Data-quality notes and interpretation cautions

Several checks are important before treating the metrics as finance or inventory reporting:

- The four raw files have no null values detected by `isnull()` and no fully duplicated rows in the supplied extracts. This does not guarantee that all business rules or relationships are valid.
- **383 customer IDs** in the customer table do not appear in the orders table. These customers may simply have no recorded orders in this extract.
- **7 orders** have no matching sales lines.
- **27 product variants** in the catalogue are not represented in sales transactions.
- **7 order–product combinations** occur more than once. These are repeated combinations, not necessarily duplicate sales rows; verify the business meaning before removing anything.
- All **5,000** sales-line `price_per_unit` values differ from the matching catalogue `price` values. This may reflect discounts, price changes, or a data-mapping issue; the supplied files do not establish which explanation is correct.
- The order-level `payment` sum (**33,972,936**) does not reconcile with the sales-line `total_price` sum (**1,031,800**). The definitions, units, and calculation logic need to be confirmed before comparing these measures.
- Do not sum product `quantity` after joining products to sales lines: catalogue stock repeats for every matching sales row and will be overstated. Aggregate inventory from the products table at the product-variant level. The notebook's grouped `recorded_inventory` after the sales merge is therefore not a valid total-stock measure.
- A current quantity field without a stock snapshot date or stock movement history is insufficient to calculate a reliable historical sell-through rate.
- Product type, product name, and variant dimensions should be considered together. For example, a product name can appear under more than one product type.
- No cost, marketing spend, returns, or currency field is supplied. Profit, margin, acquisition cost, and currency-specific conclusions cannot be calculated from these files alone.

## Tools and libraries

- Python
- Jupyter Notebook
- Pandas
- NumPy
- Matplotlib
- Seaborn

## Run the project locally

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd <your-repository-folder>
```

### 2. Create and activate a virtual environment (recommended)

```bash
python -m venv .venv
```

**Windows PowerShell:**

```powershell
.venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

### 4. Start Jupyter

```bash
jupyter notebook
```

Open `report.ipynb` and run the cells from top to bottom. Keep the four CSV files in the same directory as the notebook, or update the file paths in the data-loading cell.

## Suggested repository structure

```text
retail-eda-project/
│
├── README.md
├── report.ipynb
├── customers.csv
├── orders.csv
├── products.csv
├── sales.csv
├── montage.png
└── retail_eda_business_report.pptx
```

## Possible next steps

- Resolve the payment-versus-sales reconciliation issue with a documented business definition.
- Investigate the selling-price versus catalogue-price differences.
- Add explicit data validation for primary keys, foreign keys, valid dates, and positive quantities/prices.
- Analyse product variants using `product_id` (rather than product name alone) when comparing stock and sales.
- Add a dated inventory snapshot or stock movement data to support stock-age and sell-through analysis.
- Extend the project with a reproducible dashboard or a SQL-based analysis layer.

## Notes

- Monetary values are reported as **dataset units** because no currency is specified.
- Findings describe the supplied 2021 extract and should not be interpreted as current retail performance.
- Before making the CSV files public on GitHub, confirm that customer names and addresses are synthetic or otherwise approved for public release. Do not publish personal or confidential customer information.

---

*Exploratory analysis and presentation based on the supplied `report.ipynb` and its four CSV input files.*
