
creating a table:

```postgresql
create table emids.account(
	id smallint primary key,
	name varchar unique
)

create table emids.transactions(
	id smallint primary key,
	transaction_number int,
	amount int,
	account_id smallint references emids.account(id),
	type varchar,
	created_at date
	
)
```

inserting values:

```postgresql
insert into emids.account(id, name)
values(1,'neha'), (2, 'ankit'), (3, 'raman'), (4, 'sunil')

insert into emids.transactions
values(1, 'abc1', 1000, 1, 'deposit', '2023-01-01'),
(2, 'abc', 1500, 2, 'deposit', '2023-10-01'),
(3, 'xyz', 100, 2, 'withdraw', '2024-01-01'),
(4, 'abc2', 1200, 1, 'withdraw', '2024-02-01'),
(5, 'def', 200, 3, 'deposit', '2024-03-01')
```

altering columns:

```postgresql
alter table emids.transactions
alter column transaction_number type varchar
```

