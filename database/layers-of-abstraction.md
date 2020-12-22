## Low level: Database driver

This is basically as low-level as you can get — short of manually generating TCP packets and delivering them to the database. A database driver is going to handle connecting to a database (and sometimes connection pooling). At this level you’re going to be writing raw SQL strings and delivering them to a database, and receiving a response from the database. In the Node.js ecosystem there are many libraries operating at this layer. Here are three popular libraries:

- [mysql](https://github.com/mysqljs/mysql): MySQL (13k stars / 330k weekly downloads)
- [pg](https://github.com/brianc/node-postgres): PostgreSQL (6k stars / 520k weekly downloads)
- [sqlite3](https://github.com/mapbox/node-sqlite3): SQLite (3k stars / 120k weekly downloads)

Each of these libraries essentially works the same way: take the database credentials, instantiate a new database instance, connect to the database, and send it queries in the form of a string and asynchronously handle the result.

https://blog.logrocket.com/why-you-should-avoid-orms-with-examples-in-node-js-e0baab73fa5/
