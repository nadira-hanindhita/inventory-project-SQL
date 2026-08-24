# inventory-project-SQL

## Data Understanding
This project uses [Grocery Inventory and Sales Dataset](https://www.kaggle.com/datasets/salahuddinahmedshuvo/grocery-inventory-and-sales-dataset) from Kaggle.
The dataset contains 990 records, with each record representing a Stock Keeping Unit (SKU), and consists of 16 columns covering product details, supplier information, inventory levels, sales performance, and product lifecycle information. The columns are described below:

| Column Name | Description |
| ------------- | ------------- |
| Product_ID  | Unique identifier for each product (SKU) |
| Product_Name  | Name of the product |
| Category | The category assigned to each product (e.g. Grains & Pulses, Beverages, and Fruits & Vegetables). |
| Supplier_ID | Unique identifier for each product supplier |
| Supplier_Name | Name of the supplier |
| Stock_Quantity | |
| Reorder_Level | |
| Reorder_Quantity | |
| Unit_Price | |
| Date_Received | |
| Last_Order_Date | Most recent date on which the product was ordered |
| Expiration_Date | |
| Warehouse_Location | |
| Sales_Volume | Total number of units sold |
| Inventory_Turnover_Rate | |
| Status | |

Initial exploration of the dataset shows that each 'Product_ID' is unique, while 'Product_Name' can appear across multiple records. Each product record is also associated with a unique 'Supplier_ID'; however, some 'Supplier_Name' values are linked to multiple supplier IDs. Additionally, each 'Warehouse_Location' occurs only once in the dataset.
