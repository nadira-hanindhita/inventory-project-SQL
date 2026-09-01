# inventory-project-SQL

## Data Understanding
This project uses [Grocery Inventory and Sales Dataset](https://www.kaggle.com/datasets/salahuddinahmedshuvo/grocery-inventory-and-sales-dataset) from Kaggle.
The dataset contains **990 records**, where each row represents a Stock Keeping Unit (SKU) covering product details, supplier information, inventory levels, sales perfomance, and lifetime cycle. The columns are as follows:
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

- The missing category was imputed based on the category of product with the same name
```sql
SELECT Product_ID, Product_Name, Category FROM inventory WHERE Product_Name = 'Cabbage';
```
Result:
| Product_ID | Product_Name | Category |
| ------- | ------- | ------- |
| 10-378-9729 | Cabbage | NULL |
| 21-013-3508 | Cabbage | Fruits & Vegetables |
| 31-745-6850 | Cabbage | Fruits & Vegetables |
| 45-380-4627 | Cabbage | Fruits & Vegetables |
| 67-025-1245 | Cabbage | Fruits & Vegetables |
| 75-927-9108 | Cabbage | Fruits & Vegetables |
| 79-428-8753 | Cabbage | Fruits & Vegetables |
| 82-538-4809 | Cabbage | Fruits & Vegetables |

Based on this match, the missing Category was imputed as 'Fruits & Vegetables' as below:
```sql
UPDATE inventory
SET Category = 'Fruits & Vegetables'
WHERE Product_ID = '10-378-9729';
```
  
## SQL Analysis & Business Insights
1. What is the total stock volume and total monetary valuation of our current inventory?
```sql
SELECT SUM(stock_quantity) AS total_stock, SUM(stock_quantity * unit_price) AS total_inventory_value
FROM inventory;
```
Result:
| | total_stock | total_inventory_value |
| ------- | ------- | ------- |
| 1 | 55053 | 332654.71 |

2. What product categories does the inventory contain, and how many unique items are in each?
```sql
SELECT Category, Count(DISTINCT Product_Name) AS Product_Count
FROM inventory
GROUP BY Category
ORDER BY Product_Count DESC;
```
Result : 
| | Category | Product_Count |
| ------- | ------- | ------- |
| 1 | Fruits & Vegetables | 41 |
| 2 | Dairy | 24 |
| 3 | Grains & Pulses | 20 |
| 4 | Oils & Fats	| 10 |
| 5 | Seafood | 10 |
| 6 | Bakery | 10 |
| 7 | Beverages | 8 |

3. What are the top 5 high-demand active products that have reached or dropped below their reorder threshold and require immediate purchase orders?
```sql
SELECT TOP 5 * FROM inventory
WHERE stock_quantity <= reorder_level AND status = 'Active'
ORDER BY Sales_Volume DESC
```
Result:
| Product_ID | Product_Name | Category | Supplier_ID | Supplier_Name| Stock_Quantity | Reorder_Level | Reorder_Quantity | Unit_Price | Date_Received | Last_Order_Date | Expiration_Date | Warehouse_Location | Sales_Volume | Inventory_Turnover_Rate | Status |
| ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- | ------- |
| 99-561-4871|Haddock|	Seafood|	04-786-5408|	Buzzdog|	17|	38|	93|	9.00|	2025-02-14|	2024-08-20|	2024-07-28|	952 Rowland Junction	100	63	Active
02-575-1980	Plum	Fruits & Vegetables	61-100-8296	Yambee	11	58	32	4.00	2024-02-28	2024-07-09	2024-11-24	34832 Autumn Leaf Terrace	99	94	Active
30-996-2526	Green Beans	Fruits & Vegetables	30-942-0054	Izio	86	90	39	2.10	2024-10-20	2024-02-28	2024-09-07	77 Monterey Avenue	98	85	Active
98-235-2711	Asparagus	Fruits & Vegetables	23-052-4744	Devpoint	22	40	7	5.00	2024-09-12	2024-12-31	2025-01-26	5 Transport Pass	98	60	Active
74-943-9034	White Bread	Bakery	15-739-5480	Tagfeed	30	91	34	2.50	2024-07-11	2024-11-18	2024-05-06	0 Saint Paul Center	97	88	Active
