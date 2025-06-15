### **Comprehensive Video Summary**

This video provides a hands-on, deep-dive into using T-SQL within a Microsoft Fabric Data Warehouse, specifically tailored for DP-700 exam preparation. It moves beyond fundamental syntax to cover practical data engineering tasks. The core message is that while fundamental T-SQL knowledge is assumed, the exam requires a deeper understanding of its application in data transformation, modeling, and security within the Fabric ecosystem.

**Key Takeaways:**

- **Data Ingestion & Creation:** Several T-SQL methods for creating and populating tables are demonstrated, including CREATE TABLE, COPY INTO, INSERT INTO, and CTAS, with a focus on their specific uses and limitations in Fabric.
    
- **Dimensional Modeling:** The video explains how to implement core dimensional modeling concepts like Primary and Surrogate Keys, different types of Joins, and Slowly-Changing Dimensions (SCDs) using T-SQL.
    
- **Security:** It provides a practical recap of implementing granular security, covering Row-Level Security (RLS), Column-Level Security (CLS), Object-Level Security (OLS), and Dynamic Data Masking (DDM) with specific T-SQL syntax.
    
- **Fabric-Specific Nuances:** Throughout the tutorial, specific behaviors and limitations within the Fabric Data Warehouse are highlighted, such as how Primary Keys are NOT ENFORCED and the lack of IDENTITYcolumns requiring workarounds for surrogate key generation.
    

---

### **Detailed Explanation of Each Topic**

#### 1. T-SQL Fundamentals (Assumed Knowledge)

The video explicitly states that a foundational understanding of T-SQL is a prerequisite for the DP-700 exam and this tutorial. These fundamentals are not covered in depth but are expected knowledge.

- **Core Commands:** SELECT, WHERE, GROUP BY, HAVING, INSERT, UPDATE, DELETE.
    
- **Advanced Functions:** Ranking functions (ROW_NUMBER(), RANK()), UNION, and UNION ALL.
    
- **Database Objects:** Understanding T-SQL data types, and how to create and use Views, Stored Procedures, and Functions.
    
- **Orchestration:** Knowledge of how to run Data Warehouse queries from a Data Pipeline using activities like the Stored Procedure or Script/Lookup activity.
    

#### 2. Table Creation / Data Ingestion Methods

The video demonstrates several T-SQL methods for table creation and data population in a Fabric Data Warehouse.

- **CREATE TABLE:**
    
    - **Concept:** The standard DDL command to define a new table and its schema (columns and data types).
        
    - **Fabric Nuance:** It's noted that in Fabric Data Warehouse, you **cannot** specify a PRIMARY KEY or an IDENTITY column directly within the CREATE TABLEstatement. These must be handled with post-creation steps or workarounds. It's recommended to create tables upfront to control data types, rather than relying on auto-create functions in pipelines.
        
- **COPY INTO:**
    
    - **Concept:** A high-performance, flexible command for ingesting data from an external storage location into a Data Warehouse table. It is one of the fastest methods for ingestion into Fabric.
        
    - **Source:** Currently, it supports ingesting data from Azure Data Lake Storage Gen2 (ADLS Gen2) for files like CSV and Parquet.
        
    - **Usage:** It can be used with Trusted Workspace Access for secure connections to storage. It also supports various options, such as specifying DATETIME formats, file types, and skipping header rows (FIRSTROW = 2).
        
- **INSERT INTO ... SELECT FROM and SELECT * INTO:**
    
    - **Concept:** These are methods for creating and/or populating a table using data from another existing table.
        
    - **INSERT INTO ... SELECT:** Populates an existing table with data from a query result. This is useful for moving data between tables (e.g., from a staging table to a final table).
        
    - **SELECT * INTO:** Creates a new table and populates it with the data from a query result in a single step. The schema of the new table is inferred from the SELECT statement.
        
- **CREATE TABLE AS SELECT (CTAS):**
    
    - **Concept:** A powerful DDL statement that creates a new table and populates it with the results of a SELECT query in one atomic operation. This is highly efficient for creating summary tables or transformed copies of data.
        

#### 3. Primary Keys and Surrogate Keys

- **Primary Key Constraints:**
    
    - **Definition:** In Fabric Data Warehouse, primary keys are defined after table creation using the ALTER TABLE ... ADD CONSTRAINTsyntax.
        
    - **Fabric Nuance:** Primary key constraints are **NONCLUSTERED** and, most importantly, **NOT ENFORCED**. This means the system will not prevent you from inserting duplicate primary key values.
        
    - **Benefit:** Even though not enforced, defining them helps the query optimizer generate more efficient execution plans and serves as crucial documentation for the data model.
        
