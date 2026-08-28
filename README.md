# inventory-project-SQL

## Data Understanding
This project uses [Grocery Inventory and Sales Dataset](https://www.kaggle.com/datasets/salahuddinahmedshuvo/grocery-inventory-and-sales-dataset) from Kaggle.
The dataset contains **990 records**, where each row represents a Stock Keeping Unit (SKU) covering product details, supplier info, inventory levels, sales perfomance, and lifetime cycle. The columns are as follows:
- Product_ID
- Product_Name
- Category
- Supplier_ID
- Supplier_Name
- Stock_Quantity
- Reorder_Level
- Reorder_Quantity
- Unit_Price
- Date_Received
- Expiration_Date
- Warehouse_Location
- Sales_Volume
- Inventory_Turnover_Rate
- Status

## Key Data Structure Observations
- **Product_ID** is unique accros all 990 records, while 'Product_Name' can appear across multiple records.
-  **Supplier_ID** is unique per record, though some **Supplier_Name** values link to multiple supplier IDs.
-  **Warehouse_Location** is unique per record.


## Data Preprocessing
During
