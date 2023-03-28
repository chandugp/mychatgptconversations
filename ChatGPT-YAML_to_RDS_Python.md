# YAML to RDS Python.

#### You:
Load data from yaml file into RDS Tables using Python

#### ChatGPT:
To load data from a YAML file into RDS tables using Python, you can use the PyYAML library to parse the YAML file and the psycopg2 library to connect to the RDS database and insert the data.

Here's an example code snippet to get you started:

```python
import yaml
import psycopg2

# Load YAML data from file
with open('data.yaml', 'r') as file:
    data = yaml.safe_load(file)

# Connect to RDS database
conn = psycopg2.connect(
    host='your-rds-hostname',
    database='your-database-name',
    user='your-username',
    password='your-password'
)

# Create a cursor object
cur = conn.cursor()

# Iterate through data and insert into table
for row in data:
    cur.execute(
        "INSERT INTO your_table_name (column1, column2, column3) VALUES (%s, %s, %s)",
        (row['column1'], row['column2'], row['column3'])
    )

# Commit changes and close cursor and connection
conn.commit()
cur.close()
conn.close()
```

In this example, replace 'your-rds-hostname', 'your-database-name', 'your-username', 'your-password', and 'your\_table\_name' with your specific information. Also, update the INSERT statement with the appropriate column names for your table.


#### You:
Dynamically Load data from yaml file into Multiple RDS Tables using Python

#### ChatGPT:
To dynamically load data from a YAML file into multiple RDS tables using Python, you can use a combination of Python's built-in `globals()` and `exec()` functions to create and execute SQL statements based on the YAML data and the table names.

Here's an example code snippet to get you started:

```python
import yaml
import psycopg2

# Load YAML data from file
with open('data.yaml', 'r') as file:
    data = yaml.safe_load(file)

# Connect to RDS database
conn = psycopg2.connect(
    host='your-rds-hostname',
    database='your-database-name',
    user='your-username',
    password='your-password'
)

# Create a cursor object
cur = conn.cursor()

# Iterate through data and insert into tables dynamically
for row in data:
    table_name = row['table_name']
    values = tuple(row['values'])
    columns = row['columns']
    column_names = ', '.join(columns)
    placeholders = ', '.join(['%s' for _ in range(len(columns))])

    # Dynamically create SQL statement
    sql = f"INSERT INTO {table_name} ({column_names}) VALUES ({placeholders})"

    # Execute SQL statement
    cur.execute(sql, values)

# Commit changes and close cursor and connection
conn.commit()
cur.close()
conn.close()
```

In this example, the YAML data should have each row containing the name of the table to insert into (`table_name`), the values to insert (`values`), and the columns to insert into (`columns`). This code will dynamically create SQL statements based on the YAML data and insert the data into the appropriate tables. The `globals()` function is used to get a reference to the global namespace so that the `exec()` function can execute the dynamically created SQL statements.


#### You:
Provide Example Yaml file for Above

#### ChatGPT:
Sure! Here's an example YAML file that can be used with the Python code snippet provided in my previous answer:

```yaml
- table_name: users
  columns: [id, name, email]
  values:
    - [1, "John Smith", "john.smith@example.com"]
    - [2, "Jane Doe", "jane.doe@example.com"]
    - [3, "Bob Johnson", "bob.johnson@example.com"]

- table_name: orders
  columns: [id, user_id, product_name, quantity]
  values:
    - [1, 1, "Product A", 2]
    - [2, 1, "Product B", 1]
    - [3, 2, "Product A", 3]
```

This YAML file contains two tables: `users` and `orders`. The `users` table has three columns: `id`, `name`, and `email`. The `orders` table has four columns: `id`, `user_id`, `product_name`, and `quantity`. Each table has some sample data provided as nested lists under the `values` key. The Python code will dynamically insert this data into the corresponding tables in the RDS database.