- **Surrogate Key Generation:**
    
    - **Natural vs. Surrogate Keys:** A **natural key** is an identifier from the source system (e.g., ProductID). A **surrogate key** is a unique, system-generated identifier created within the data warehouse that is independent of the source data.
        
    - **Importance:** Surrogate keys are essential for dimensional modeling, especially for handling Slowly-Changing Dimensions (SCDs), where a single natural key might have multiple historical records.
        
    - **Workaround in Fabric:** Since IDENTITY columns are not currently supported, a common workaround is to use the ROW_NUMBER() window function combined with the current maximum key value from the table to generate incremental, unique surrogate keys during the ingestion process.
        

#### 4. Dimensional Modeling in T-SQL

- **Joins (LEFT, INNER, FULL OUTER):**
    
    - **Concept:** The video emphasizes understanding the output and performance implications of different join types.
        
    - **Referential Integrity:** It highlights the issue of referential integrity violations (e.g., a key in a fact table not existing in the corresponding dimension table). An INNER JOIN would drop this record, while a LEFT JOIN would keep it and show NULL values for the columns from the missing dimension. Understanding this is critical for accurate reporting.
        
- **Implementing Slowly-Changing Dimensions (SCD Type 2):**
    
    - **Concept:** SCD Type 2 is a method for tracking the history of changes in dimension attributes over time. Instead of overwriting old data, new rows are added.
        
    - **Implementation:** This is achieved by adding metadata columns to the dimension table, typically Valid_Fromand Valid_To timestamps. When an attribute changes, the Valid_To date of the current record is updated to the current time, and a new record is inserted with the new attribute value and a new Valid_From date.
        

#### 5. Implementing Granular Security

- **Row-Level Security (RLS):**
    
    - **Concept:** Restricts which rows a user can see in a table based on their identity.
        
    - **Implementation (3 Steps):**
        
        1. **Create a table-valued function:**This function contains the filter logic, typically comparing a column in the data to the result of USER_NAME().
            
        2. **Create a security policy:** This policy binds the function to a specific table as a filter predicate.
            
        3. **Set State to ON:** The policy must be activated.
            
- **Object-Level (OLS) and Column-Level (CLS) Security:**
    
    - **Concept:** These are simpler forms of security that grant or deny access to entire database objects (like tables) or specific columns.
        
    - **Implementation:** They are implemented using standard GRANT and DENY T-SQL statements. GRANT SELECT ON table TO userprovides OLS, while GRANT SELECT ON table(column1, column2) TO userprovides CLS.
        
- **Dynamic Data Masking (DDM):**
    
    - **Concept:** Obscures sensitive data in query results for non-privileged users, while the underlying data remains unchanged. It is a presentation-layer feature.
        
    - **Implementation:** Can be applied inline during CREATE TABLE or with ALTER TABLE using the MASKED WITH (FUNCTION = ...)syntax.
        
    - **Functions:** The video mentions four masking functions: default(), email(), random(), and partial().
        

---

### **Step-by-Step UI Tutorials (T-SQL Code)**

#### 1. Ingesting Data from ADLS Gen2 with COPY INTO

This process loads data from a CSV file in ADLS Gen2 into a pre-existing warehouse table.

