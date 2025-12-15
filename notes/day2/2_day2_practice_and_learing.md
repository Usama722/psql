-- Customer Table
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL,
    phone VARCHAR(20),
    active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT current_timestamp
);

-- Product table
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    sku VARCHAR(50) UNIQUE NOT NULL,
    price NUMERIC(10,2) NOT NULL DEFAULT 0.00,
    stock INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Customers isnertion
INSERT INTO customers (name, email, phone)
VALUES
('Ali Khan', 'ali@example.com', '03001234567'),
('Sara Ahmed', 'sara@example.com', '03111234567');

-- Products
INSERT INTO products (name, sku, price, stock)
VALUES
('Wheel', 'WH-001', 500, 20),
('Tyre', 'TY-002', 1200, 10);
-- complete view for customer table
select * from customers;
-- Only record that stock quantity > 10
select * FROM products where stock > 10;
-- only show name and stock column from produts
SELECT name, stock from products;
-----||||| Practice Exercises ||||-------
--1 Apni marzi ke 3-5 records insert karain har table men.
INSERT INTO customers(name, email, phone)
VALUES
('Usama', 'usama11@gmail.com','066277632'),
('Abid', 'abidsultan@gamil.com', '066277644'),
('Nisar Husain', 'n1123@gamil.com', '03007788555');
INSERT INTO products(name, sku, price, stock)
VALUES
('Head Light','hl001',700,2),
('Back Light','bl001',400,7),
('Indicator Lights set','ils001',1500,23);
---Customer ka phone update karain.
UPDATE customers
SET phone = '03091122333'
WHERE name = 'Usama';
--Product delete kar ke phir insert karain.
DELETE FROM products WHERE sku = 'hl001';
INSERT INTO products(name, sku, price, stock)
VALUES
('Head Ligh', 'hl001', 700, 15);
--(Verify inertion)
SELECT * FROM products WHERE sku = 'hl001'; -- result found with new id(6)
--=== Constraints test karain:
----step 1 Duplicate email insert karne ki koshish
INSERT INTO customers(name, email, phone)
VALUES
('Mughal ', 'n1123@gamil.com', '03007722555'); -- oh no insertion successfully due to not apply unique constraints
-- try to update constraint
UPDATE column FROM customers
where email = email VARCHAR(100) UNIQUE NOT NULL; -- syntex error 
-- insertion negative stock
INSERT INTO products(name, sku, price, stock)
VALUES
('clutch cable','cc001',150,-2); -- insertion success
-- adding check constraint
ALTER TABLE products 
ADD constraint stock_positve check (stock >= 0); -- not altering error is voilate some rows
-- correct apply unique constraint on email
ALTER TABLE customers
ADD CONSTRAINT customers_email_unique UNIQUE (email);

SELECT email, count(*)
FROM customers 
GROUP BY email
HAVING count(*)>1;

DELETE from customers WHERE email = 'n1123@gamil.com';
ALTER TABLE customers
ADD CONSTRAINT customers_email_unique UNIQUE (email);

SELECT * from customers;
INSERT INTO customers(name, email, phone)
VALUES
('Sara Nisar ', 'sara@example.com', '03107722555'); -- working uniqu constrint on email properly

UPDATE products
set stock = 0
WHERE stock <0;
SELECT * from products;

-- adding check constraint after updating negative stock convert to 0 
ALTER TABLE products 
ADD constraint stock_positve check (stock >= 0);
-- now again try to add negative stock
INSERT INTO products(name, sku, price, stock)
VALUES
('Brakc cable','bc001',150,-5); -- successfully error found voilate check constraint
