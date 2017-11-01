---
title: Importing Data to Your Cluster
summary: Learn how to import data from pg_dump and CSV files into your CockroachDB cluster.
toc: false
sidebar_data: sidebar-data-training.json
---

<div id="toc"></div>

## Presentation

<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRg_445dWS0Ag1Ta3jdMdyfyOIvpP72U0W3XklF8ScJmUlLkdezZUy7JK1jca3A5fWoZiEpq8iu_OMd/embed?start=false&loop=false" frameborder="0" width="756" height="454" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

#### URL for comments

[Importing Data to Your Cluster](https://docs.google.com/presentation/d/13yXxi-7AktFMzq6UlteD7CejTdQE-HLWKXKyUqeebgM/)

## Lab

In this lab, we'll download a dump from PostgreSQL (generated with `pg_dump`), reformat it, and then import it into CockroachDB.

### Before You Begin

To complete this lab, you need a [local cluster of 3 nodes](3-node-local-secure-cluster.html).

### Step 1. Download Postgres dump

Download our prepared [PostgreSQL dump file](resources/pg_dump.sql).

### Step 2. Clean up the dump file

1. Review and understand the schema from `pg_dump.sql`. It contains 2 tables, `customers` and `accounts`, as well as some constraints on both tables.
2. Rewrite the two [`CREATE TABLE`](../stable/create-table.html) statements to contain all of the constraints identified in the file, including each table's [`PRIMARY KEY`](../stable/primary-key.html#syntax).
  This has to be done manually because PostgreSQL attempts to add the primary key after creating the table, but CockroachDB requires the primary key be defined upon table creation.
3. Remove all statements from the file besides the `CREATE TABLE` and `COPY` statements.

### Step 3. Import the dump

~~~ shell
$ psql -p [port] -h [node host] -d [database] -U [user] < pg_dump.sql
~~~

For reference, CockroachDB uses these defaults:

- `[port]`: **26257**
- `[user]`: **root**

## Up Next

- [Identity and Access Management](identity-and-access-management.html)
