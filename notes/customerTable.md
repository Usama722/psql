Customers Table - Possible Fields

The Customers Table may contain the following columns:

1. id
   - Type: INT / SERIAL
   - Description: Primary key. Should **not change**. Used for referencing in other tables.

2. name
   - Type: VARCHAR
   - Description: Name of the customer. Can be updated.

3. address
   - Type: VARCHAR
   - Description: Customer's address. Can be updated.

4. city
   - Type: VARCHAR
   - Description: City of the customer. Can be updated.

5. phone
   - Type: VARCHAR
   - Description: Contact number of the customer. Can be updated.

6. activeStatus
   - Type: BOOLEAN / TINYINT
   - Description: Indicates if the customer is active. Can be updated.

7. created_at
   - Type: TIMESTAMP / DATETIME
   - Description: Record creation date. Usually set once at creation.

Notes:
- The `id` column is **immutable** for reference integrity.
- Other columns like `name`, `address`, `city`, `phone`, `activeStatus`, and `created_at` can be updated as needed.
- Use `id` when creating **relationships** with other tables (foreign keys).
