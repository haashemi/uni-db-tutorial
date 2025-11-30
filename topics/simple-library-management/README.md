# 📚 Simple Library Management

A really simple library management system database. Made of three tables of `users`, `books`, and `borrowed_books`.

## Schema

![ERD Schema](/topics/simple-library-management/erd.png)

```sql
create table users (
    id              bigint          primary key generated always as identity,
    full_name       varchar(512)    not null,
    phone_number    varchar(11)     not null,
    home_address    text            not null,
    national_code   varchar(10)     not null    unique  check (length(national_code) = 10),
    created_at      timestamptz     not null    default now()
);

create table books (
    id              bigint          primary key generated always as identity,
    title           varchar(1024)   not null,
    author          varchar(512)    not null,
    publisher       varchar(512)    not null,
    category        varchar(128)    not null,
    row_number      smallint        not null
);

create table borrowed_books (
    id          bigint      primary key generated always as identity,
    user_id     bigint      not null    references users (id),
    book_id     bigint      not null    references books (id),
    notes       text,
    returned_at timestamptz             default now(),
    borrowed_at timestamptz not null    default now()
);
```
