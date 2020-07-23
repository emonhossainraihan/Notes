- **OGM(Object Graph Mapper):** maps between an object model and a Relational Database 
- **ORM(Object Relational Modeling):** maps between an object model and a Relational Database 
- **ODM(Object Data Modeling):** maps between an object model and a Document Database 

## Database Query: ORM vs SQL

Object relational mappers (ORM) are utilized to abstract database access into your preferred object oriented programming language. On the other hand, raw SQL statements are a more direct way of communicating with a database. Usually raw SQL statements are either embedded into your database access layer or stored in your database as a stored procedure. Each approach has its advantages and disadvantages.

Usually, the most important consideration when choosing a database access method is the complexity of the query. For simple CRUD queries,  the ORM tends to be best. For very complex queries, which usually involve data manipulation and calculation, SQL tends to be the way to go.  Most large applications tend to utilize both the ORM and SQL for database access.

- knex, A batteries-included SQL query & schema builder for Postgres, MySQL and SQLite3 and the Browser. It was authored by Tim Griesser on May, 2013.
- orm, NodeJS Object-relational mapping. It was authored by Diogo Resende on Mar, 2011.
- sequelize, Multi dialect ORM for Node.JS. It was authored on May, 2011.
- typeorm, Data-Mapper ORM for TypeScript, ES7, ES6, ES5. Supports MySQL, PostgreSQL, MariaDB, SQLite, MS SQL Server, Oracle, MongoDB databases. It was authored by Umed Khudoiberdiev on Apr, 2016
