# Vendor Performance Dashboard

A vendor and product performance project that combines a Python exploratory analysis with a Power BI dashboard. It brings purchase, sales, price, and freight data together to review revenue, purchasing, margins, stock movement, and estimated unsold inventory.

## Project views

- **Python analysis** loads the source CSV files into SQLite, builds a vendor and brand summary, calculates performance measures, explores business questions, and exports analysis tables.
- **Power BI dashboard** presents interactive filters and a visual summary of vendor, brand, purchasing, sales, profitability, and inventory measures.

## Data inputs

The Python workflow expects a `data/` folder beside the notebook or script, containing these CSV files:

data/
├── begin_inventory.csv
├── end_inventory.csv
├── purchases.csv
├── purchase_prices.csv
├── sales.csv
└── vendor_invoice.csv

Each CSV is loaded as a SQLite table named from its filename (without `.csv`). The database file is `inventory.db`. The input reader uses `latin1` encoding. Keep the source column names and join keys consistent with the supplied data because the SQL analysis joins purchase prices by brand, sales by vendor and brand, and freight by vendor.

## Python workflow

The analysis follows these main steps:

1. Load the six CSV files into SQLite, replacing same-named tables when rerun.
2. Aggregate purchases, sales, purchase prices, and freight into a `vendor_sales_summary` table.
3. Clean and derive measures such as gross profit, profit margin, stock turnover, and the sales-to-purchase ratio.
4. Explore brand and vendor performance, purchase concentration, bulk-purchase pricing, low-turnover vendors, unsold inventory value, and differences in profit margins between higher- and lower-sales groups.
5. Export the resulting analysis tables as CSV files.

The analysis includes scatter plots, bar charts, a Pareto chart, a purchase-contribution donut chart, a bulk-purchase price comparison, and profit-margin confidence intervals. It also applies a two-sample Welch t-test to compare profit margins across vendor sales groups.

## Power BI dashboard

The dashboard shown in the supplied report includes slicers for **Vendor Name**, **Brand**, and **Description**, plus a control to clear the slicers. Its summary cards show total sales, total purchases, gross profit, and profit margin. Visuals include:

- A vendor inventory health table with purchase and sales quantities, average stock turnover, and unsold capital.
- Purchase contribution by vendor.
- Top vendors by sales.
- Top brands by sales.
- Average stock turnover by vendor.

## Measures

The Python analysis uses the following definitions:

- **Gross profit** = total sales dollars − total purchase dollars.
- **Profit margin** = gross profit ÷ total sales dollars × 100, where sales are positive.
- **Stock turnover** = total sales quantity ÷ total purchase quantity, with zero used when purchase quantity is not positive.
- **Sales-to-purchase ratio** = total sales dollars ÷ total purchase dollars, with zero used when purchase dollars are not positive.
- **Unsold inventory value** = (total purchase quantity − total sales quantity) × purchase price.
- **Purchase contribution** = a vendor's purchase dollars as a percentage of total purchase dollars.

Unsold inventory value is an estimate derived from the purchase and sales aggregates. It is not a stock count reconciliation. Interpret turnover and margin results in the context of the source data and aggregation rules.

## Outputs

The Python workflow writes these CSV files to the current working directory:

- `vendor_sales_summary.csv`
- `BrandPerformance.csv`
- `LowTurnoverVendor.csv`
- `PurchaseContribution.csv`

The supplied analysis export reported 6,386 rows in the vendor sales summary, 5,678 brand-performance rows, 1,702 low-turnover-vendor rows, and 113 purchase-contribution rows. These are reference counts from that run and can change with the input data.

## Running the Python analysis

Install the packages used by the analysis:
python -m pip install pandas sqlalchemy numpy matplotlib seaborn scipy jupyter

Place the six CSV files in `data/`, then run the project notebook from the directory that contains `data/`. Run the notebook cells in order so the SQLite tables and `vendor_sales_summary` are available before the analysis and export sections.

The supplied reference exports do not identify the notebook or Power BI report filenames or the Power BI refresh configuration. Use the matching source files and connection settings in the project to rerun the analysis or refresh the dashboard.

## Notes

- The Python workflow overwrites same-named SQLite tables and CSV outputs when rerun.
- Dashboard values and analysis findings depend on the input data and may differ from the supplied PDF exports.
- The documented results describe the behavior visible in the supplied analysis and dashboard exports.
