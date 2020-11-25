# Database

Parse Server let you use [MongoDB](https://www.mongodb.org/) or [Postgres](https://www.postgresql.org/) as a database.

The prefered database is MongoDB but Postgres is a great option if you're starting a new project and you expect to have a stable Schema.

## MongoDB

If you have not used MongoDB before, we highly recommend familiarizing yourself with it first before proceeding.

The Mongo requirements for Parse Server are:

* MongoDB version 3.6
* An SSL connection is recommended (but not required).

When using MongoDB with your Parse app, you need to manage your indexes yourself. You will also need to size up your database as your data grows.

If you are planning to run MongoDB on your own infrastructure, we highly recommend using the [RocksDB Storage Engine](#using-mongodb--rocksdb).

In order to allow for better scaling of your data layer, it is possible to direct queries to a mongodb secondary for read operations.  See: [Mongo Read Preference](#using-mongodb-read-preference).

## Postgres

The Postgres requirements for Parse Server are:

* Postgres version 9.5
* PostGIS extensions 2.3

The postgres database adapter will be automatically loaded when you pass a valid postgres URL, for example: `postgres://localhost:5432`. The available configuration options through the URL are: 

```
postgres://localhost:5432/db?ssl=boolean&rejectUnauthorized=boolean&ca=/path/to/file&pfx=/path/to/file&cert=/path/to/file&key=/path/to/file&passphrase=string&secureOptions=number&client_encoding=string&application_name=string&fallback_application_name=string&max=number&query_timeout=idleTimeoutMillis=number&poolSize=number&binary=boolean&keepAlive=boolean
``` 

When using Postgres with your Parse app, you need to manage your indexes yourself.

Details about the configuration options can be found on [pg-promise](https://github.com/vitaly-t/pg-promise/wiki/Connection-Syntax). Some useful combinations are below:

* SSL with verification - `postgres://localhost:5432/db?ca=/path/to/file` 
* SSL with no verification - `postgres://localhost:5432/db?ssl=true&rejectUnauthorized=false`

### Caveats

* You will need to configure a [file adapter](#configuring-file-adapters) in order to store files.
* Join tables are resolved in memory, there is no performance improvements using Postgres over MongoDB for relations or pointers.
* Mutating the schema implies running ALTER TABLE, therefore we recommend you setup your schema when your tables are not full.
* The postgres URL for 4.2.0 and below only supports the following configuration options:

    ```
    postgres://localhost:5432/db?ssl=boolean&client_encoding=string&application_name=string&fallback_application_name=string&poolSize=number&binary=boolean&keepAlive=boolean
    ```
