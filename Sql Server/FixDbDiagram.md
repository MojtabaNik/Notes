# Sql Server Database Creating New Diagram Error

## Here is a fix (Change MyDb to your database name)

```sql
USE MyDb 
GO 
ALTER DATABASE MyDB set TRUSTWORTHY ON; 
GO 
EXEC dbo.sp_changedbowner @loginame = N'sa', @map = false 
GO 
sp_configure 'show advanced options', 1; 
GO 
RECONFIGURE; 
GO 
sp_configure 'clr enabled', 1; 
GO 
RECONFIGURE; 
GO
```