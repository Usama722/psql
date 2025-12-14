Product Table - Possible Fields

The Product Table may contain the following columns:

1. id
   - Type: INT / SERIAL
   - Description: Primary key. Should not change. Used for referencing in other tables.

2. product_name
   - Type: VARCHAR
   - Description: Name of the product.

3. category
   - Type: VARCHAR
   - Description: Category to which the product belongs.

4. unit
   - Type: VARCHAR
   - Description: Unit of measurement for the product (e.g., kg, liter, piece).

Notes:
- The 'id' column is crucial for reference integrity. It should remain immutable.
- Other columns like 'product_name', 'category', and 'unit' can be updated as needed.
- Use 'id' when creating relationships with other tables (e.g., foreign keys).
