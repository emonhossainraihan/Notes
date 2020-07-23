- **OGM(Object Graph Mapper):** maps between an object model and a Relational Database 
- **ORM(Object Relational Modeling):** maps between an object model and a Relational Database 
- **ODM(Object Data Modeling):** maps between an object model and a Document Database 

## Database Query: ORM vs SQL

Object relational mappers (ORM) are utilized to abstract database access into your preferred object oriented programming language. On the other hand, raw SQL statements are a more direct way of communicating with a database. Usually raw SQL statements are either embedded into your database access layer or stored in your database as a stored procedure. Each approach has its advantages and disadvantages.

Usually, the most important consideration when choosing a database access method is the complexity of the query. For simple CRUD queries,  the ORM tends to be best. For very complex queries, which usually involve data manipulation and calculation, SQL tends to be the way to go.  Most large applications tend to utilize both the ORM and SQL for database access.
