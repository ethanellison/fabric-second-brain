URL Source: https://learn.microsoft.com/en-us/fabric/data-warehouse/dynamic-data-masking

Markdown Content:
Dynamic data masking in Fabric Data Warehouse - Microsoft Fabric | Microsoft Learn

===============
In this article
---------------

1.   [Define a dynamic data mask](https://learn.microsoft.com/en-us/fabric/data-warehouse/dynamic-data-masking#define-a-dynamic-data-mask)
2.   [Security consideration: bypassing masking using inference or brute-force techniques](https://learn.microsoft.com/en-us/fabric/data-warehouse/dynamic-data-masking#security-consideration-bypassing-masking-using-inference-or-brute-force-techniques)
3.   [Related content](https://learn.microsoft.com/en-us/fabric/data-warehouse/dynamic-data-masking#related-content)
4.   [Next step](https://learn.microsoft.com/en-us/fabric/data-warehouse/dynamic-data-masking#next-step)

**Applies to:** ✅ SQL analytics endpoint and Warehouse in Microsoft Fabric

Dynamic data masking limits sensitive data exposure by masking it to nonprivileged users. It can be used to greatly simplify the design and coding of security in your application.

Dynamic data masking helps prevent unauthorized viewing of sensitive data by enabling administrators to specify how much sensitive data to reveal, with minimal effect on the application layer. Dynamic data masking can be configured on designated database fields to hide sensitive data in the result sets of queries. With dynamic data masking, the data in the database isn't changed, so it can be used with existing applications since masking rules are applied to query results. Many applications can mask sensitive data without modifying existing queries.

*   A central data masking policy acts directly on sensitive fields in the database.
*   Designate privileged users or roles that do have access to the sensitive data.
*   Dynamic data masking features full masking and partial masking functions, and a random mask for numeric data.
*   Simple Transact-SQL commands define and manage masks.

The purpose of dynamic data masking is to limit exposure of sensitive data, preventing users who shouldn't have access to the data from viewing it. Dynamic data masking doesn't aim to prevent database users from connecting directly to the database and running exhaustive queries that expose pieces of the sensitive data.

Dynamic data masking is complementary to other Fabric security features like [column-level security](https://learn.microsoft.com/en-us/fabric/data-warehouse/column-level-security) and [row-level security](https://learn.microsoft.com/en-us/fabric/data-warehouse/row-level-security). It's highly recommended to use these data protection features together in order to protect the sensitive data in the database.

[](https://learn.microsoft.com/en-us/fabric/data-warehouse/dynamic-data-masking#define-a-dynamic-data-mask)
Define a dynamic data mask
--------------------------

A masking rule can be defined on a column in a table, in order to obfuscate the data in that column. There are four types of masks available.

Expand table

| Function | Description | Examples |
| --- | --- | --- |
| Default | Full masking according to the data types of the designated fields. For string data types, use `XXXX` (or fewer) if the size of the field is fewer than 4 characters (**char**, **nchar**, **varchar**, **nvarchar**, **text**, **ntext**). For numeric data types use a zero value (**bigint**, **bit**, **decimal**, **int**, **money**, **numeric**, **smallint**, **smallmoney**, **tinyint**, **float**, **real**). For date and time data types, use `1900-01-01 00:00:00.0000000` (**date**, **datetime2**, **datetime**, **datetimeoffset**, **smalldatetime**, **time**). For binary data types use a single byte of ASCII value 0 (**binary**, **varbinary**, **image**). | Example column definition syntax: `Phone# varchar(12) MASKED WITH (FUNCTION = 'default()') NULL` Example of alter syntax: `ALTER COLUMN Gender ADD MASKED WITH (FUNCTION = 'default()')` |
| Email | Masking method that exposes the first letter of an email address and the constant suffix ".com", in the form of an email address. `aXXX@XXXX.com`. | Example definition syntax: `Email varchar(100) MASKED WITH (FUNCTION = 'email()') NULL` Example of alter syntax: `ALTER COLUMN Email ADD MASKED WITH (FUNCTION = 'email()')` |
| Random | A random masking function for use on any numeric type to mask the original value with a random value within a specified range. | Example definition syntax: `Account_Number bigint MASKED WITH (FUNCTION = 'random([start range], [end range])')` Example of alter syntax: `ALTER COLUMN [Month] ADD MASKED WITH (FUNCTION = 'random(1, 12)')` |
| Custom String | Masking method that exposes the first and last letters and adds a custom padding string in the middle. `prefix,[padding],suffix` If the original value is too short to complete the entire mask, part of the prefix or suffix isn't exposed. | Example definition syntax: `FirstName varchar(100) MASKED WITH (FUNCTION = 'partial(prefix,[padding],suffix)') NULL` Example of alter syntax: `ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)')` This turns a phone number like `555.123.1234` into `5XXXXXXX`. Additional example: `ALTER COLUMN [Phone Number] ADD MASKED WITH (FUNCTION = 'partial(5,"XXXXXXX",0)')` This turns a phone number like `555.123.1234` into `555.1XXXXXXX`. |

For more examples, see [How to implement dynamic data masking in Fabric Data Warehouse](https://learn.microsoft.com/en-us/fabric/data-warehouse/howto-dynamic-data-masking).

[](https://learn.microsoft.com/en-us/fabric/data-warehouse/dynamic-data-masking#permissions)
### Permissions

Users without the Administrator, Member, or Contributor rights on the workspace, and without elevated permissions on the Warehouse, will see masked data.

You don't need any special permission to create a table with a dynamic data mask, only the standard`CREATE TABLE`and`ALTER`on schema permissions.

Adding, replacing, or removing the mask of a column, requires the`ALTER ANY MASK`permission and`ALTER`permission on the table. It's appropriate to grant`ALTER ANY MASK`to a security officer.

Users with`SELECT`permission on a table can view the table data. Columns that are defined as masked will display masked data. Grant the`UNMASK`permission to a user to enable them to retrieve unmasked data from the columns for which masking is defined.

The`CONTROL`permission on the database includes both the`ALTER ANY MASK`and`UNMASK`permission that enables the user to view unmasked data. Administrative users or roles such as Admin, Member, or Contributor have CONTROL permission on the database by design and can view unmasked data by default. Elevated permissions on the Warehouse include `CONTROL` permission.

[](https://learn.microsoft.com/en-us/fabric/data-warehouse/dynamic-data-masking#security-consideration-bypassing-masking-using-inference-or-brute-force-techniques)
Security consideration: bypassing masking using inference or brute-force techniques
-----------------------------------------------------------------------------------

Dynamic data masking is designed to simplify application development by limiting data exposure in a set of predefined queries used by the application. While Dynamic Data Masking can also be useful to prevent accidental exposure of sensitive data when accessing data directly, it's important to note that unprivileged users with query permissions can apply techniques to gain access to the actual data.

As an example, consider a user that has sufficient privileges to run queries on the Warehouse, and tries to 'guess' the underlying data and ultimately infer the actual values. Assume that we have a mask defined on the `[Employee].[Salary]` column, and this user connects directly to the database and starts guessing values, eventually inferring the `[Salary]` value in the `Employees` table:

SQL Copy

```sql
SELECT ID, Name, Salary FROM Employees
WHERE Salary > 99999 and Salary < 100001;
```

Results in:

Expand table

| ID | Name | Salary |
| --- | --- | --- |
| 62543 | Jane Doe | 0 |
| 91245 | John Smith | 0 |

This demonstrates that dynamic data masking shouldn't be used alone to fully secure sensitive data from users with query access to the Warehouse or SQL analytics endpoint. It's appropriate for preventing sensitive data exposure, but doesn't protect against malicious intent to infer the underlying data.

It's important to properly manage object-level security with [SQL granular permissions](https://learn.microsoft.com/en-us/fabric/data-warehouse/sql-granular-permissions), and to always follow the minimal required permissions principle.