1. First, ensure a table exists to receive the data.
    
    ```
    -- simple CREATE TABLE statement
    CREATE TABLE [dbo].[contacts] (
        customer_id INT NOT NULL,
        customer_name VARCHAR(100),
        customer_city_id INT,
        customer_email VARCHAR(100)
    );
    ```
    
    Use code [with caution](https://support.google.com/legal/answer/13505487). SQL
    
2. Execute the COPY INTO command to load the data.
    
    ```
    -- COPY INTO (from ADLS Gen2)
    COPY INTO [dbo].[contacts]
    FROM 'https://<your_storage_account>.blob.core.windows.net/<container>/hubspot_contacts_test.csv'
    WITH (
        FILE_TYPE = 'CSV',
        FIRSTROW = 2 -- Skips the header row
    );
    ```
    
    Use code [with caution](https://support.google.com/legal/answer/13505487). SQL
    

#### 2. Adding a NOT ENFORCED Primary Key Constraint

This demonstrates how to add a primary key after a table has been created.

1. Create the table without a primary key constraint.
    
    ```
    CREATE TABLE [dbo].[products]
    (
        product_surr_key INT NOT NULL,
        product_id INT,
        product_name VARCHAR(100)
    );
    ```
    
    Use code [with caution](https://support.google.com/legal/answer/13505487). SQL
    
2. Use the ALTER TABLE statement to add the primary key constraint. Note the NONCLUSTERED and NOT ENFORCEDkeywords, which are required/standard in Fabric DW.
    
    ```
    ALTER TABLE [dbo].[products] ADD CONSTRAINT PK_Products 
    PRIMARY KEY NONCLUSTERED ([product_surr_key]) NOT ENFORCED;
    ```
    
    Use code [with caution](https://support.google.com/legal/answer/13505487). SQL
    

#### 3. Generating Surrogate Keys (Workaround for No IDENTITY Column)

This pattern generates new surrogate keys for incoming records by finding the current max key and incrementing from there.

1. Declare a variable to hold the maximum current surrogate key.
    
    ```
    DECLARE @MaxProductSurrKey AS INT;
    ```
    
    Use code [with caution](https://support.google.com/legal/answer/13505487). SQL
    
2. Check if the destination table has any records. If it does, set the variable to the maximum key value. If not, initialize it to 0.
    
    ```
    IF EXISTS(SELECT * FROM [dbo].[products])
        SET @MaxProductSurrKey = (SELECT MAX([product_surr_key]) FROM [dbo].[products]);
    ELSE
        SET @MaxProductSurrKey = 0;
    ```
    
    Use code [with caution](https://support.google.com/legal/answer/13505487). SQL
    
3. Insert new records from a staging table (staging.updated_products), generating the new surrogate key by adding the ROW_NUMBER() to the max key variable.
    
    ```
    INSERT INTO [dbo].[products]
    SELECT
        @MaxProductSurrKey + ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS [product_surr_key],
        [src].[product_id],
        [src].[product_name]
    FROM [staging].[updated_products] AS [src]
    ```
    
    Use code [with caution](https://support.google.com/legal/answer/13505487). SQL
    

#### 4. Implementing Row-Level Security (RLS)

This is a three-step process to filter data based on the logged-in user.

1. (Optional) Create a dedicated schema for security objects.
    
    ```
    CREATE SCHEMA Security;
    ```
    
    Use code [with caution](https://support.google.com/legal/answer/13505487). SQL
    
2. Create a table-valued function that defines the filter logic. It returns a result if the user's name matches the SalesReps column.
    
    ```
    CREATE FUNCTION Security.f_FilterRowsForLoggedInUser(@SalesRep AS varchar(100))
        RETURNS TABLE
    WITH SCHEMABINDING
    AS
        RETURN SELECT 1 AS f_FilterResult
        WHERE @SalesRep = USER_NAME();
    GO;
    ```
    
    Use code [with caution](https://support.google.com/legal/answer/13505487). SQL
    
3. Create a security policy to apply the function as a filter to a target table.
    
    ```
    CREATE SECURITY POLICY SalesRowFilterPolicy
    ADD FILTER PREDICATE Security.f_FilterRowsForLoggedInUser(SalesReps)
    ON Sales.Orders
    WITH (STATE = ON);
    ```
    
    Use code [with caution](https://support.google.com/legal/answer/13505487). SQL
    

---

### **DP-700 Exam-Style Questions & Answers**

<details>  
<summary><b>Question 1:</b> A data engineer needs to implement a primary key on the `DimCustomer` table in a Microsoft Fabric Data Warehouse. The primary key constraint was not defined when the table was created. Which of the following statements is correct regarding the implementation?</summary>  
A. The table must be dropped and recreated with the primary key constraint.<br>  
B. The `ALTER TABLE` statement should be used to add an enforced, clustered primary key.<br>  
C. The `ALTER TABLE` statement should be used, and the primary key will be `NONCLUSTERED` and `NOT ENFORCED`.<br>  
D. Primary keys cannot be defined on existing tables in a Fabric Data Warehouse.  
<br><br>  
<b>Correct Answer: C</b>  
<br><b>Explanation:</b> As shown in the video (7:33), in a Fabric Data Warehouse, primary keys are added to existing tables using `ALTER TABLE`. They are always `NONCLUSTERED` and `NOT ENFORCED`, meaning the system does not validate uniqueness but uses the constraint for query optimization.  
</details>  

<details>  
<summary><b>Question 2:</b> You need to load a large Parquet file from an ADLS Gen2 container into a table in your Fabric Data Warehouse. Which T-SQL command is the most efficient and recommended method for this task?</summary>  
A. `INSERT INTO ... VALUES`<br>  
B. `CTAS (CREATE TABLE AS SELECT)`<br>  
C. `COPY INTO`<br>  
D. `BULK INSERT`  
<br><br>  
<b>Correct Answer: C</b>  
<br><b>Explanation:</b> The video highlights that the `COPY INTO` statement (4:29, 4:56) is one of the fastest and most flexible methods for ingesting data into Fabric from external sources like ADLS Gen2. `BULK INSERT` is not the correct syntax for this context, and the other options are not designed for high-throughput external file ingestion.  
</details>  

<details>  
<summary><b>Question 3:</b> You are designing a dimension table, `dim_customer_scd2`, to track the history of customer addresses using the Slowly-Changing Dimension (SCD) Type 2 pattern. Which set of columns is essential for this implementation?</summary>  
A. `customer_id`, `customer_address`, `is_current`<br>  
B. `customer_surrkey`, `customer_id`, `customer_address`, `valid_from`, `valid_to`<br>  
C. `customer_id`, `old_address`, `new_address`, `change_date`<br>  
D. `customer_surrkey`, `customer_id`, `update_timestamp`  
<br><br>  
<b>Correct Answer: B</b>  
<br><b>Explanation:</b> As demonstrated in the video (24:06), implementing SCD Type 2 involves creating a unique surrogate key (`customer_surrkey`) for each historical version of a record and using `valid_from` and `valid_to` date/time columns to track the time period during which a specific version of the record was active.  
</details>  

<details>  
<summary><b>Question 4:</b> A developer in your team has defined a primary key on a table in the Fabric Data Warehouse. They are surprised when they are able to successfully insert a row with a primary key value that already exists in the table. Why did this happen?</summary>  
A. The developer does not have the correct permissions to enforce constraints.<br>  
B. The primary key constraint in a Fabric Data Warehouse is `NOT ENFORCED` by default.<br>  
C. An index must be created on the primary key column to enable enforcement.<br>  
D. The `SET_CONSTRAINTS_ON` database option is turned off.  
<br><br>  
<b>Correct Answer: B</b>  
<br><b>Explanation:</b> The video explicitly demonstrates (9:41 - 10:43) and explains that primary keys in a Fabric Data Warehouse are not enforced. This means they do not prevent the insertion of duplicate values. Their primary purpose is to inform the query optimizer and for data model documentation.  
</details>  

<details>  
<summary><b>Question 5:</b> You need to implement Row-Level Security (RLS) on the `Sales.Orders` table. What are the correct T-SQL objects that must be created in the correct sequence?</summary>  
A. 1. Create a View, 2. Create a Security Policy<br>  
B. 1. Create a Trigger, 2. Create a Security Policy<br>  
C. 1. Create a Table-Valued Function, 2. Create a Security Policy<br>  
D. 1. Create a Security Policy, 2. Create a Table-Valued Function  
<br><br>  
<b>Correct Answer: C</b>  
<br><b>Explanation:</b> The video shows the correct two-step process for RLS (28:44). First, a table-valued function is created to define the filtering logic. Second, a security policy is created to apply that function's logic as a predicate to the target table.  
</details>  

<details>  
<summary><b>Question 6:</b> You need to generate surrogate keys for a dimension table in a Fabric Warehouse. Since the `IDENTITY` property is not supported, you decide to use the `ROW_NUMBER()` function. How do you ensure the generated keys are unique and incremental with each new batch load?</summary>  
A. Use `ROW_NUMBER()` by itself, as it guarantees unique numbers across all loads.<br>  
B. Add the result of `ROW_NUMBER()` to the maximum existing surrogate key value in the target table.<br>  
C. Use `ROW_NUMBER()` partitioned by the natural key.<br>  
D. Use the `RAND()` function combined with `ROW_NUMBER()` to create a unique key.  
<br><br>  
<b>Correct Answer: B</b>  
<br><b>Explanation:</b> The video demonstrates this exact workaround (12:37). Before inserting a new batch, you find the `MAX()` value of the existing surrogate key column. Then, for each new row, you generate a new key by adding the result of `ROW_NUMBER()` to that maximum value, ensuring a continuous and unique sequence.  
</details>  

<details>  
<summary><b>Question 7:</b> When implementing Dynamic Data Masking on a column in a Fabric Data Warehouse, what is a critical security consideration to remember?</summary>  
A. DDM encrypts the data at rest, making it fully secure.<br>  
B. DDM only applies to users with read-only access to the workspace.<br>  
C. DDM is a presentation-layer feature and does not prevent users with ad-hoc query permissions from potentially inferring the original data.<br>  
D. DDM functions can only be applied to `VARCHAR` data types.  
<br><br>  
<b>Correct Answer: C</b>  
<br><b>Explanation:</b> The video warns (32:41) that Dynamic Data Masking only masks the data in the query output. A user with sufficient permissions can still run queries that could potentially expose the underlying data through inference. Therefore, it should be used in conjunction with other security measures like RLS and CLS.  
</details>