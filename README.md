# Entity_FrameworkCore5_FromScratch

1. create ASP.net core web app > Wiblib
2. create data clas libraries


packages:

Microsoft.EntityFrameworkCore > install in webapp,dataaccess project
Microsoft.EntityFrameworkCore.SQLServer > install in webapp,dataaccess

## DBContext
* Instance of DBContext represents a session with th DB which can be used to query and save instances of your entities to a database.
* its allows to perform
    * Manage database connection
    * configure model & relationship
    * Querying database
    * Saving data to database
    * configure change tracking
    * Caching
    * Transation management


* Model class + Mappings => Database 
* Database must be in synk with models and mappings

## Migrations in EFCore
1. Change/ Create Model :You should first create new model or change existing model
2. Add Migration: Add a new migration once you make your changes to see what will be pushed to database
3. Apply Migration: Once a migration is added use the updated-database to push migration

* install package > Microsoft.EntityFrameworkCore.Tools > install in webapp,dataaccess

Goto > Tools > nuget package manager > package manager console

run inside dataAccess project because DBContext classes exist there. Also startup project as web app.(if you keep model project as startup it throws error as it is not using DBContext)

    >   add-migration AddCategoryTableToDb

* above command creates a migration file (xxxx_AddCategoryTableToDb) and snapshot file (ApplicationDbContextModelSnapShot)
* snapshot class file keeps a backup of what exactly as been pushed to database so far. it is a tracker of what migration as been applied and what is the current state of DB.so it can push the difference during update-databse command

 >  update-databse

 * above command pushes the migration to DB

 * NOTE: always create small changes migrations so it can be identitied with name. also easy to rollback

 >   add-migration AddGenreTableToDb

 ## Migration Scenarios
 1. Add a new class/table in the db
 2. Add new property/column to table
 3. Modify existing property/column in a table
 4. Delete existing property/column in a table
 5. Delete a class/table in the database

>  add-migration AddDisplayOrderToGenreTable
* new migration to add new column

>  add-migration ChangeNameToGenereNameInGenreTable

 * above command could result in loss of data as it deletes and add new column
 * to avoid this run SQL staatement (copy) before running dropColumn inside "xxxx_ChangeNameToGenreNameInGenreTable" migration file

> add-migration RemoveDisplayOrderColumnFromGenreTable
* causes data loss

* NOTE: whenever you deal with migrations, if you donno, don't delete migration class in migrations folder. DB will become unstable state.
else In beginning you can delete all migrations folder and run new migrations in new database. don't keep changes names and props in migrations folder, will be inconsistant db.
* migrations are source countrol friendly

 > update-database AddGenreTableToDb
 * above command revert the state of the DB to AddGenreTableToDb migration
* now you can work with that state

> update-database
* above command make the DB back to current state by running all migrations

* check the migrations history in migrations table

#### to remove migrations
1. run 'update-database lastMigrationNameYouWantDbTobe' then delete following migrations. (not recommanded)
2. change the models and add new migration (prefer) 



