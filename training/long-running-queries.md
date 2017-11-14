---
title: Long Running Queries
summary: 
toc: false
sidebar_data: sidebar-data-training.json
---

<div id="toc"></div>

## Presentation

<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRTFcuRZXD__ddiZsGUIbHS4hM7Oqxu0muKt5OCJziJpB39ciLHL3kjcnnuJK7Joix5pNgak5kgv4kD/embed?start=false&loop=false" frameborder="0" width="756" height="454" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

#### URL for comments

[Long-Running Queries](https://docs.google.com/presentation/d/1ymrcgdemvtYxMaIAS5o-9li4BO5ox7gERrwZnBxjraw/)

## Lab

In this lab, we'll check out the page in the Admin UI that also shows us long-running queries.

### Before You Begin

To complete this lab, you need:

- A [secure local cluster of 3 nodes](3-node-local-secure-cluster).
- Installed the CockroachDB version of YCSB:

    <div class="filters clearfix">
      <button style="width: 15%" class="filter-button" data-scope="mac">Mac</button>
      <button style="width: 15%" class="filter-button" data-scope="linux">Linux</button>
    </div>
    <p></p>

    <div class="filter-content" markdown="1" data-scope="mac">
    {% include copy-clipboard.html %}
    ~~~ shell
    # Get the CockroachDB tarball:
    $ curl -O {{site.url}}/docs/training/resources/crdb-ycsb-mac.tar.gz
    ~~~

    {% include copy-clipboard.html %}
    ~~~ shell
    # Extract the binary:
    $ tar xfz crdb-ycsb-mac.tar.gz
    ~~~
    </div>

    <div class="filter-content" markdown="1" data-scope="linux">
    {% include copy-clipboard.html %}
    ~~~ shell
    # Get the CockroachDB tarball:
    $ wget {{page.url}}/training/resources/crdb-ycsb-linux.tar.gz
    ~~~

    {% include copy-clipboard.html %}
    ~~~ shell
    # Extract the binary:
    $ tar xfz crdb-ycsb-linux.tar.gz
    ~~~
    </div>

### Step 1. Start YCSB

{% include copy-clipboard.html %}
~~~ shell
$ ./ycsb -duration 5m -tolerate-errors -concurrency 2 -rate-limit 100 'postgres://root@localhost:26257/?sslmode=verify-ca&sslrootcert=certs/ca.crt&sslcert=certs/root.crt&sslkey=certs/root.key'
~~~

{{site.data.alerts.callout_info}}This is slightly different than when we ran YCSB previously because we're connecting directly to a node instead of a load balancer. In this scenario, if the node we connected to went down, our application could not connect to the database.{{site.data.alerts.end}}

### Step 2. Find running queries in the CLI

1. Launch the built-in SQL client:

    ~~~ sql
    cockroach sql --certs-dir=certs
    ~~~

2. Find the long-running queries:

    ~~~ sql
    > SELECT query_id, query, start FROM [SHOW CLUSTER QUERIES];
    ~~~

    You can find long-running queries by filtering on the `start` column, e.g.:

    ~~~ sql
    > SELECT query_id, query, start FROM [SHOW CLUSTER QUERIES]
          WHERE start < (now() - INTERVAL '1 hour');
    ~~~

### Step 3. Find Long-Running Queries in the Admin UI

1. Access the Admin UI at `http://localhost:8080`

2. Find your long-running queries from **Dashboards** > **Slow Running Queries**.

If your cluster had any long-running queries still running, this is where they would display.

## Up Next

- [Third-Party Monitoring & Alerts](monitoring.html)
