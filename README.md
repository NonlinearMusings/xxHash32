SqlxxHashSharp
===========

A SQLCLR SAFE implementation of xxHashSharp, which returns a xxHash32 has as SqlInt32, that can be deployed to SQL Server or Azure SQL Managed Instance.

Note that only SAFE assemblies are supported in Linux and as such only [Supported Libraries](https://learn.microsoft.com/en-us/sql/relational-databases/clr-integration/database-objects/supported-net-framework-libraries?view=sql-server-ver16#supported-libraries) maybe be referenced in the assembly. xxHashSharp's code base meets this requirement.

## Path to implementation

1. Compile xxHashSharp.cs to a .NET Framework 4.8 assembly named SqlxxHashSharp.dll
2. Copy SqlxxHashSharp.dll to a location accessible by SQL Server.
3. Create the assembly in a database:
```sql
create assembly xxhash32 from 'c:/tmp/CLR/SqlXxHashSharp.dll' with permission_set = safe;
```
4. Create a function referencing the method to be called:
```sql
create function dbo.XxHash32(@Value varbinary(max))
returns int
external name xxhash32.xxHashSharp.HashToInt32;
```
5. Test it...
```sql
declare @vb varbinary(max) = convert(varbinary, 'hello world');
select dbo.XxHash32(@vb);
```
6. Verfiy it...

-826579422 is 0xCEBB6622, which matches the output from [Generate a xxh hash value](https://www.coderstool.com/xxh-hash-generator)

8. Declare victory!
