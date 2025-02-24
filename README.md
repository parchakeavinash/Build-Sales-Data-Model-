# Data Modeling in PostgreSQL (pgAdmin)

## Overview
This project demonstrates data modeling in PostgreSQL using pgAdmin. It involves creating a relational database with multiple tables, defining relationships, and inserting data from CSV files using Python.

## Project Structure
1. **Define Dataset & Tables**
   - **Customers**: Stores customer details.
   - **Products**: Stores product information.
   - **Orders**: Stores order details.
   - **OrderDetails**: Links orders and products (Many-to-Many relationship).

2. **Build Data Model (ER Diagram)**
   - A customer can place multiple orders (**One-to-Many**).
   - An order can contain multiple products, and a product can be in multiple orders (**Many-to-Many**, handled by `OrderDetails`).

3. **Write Python Code**
   - Use `psycopg2` to connect to PostgreSQL and create tables.
   - Insert data from CSV files using `pandas`.

## Table Schema

### Customers Table
```sql
CREATE TABLE IF NOT EXISTS Customers (
    customer_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    phone VARCHAR(20),
    address TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
```sql
CREATE TABLE IF NOT EXISTS Products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2),
    stock_quantity INT
);
```
```sql
CREATE TABLE IF NOT EXISTS Orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES Customers(customer_id) ON DELETE CASCADE,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10,2),
    status VARCHAR(20) CHECK (status IN ('Pending', 'Completed', 'Cancelled'))
);
```
```sql
CREATE TABLE IF NOT EXISTS OrderDetails (
    order_id INT REFERENCES Orders(order_id) ON DELETE CASCADE,
    product_id INT REFERENCES Products(product_id) ON DELETE CASCADE,
    quantity INT,
    subtotal DECIMAL(10,2),
    PRIMARY KEY (order_id, product_id)
);
```
## Author
### Avinash Parchake
```sql

Copy and paste this into your GitHub README file. Let me know if you need any modifications! 🚀
