-- Creating table for new Topic Foreign Key
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER,
    order_date DATE DEFAULT current_date,

    constraint fk_customers
    Foreign Key (customer_id) REFERENCES customers(id)
);
-- Query Success fully execute
-- now on defaul position on Delete restricted, but i want to empliment casecad for deleting child record

-- so basically we drop old fk before to apply new condition due to psql working method constraint not to over write so first we drop old then apply new

ALTER TABLE orders
DROP constraint fk_customers; -- query execute success

-- now we need to add with delete casecade
ALTER TABLE orders
ADD constraint fk_customers
Foreign Key (customer_id) REFERENCES customers(id) on delete CASCADE;
-- success query 
SELECT * FROM products;
-- add new table for detil of orders with some checks for restrict adding abnormal data
CREATE Table orders_items(
    id SERIAL PRIMARY KEY,
    order_id int REFERENCES orders(id), -- new style to add FOREIGN key inline style
    product_id int REFERENCES products(id),
    qty int check( qty > 0), -- safe for negative or 0 qty 
    price NUMERIC(10,2) check( price >= 0), -- safe for negative price but 0 price allow for like bonus piece

    UNIQUE(product_id, order_id)-- for RESTRICT duplicate skue on same order
); ---Execution completed in 66ms
SELECT * from customers;
-- Now we move to next topic \\\ INDEXES \\\
-- indexes is performance booster database sort data on every insertion or updation behalf on index for fast fetching data
CREATE INDEX idx_customer_name
ON customers(name);
-- here is some mistak in index name it is typo error but now we rename over index
ALTER INDEX idx_customer_name
RENAME to idx_customers_name; -- more relevent to customers table
-- Query Success
-- adding some other indexes
SELECT * FROM products;
CREATE INDEX idx_products_name
ON products(name);
