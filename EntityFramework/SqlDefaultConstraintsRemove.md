# Entity Framework Sql Server table Default Constraints error fix

## Some times we want to change a property type from int to decimal for example, then we will get error, because Sql already made a DF_* constraints for our int field in our database table

### Here is an extension method that we can add to our data layer and use it inside our migration.cs file to delete all default constraints

```C#
  internal static class MigrationExtensions
    {
        public static void DeleteDefaultContraint(this IDbMigration migration, string tableName, string colName, bool suppressTransaction = false)
        {
            var sql = new SqlOperation(String.Format(@"DECLARE @SQL varchar(1000)
                SET @SQL='ALTER TABLE {0} DROP CONSTRAINT ['+(SELECT name
                FROM sys.default_constraints
                WHERE parent_object_id = object_id('{0}')
                AND col_name(parent_object_id, parent_column_id) = '{1}')+']';
                PRINT @SQL;
                EXEC(@SQL);", tableName, colName)) { SuppressTransaction = suppressTransaction };
            migration.AddOperation(sql);
        }
    }
```

#### Here is the usage

```C#
 this.DeleteDefaultContraint("dbo.tableName", "ColumnNameWhichHasDefaultConstraint");
```