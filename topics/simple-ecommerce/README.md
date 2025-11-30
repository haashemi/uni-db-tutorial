# 📚 Simple E-Commerce

A really simple e-commerce website database. Made of three tables of `users`, `products`, and `orders`.

## Schema

![ERD Schema](/topics/simple-ecommerce/erd.png)

```sql
create table users (
    id              uuid            primary key default uuidv7(),
    full_name       varchar(256)    not null,
    phone_number    varchar(11)     not null,
    password        text            not null,
    home_address    text            not null,
    national_code   varchar(10)                 unique  check (length(national_code) = 10)
);

create table products (
    id              bigint          primary key generated always as identity,
    name            varchar(2048)   not null,
    description     text,
    price           integer         not null    check (price > 0),
    off_percentage  smallint        not null    default 0   check (0 <= off_percentage and off_percentage <= 100),
    created_at      timestamptz     not null    default now()
);

create table orders (
    id              uuid        primary key default uuidv7(),
    user_id         uuid        not null    references users (id),
    product_id      bigint      not null    references products (id),
    final_price     integer     not null    check (final_price >= 0),
    status          smallint    not null    default 0,
    created_at      timestamptz not null    default now()
);
```
