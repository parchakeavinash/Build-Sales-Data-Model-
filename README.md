# Data Modeling in PostgreSQL (pgAdmin)

## Overview
This project demonstrates data modeling in PostgreSQL using pgAdmin. It involves creating a relational database with multiple tables, defining relationships, and inserting data from CSV files using Python.
![image](https://github.com/user-attachments/assets/18259a73-b453-40c4-8d22-2f012605b2f6)

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
# Data Insertion from CSV Files

## Create CSV Files
- `customers.csv`
- `products.csv`
- `orders.csv`
- `order_details.csv`

## Insert Data Using Python
- Load data using `pandas`
- Insert records using `psycopg2`

## Python Script for Data Insertion

```python
import pandas as pd
import psycopg2

# Connect to PostgreSQL
conn = psycopg2.connect(
    dbname="your_database",
    user="your_username",
    password="your_password",
    host="localhost",
    port="5432"
)
cur = conn.cursor()

# Function to insert data into tables
def insert_data(table, dataframe):
    for _, row in dataframe.iterrows():
        values = tuple(row)
        placeholders = ", ".join(["%s"] * len(values))
        query = f"INSERT INTO {table} VALUES ({placeholders})"
        cur.execute(query, values)

# Load CSV files
customers = pd.read_csv("customers.csv")
products = pd.read_csv("products.csv")
orders = pd.read_csv("orders.csv")
order_details = pd.read_csv("order_details.csv")

# Insert data into tables
insert_data("Customers", customers)
insert_data("Products", products)
insert_data("Orders", orders)
insert_data("OrderDetails", order_details)

# Commit changes and close connection
conn.commit()
cur.close()
conn.close()
```
## Conclusion
### This project demonstrates relational database design, data modeling, and programmatic data insertion using Python and PostgreSQL. It follows best practices for ensuring data integrity and enforcing ### constraints through foreign keys and checks.
## Author
### Avinash Parchake

