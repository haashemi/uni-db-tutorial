# 🗃️ CRUD Basics

In this tutorial we're going to learn how to work with databases _practically_. We'll start with creating a database, creating the tables, and perform some basic CRUD operations.

This tutorial assumes that you already home common knowledge on what a database is, what relational-database is, and how to design a database. As today's tutorial is more practical rather than theoretical.

## Choose a Topic

First of all, we need a topic to design and implement our database for. Here are a two basic topics to choose from:

- [Simple Library Management](/topics/simple-library-management)
- [Simple E-Commerce](/topics/simple-ecommerce)

## Choose a DBMS

Now that we know the topic and have designed its schema, we need to choose a DBMS to implement our database schema in. There are so many great DBMSs for different use-cases and project scales, such as:

- [SQLite](https://sqlite.org)
- [PostgreSQL](https://postgresql.org)
- [MySQL](https://mysql.com) or [MariaDB](https://mariadb.org)
- [MS SQL Server](https://microsoft.com/en-us/sql-server)

For this tutorial I chose `PostgreSQL` as it's being widely used in large scale projects and does a great job on providing the best possible developer experience. Though as this tutorial just covers the basics, any DBMS works just fine.

- [Download Postgres](https://postgresql.org/download)
- [Postgres Arch Wiki](https://wiki.archlinux.org/title/PostgreSQL)

## Quick Note

Now that we have everything to get started, there's one little piece that's worth mentioning.

- **Documentation is your best friend.**

Thus, in this tutorial, we will take a quick look at PostgreSQL's documentation and learn how we could take the most advantage out of it.

- [Official Documentation](https://postgresql.org/docs)

## Create the Database

Creating a new database is the easiest part in almost any DBMS if you have a proper setup.

It's as simple as executing as this:

```sql
create database my_project
    with owner = ali;
```

- [Reference](https://www.postgresql.org/docs/18/sql-createdatabase.html)

## Create the tables

Creating the tables are a bit tricky depending on your schema as there are multiple things you have to consider. Such as:

1. [Data Types](https://www.postgresql.org/docs/current/datatype.html)
   - [Numerics](https://www.postgresql.org/docs/current/datatype-numeric.html)
   - [Characters](https://www.postgresql.org/docs/current/datatype-character.html)
   - [Dates/Times](https://www.postgresql.org/docs/current/datatype-datetime.html)
   - [Booleans](https://www.postgresql.org/docs/current/datatype-boolean.html)
2. [Identity columns](https://postgresql.org/docs/current/ddl-identity-columns.html)
3. [Constraints](https://www.postgresql.org/docs/current/ddl-constraints.html)
4. [Indexes](https://www.postgresql.org/docs/current/indexes.html)

But here's how it usually look like:

```sql
create table products (
    id          bigint          primary key generated always as identity,
    name        varchar(512)    not null,
    category_id bigint          not null    references categories (id),
    description text,
    price       int             not null    check (price >= 10000)
    created_at  timestamptz     not null    default now()
)
```

- [Reference](https://www.postgresql.org/docs/18/sql-createtable.html)

## CRUD

The term **CRUD** stands for **Create**, **Read**, **Update**, and **Delete**, Which in SQL terms they'll translate to:

| Term   | SQL    |
| ------ | ------ |
| Create | INSERT |
| Read   | SELECT |
| Update | UPDATE |
| Delete | DELETE |

### `INSERT`

Insert queries are used to _create_ a record inside a _table_.

```sql
insert into products (name, description)
    values ('BMW M5 Competition (2024)', 'You know what exactly this is');
```

- [Reference](https://www.postgresql.org/docs/current/sql-insert.html)

### `SELECT`

Select queries are used to read some data from one or multiple tables at the same time that have some optional conditions in a specific order.

```sql
select
    id,
    name,
    description
from
    products
order by
    name;
```

- [Reference](https://www.postgresql.org/docs/current/sql-select.html)

### `UPDATE`

Update queries are used to update the data of one or multiple records of a table that have a specific condition.

```sql
update products
set
    off_percentage = 10
where
    created_at >= (now() - '7 days'::interval);
```

- [Datetime Operators](https://www.postgresql.org/docs/current/functions-datetime.html)
- [Reference](https://www.postgresql.org/docs/current/sql-update.html)

### `DELETE`

Delete queries are used to delete one or multiple records from a table table have a specific condition.

```sql
delete from products
where id = 96329;
```

- [Reference](https://www.postgresql.org/docs/current/sql-delete.html)
