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
- **Product_ID** is unique, while 'Product_Name' can appear across multiple records.
-  **Supplier_ID** is unique, though some **Supplier_Name** values link to multiple supplier IDs.
-  **Warehouse_Location** is unique per record.


## Data Preprocessing
- ### Transforming `Unit_Price` into Decimal Data Type
`Unit_Price` was imported as VARCHAR to preserve original written decimal format. Hence, the `Unit_Price` column were converted to DECIMAL for numerical analysis.
```sql
ALTER TABLE inventory 
ALTER COLUMN Unit_Price DECIMAL(10, 2);
```
- ### Checking and Handling Missing Value
This query below was run to check the missing value across all columns
```sql
SELECT * FROM inventory
WHERE Product_ID IS NULL
      OR Product_Name IS NULL
      OR Category IS NULL
      OR Supplier_ID IS NULL
      OR Supplier_Name IS NULL
      OR Stock_Quantity IS NULL
      OR Reorder_Level IS NULL
      OR Reorder_Quantity IS NULL
      OR Unit_Price IS NULL
      OR Date_Received IS NULL
      OR Last_Order_Date IS NULL
      OR Expiration_Date IS NULL
      OR Warehouse_Location IS NULL
      OR Sales_Volume IS NULL
      OR Inventory_Turnover_Rate IS NULL
      OR Status IS NULL;
```
Result: A record with a NULL value in the Category column was identified.

| Product_ID | Product_Name | Category | Supplier_ID | Supplier_Name| Stock_Quantity | Reorder_Level | Reorder_Quantity | Unit_Price | Date_Received | Last_Order_Date | Expiration_Date | Warehouse_Location | Sales_Volume | Inventory_Turnover_Rate | Status |
| ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- |
| 10-378-9729 | Cabbage | NULL | 83-941-9620 | Rooxo | 69 | 21 | 68 | 66.55 | 2024-12-23 | 2024-11-26 | 2024-09-21 | 2 Butterfield Pass | 36 | 35 | Discontinued |

- The missing category were imputed based on the category of product with the same name
```sql
SELECT Product_ID, Product_Name, Category FROM inventory WHERE Product_Name = 'Cabbage';
```
Result:
| Product_ID | Product_Name | Category |
| ------- | ------- | ------- |
| 10-378-9729 | Cabbage | NULL |
| 21-013-3508 | Cabbage | NULL |
| 31-745-6850 | Cabbage | NULL |
| 45-380-4627 | Cabbage | NULL |
| 67-025-1245 | Cabbage | NULL |
| 75-927-9108 | Cabbage | NULL |
| 79-428-8753 | Cabbage | NULL |
| 82-538-4809 | Cabbage | NULL |

Based on this match, the missing Category was imputed as 'Fruits & Vegetables' as below:
```sql
UPDATE inventory
SET Category = 'Fruits & Vegetables'
WHERE Product_ID = '10-378-9729';
```
  
  
