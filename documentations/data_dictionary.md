# Data Dictionary

| Column | Description | Data Type |
|---|---|---|
| InvoiceNo | Unique invoice/transaction number. Values beginning with C indicate cancellations. | String |
| StockCode | Unique product/item code. | String |
| Description | Product name or description. | String |
| Quantity | Number of units in the transaction. | Integer |
| InvoiceDate | Date and time of the transaction. | Datetime |
| UnitPrice | Price per unit in GBP. | Float |
| CustomerID | Customer identifier. | String |
| Country | Customer's country of residence. | String |

## Derived Columns

| Column | Formula / Purpose |
|---|---|
| Revenue | Quantity × UnitPrice |
| InvoiceYear | Year extracted from InvoiceDate |
| InvoiceMonth | Month extracted from InvoiceDate |
| InvoiceDay | Day extracted from InvoiceDate |
