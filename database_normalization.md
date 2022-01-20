---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# Database Normalization

siriuskoan

---

# Outline

- Introduction
- 1NF
- 2NF
- 3NF
- Final Words

---

# Introduction

Database Normalization is a series of processes to reduce data redundancy and improve data integrity.

The processes are called Normal Forms (NF).

By implementing the processes, we can develop functions related to database easier.

<!--

We have talked about PK and FK, but we do not show their advantages.

There are 6 process to normalize database, but we will only talk about the first 3.

-->

---

# Introduction

There is a poorly designed table, we can make it better by 1NF, 2NF and 3NF.

The columns are
- `customer_name`
- `customer_gender`
- `store_name`
- `store_address`
- `items`
- `items_price`
- `total`

---

# Introduction

Some data in it can be

<table>
<thead>
<tr>
<th>customer_name</th>
<th>customer_gender</th>
<th>store_name</th>
<th>store_address</th>
<th>items</th>
<th>how_many</th>
<th>items_price</th>
<th>total</th>
</tr>
</thead>
<tbody>
<tr>
<td>Peter</td>
<td>Male</td>
<td>Good Store</td>
<td>ABC street</td>
<td>apple,banana</td>
<td>1,1</td>
<td>10,20</td>
<td>30</td>
</tr>
<tr>
<td>Amy</td>
<td>Female</td>
<td>Good Store</td>
<td>ABC street</td>
<td>apple,banana</td>
<td>1,1></td>
<td>10,20</td>
<td>210</td>
</tr>
<tr>
<td>Cindy</td>
<td>Female</td>
<td>Best Store</td>
<td>XYZ street</td>
<td>juice</td>
<td>2</td>
<td>50</td>
<td>100</td>
</tr>
</tbody>
</table>

---

# 1NF

The schema has many problems.
- In `items`, `how_many` and `items_price` columns, there can be more than one record.
- No primary key to distinguish the differences between records.

<!--

For the first one, it's hard to parse the content.

Actually, we should remove duplicate columns (data in different forms) in 1NF, but I think it is intuitive.

-->

---

# 1NF

To accomplish 1NF, the following requirements must be satisfied
- One value per field
- PK

<!--

`juice,ice cream` is not allowed.

-->

---

# 1NF

After doing 1NF, the database become


<table>
<thead>
<tr>
<th><span>id</span></th>
<th>customer_name</th>
<th>customer_gender</th>
<th>store_name</th>
<th>store_address</th>
<th>items</th>
<th>how_many</th>
<th>items_price</th>
<th>total</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>Peter</td>
<td>Male</td>
<td>Good Store</td>
<td>ABC street</td>
<td>apple</td>
<td>1</td>
<td>10</td>
<td>10</td>
</tr>
<tr>
<td>2</td>
<td>Peter</td>
<td>Male</td>
<td>Good Store</td>
<td>ABC street</td>
<td>banana</td>
<td>1</td>
<td>20</td>
<td>20</td>
</tr>
<tr>
<td>3</td>
<td>Amy</td>
<td>Female</td>
<td>Good Store</td>
<td>ABC street</td>
<td>apple</td>
<td>1</td>
<td>10</td>
<td>10</td>
</tr>
<tr>
<td>4</td>
<td>Amy</td>
<td>Female</td>
<td>Good Store</td>
<td>ABC street</td>
<td>banana</td>
<td>10</td>
<td>20</td>
<td>200</td>
</tr>
<tr>
<td>5</td>
<td>Cindy</td>
<td>Female</td>
<td>Best Store</td>
<td>XYZ street</td>
<td>juice</td>
<td>2</td>
<td>50</td>
<td>100</td>
</tr>
</tbody>
</table>

---

# 2NF

However, the database after 1NF has one big problem - the data of customers, stores and items are duplicated.

Therefore, in 2NF, we want to solve this by separating the big table into many small tables and use foreign key to connect them.

---

# 2NF

`customers`
<table>
<thead>
<th>id</th>
<th>name</th>
<th>gender</th>
</thead>
<tbody>
<tr>
<td>1</td>
<td>Peter</td>
<td>Male</td>
</tr>
<tr>
<td>2</td>
<td>Amy</td>
<td>Female</td>
</tr>
<tr>
<td>3</td>
<td>Cindy</td>
<td>Female</td>
</tr>
</tbody>
</table>

---

# 2NF

`stores`
<table>
<thead>
<th>id</th>
<th>name</th>
<th>address</th>
</thead>
<tbody>
<tr>
<td>1</td>
<td>Good Store</td>
<td>ABC street</td>
</tr>
<tr>
<td>2</td>
<td>Best Store</td>
<td>XYZ street</td>
</tr>
</tbody>
</table>

`items`

<table>
<thead>
<th>id</th>
<th>name</th>
<th>price</th>
</thead>
<tbody>
<tr>
<td>1</td>
<td>apple</td>
<td>10</td>
</tr>
<tr>
<td>2</td>
<td>banana</td>
<td>20</td>
</tr>
<tr>
<td>3</td>
<td>juice</td>
<td>50</td>
</tr>
</tbody>
</table>

---

# 2NF

`orders`
<table>
<thead>
<th>id</th>
<th>customer_id</th>
<th>store_id</th>
<th>item_id</th>
<th>how_many</th>
<th>total</th>
</thead>
<tbody>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>10</td>
</tr>
<tr>
<td>2</td>
<td>1</td>
<td>1</td>
<td>2</td>
<td>1</td>
<td>20</td>
</tr>
<tr>
<td>3</td>
<td>2</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>10</td>
</tr>
<tr>
<td>4</td>
<td>2</td>
<td>1</td>
<td>2</td>
<td>10</td>
<td>200</td>
</tr>
<tr>
<td>5</td>
<td>3</td>
<td>2</td>
<td>3</td>
<td>1</td>
<td>20</td>
</tr>
</tbody>
</table>

<!--

FK is `customer_id`, `store_id` and `item_id`.

-->

---

# 3NF

It is almost done.

However, there is another problem - the columns are not independent to each other.

`total` column depends on `item_id` and `how_many` columns, we consider it inappropriate.

Therefore, in 3NF, we will remove the `total` column.

---

# Final Words

Briefly speaking,
- 1NF - Don't store many values in one field and use PK to distinguish every row.
- 2NF - Use more tables to solve duplicate problem, and use FK to connect them.
- 3NF - Make columns independent to each other.

4, 5 and 6NF are not used in practical since it brings busier IO.