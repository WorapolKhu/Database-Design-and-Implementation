# Database-Design-and-Implementation
Design and implement a central relational database system for a coffee shop chain expanding nationally, streamlining operations and enabling data-driven decisions.

## Scenario
In this scenario, I have recently been hired as a Data Engineer by a New York-based coffee shop chain looking to expand nationally by opening several franchise locations. They want to streamline operations and revamp their data infrastructure as part of their expansion process.

My job is to design their relational database systems for improved operational efficiencies and make it easier for their executives to make data-driven decisions.

Currently, their data resides in several systems: accounting software, supplier databases, point of sales (POS) systems, and even spreadsheets. I will review the data in all of these systems and design a central database to house all of the data. I will then create the database objects and load them with source data. Finally, I will create subsets of data my business partners require, export them, and load them into staging databases using several RDBMS.

## Objectives
- Identify entities
- Identity attributes
- Create an entity relationship diagram (ERD) using the pgAdmin ERD tool
- Normalize tables
- Define keys and relationships
- Create database objects by generating and running the SQL script from the ERD tool
- Create a view and export the data
- Create a materialized view and export the data
  - Import data into a MySQL database using phpMyAdmin GUI tool
  - Import data into a MySQL database

## Software used in this project
-  PostgreSQL Database
-  MySQL

## Data Source: [Coffee shop sample data](https://community.ibm.com/community/user/businessanalytics/blogs/steven-macko/2019/07/12/beanie-coffee-1113?utm_source=Exinfluencer&utm_content=000026UJ&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDB0110ENSkillsNetwork24601058-2021-01-01&utm_medium=Exinfluencer&utm_term=10006555)

- A subset of data from the Coffee shop sample data.
- Staff information held in a spreadsheet at headquarters (HQ)
- Sales outlet information held in a spreadsheet at HQ
- Sales data output as a CSV file from the POS system in the sales outlets
- Customer data output as a CSV file from a custom customer relationship management system
- Product information maintained in a spreadsheet exported from your supplier's database

## Project tasks
### Task 1: Identify entities

![existing_data](https://github.com/WorapolKhu/Database-Design-and-Implementation/assets/93505768/c5a2c403-f52e-42da-aaba-cfc61c4ad0eb)
<img width="202" alt="Task1" src="https://github.com/WorapolKhu/Database-Design-and-Implementation/assets/93505768/9605dfb8-3faf-47ff-a1b1-13791c2bf8bf">

### Task 2: Identify attributes
Identify the entity's attributes that will store the sales transaction data

<img width="218" alt="Task2" src="https://github.com/WorapolKhu/Database-Design-and-Implementation/assets/93505768/f8a93858-d4f3-44b9-abd2-ba26525c3226">

### Task 3: Create an ERD using the pgAdmin ERD Tool

![Task 3 Create an ERD](https://github.com/WorapolKhu/Database-Design-and-Implementation/assets/93505768/e16eb8d1-8614-42c8-9d09-3ea29fdde16d)

### Task 4: Normalize tables
The ERD has been improved to comply with the second normal form (2NF) by addressing data redundancy:

**Sales Transaction Normalization:**

A new table named `sales_detail` has been added.

The `sales_transaction` table no longer includes columns for individual product details (e.g., product ID, quantity).

The `sales_transaction` table maintains a reference to the sales_detail table through a matching column (e.g., transaction_id).

**Product Normalization:**

A new table named `product_type` has been added.

The `product` table no longer includes columns for product category and type.

The `product` table maintains a reference to the `product_type` table through a matching column (e.g., product_type_id).

**Result:**

![Task 4 Normalize tables](https://github.com/WorapolKhu/Database-Design-and-Implementation/assets/93505768/72652895-eb42-4646-ba37-9e0e281f12d2)

### Task 5: Define keys and relationships

Identify an appropriate column in each table to be a primary key and create the primary keys in the tables in the entity-relationship diagram (ERD).

Identify the relationships between the following pairs of tables and then create the relationships in ERD:

`sales_detail` to `sales_transaction`

`sales_detail` to `product`

`product` to `product_type`

**Result:**

<img width="565" alt="Task 5 Define keys and relationships" src="https://github.com/WorapolKhu/Database-Design-and-Implementation/assets/93505768/5b6b2c0f-9271-4dad-bc0f-4a2477418919">

### Task 6: Create database objects by generating and running the SQL script from the ERD Tool

**Generating the SQL Script**

1. Utilize the Generate SQL functionality within ERD tool to create a script reflecting the database schema.
  
2. Download the generated script (e.g., `GeneratedScript.sql`) to local machine.

**Creating Tables**

1. In pgAdmin, open the Query Tool.

2. Upload and open the downloaded `GeneratedScript.sql` file.

3. Execute the script to create the tables defined in the ERD within the `COFFEE` database's public schema.

4. Verify the existence of the tables in the pgAdmin tree view pane.

**Loading Data**

1. Open another instance of the pgAdmin Query Tool.

2. Download the provided `CoffeeData.sql` file to local machine.

3. Upload and open the downloaded `CoffeeData.sql` file.

4. Execute the script to populate the tables created.

**Verifying Data**

1. In pgAdmin, view the first 100 rows of the `sales_detail` table using a query like:

   ```sql
   SELECT * FROM sales_detail LIMIT 100;
   ```
**Result:**

<img width="921" alt="Task6B" src="https://github.com/WorapolKhu/Database-Design-and-Implementation/assets/93505768/987cbc48-3cdd-4b6d-8f4b-f78520a36aaa">

### Task 7: Create a view and export the data
create a new view named `staff_locations_view` using the following SQL:
 ```sql
SELECT staff.staff_id,
staff.first_name,
staff.last_name,
staff.location
FROM staff
WHERE "position" NOT IN ('CEO', 'CFO');
 ```

View all the rows returned from the view.

**Result:**

<img width="923" alt="Task7" src="https://github.com/WorapolKhu/Database-Design-and-Implementation/assets/93505768/97ceb93f-271a-47c1-a4c9-2ecd85ee5051">

### Task 8: Create a materialized view and export the data
create a new materialized view named `product_info_m-view` using the following SQL:
```sql
SELECT product.product_name, product.description, product_type.product_category
FROM product
JOIN product_type
ON product.product_type_id = product_type.product_type_id;
```

Refresh the materialized view with data.

View all the rows returned from the view.

**Result:**

<img width="921" alt="Task8" src="https://github.com/WorapolKhu/Database-Design-and-Implementation/assets/93505768/b9e5e54c-c14c-4c36-87e3-97e9158da110">

### Task 9: Import data into a MySQL database using phpMyAdmin GUI tool

In phpMyAdmin, create a new database named `STAFF_LOCATIONS`, then import the location information saved in the `staff_locations_view.csv` file that exported from the view created in Task 7.

**Result:**

<img width="920" alt="Task9" src="https://github.com/WorapolKhu/Database-Design-and-Implementation/assets/93505768/ccd551df-c58b-43c8-ad03-692ada27769a">

### Task 10: Import data into a MySQL database

In phpMyAdmin, create a new database named `coffee_shop_products`, and then import the product information saved in the `product_info_m-view.csv` file from the materialized view into a new table in the coffee_shop_products database.

**Result:**

<img width="922" alt="Task10" src="https://github.com/WorapolKhu/Database-Design-and-Implementation/assets/93505768/b4faccf5-107a-4788-8a55-0cb356bc8b1f">
