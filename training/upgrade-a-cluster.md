---
title: Upgrade a Cluster
summary: Learn how to upgrade a CockroachDB cluster to a new version.
toc: false
sidebar_data: sidebar-data-training.json
---

<div id="toc"></div>

## Presentation

<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vQFwUiv1205icOGxTN4OlMuMYSGjbx9Co3Ggx2mRsI9F9-pEUsvwkaJjOXb92ws1oOG-OY0j-C43G-j/embed?start=false&loop=false" frameborder="0" width="756" height="454" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

#### URL for comments

[Upgrade Your Custer](https://docs.google.com/presentation/d/1V_Y-V5EjUXNnw7RVgpdZx5Z0_JYP4IqYgkeklvqV5EA)

## Lab

In this lab, we'll upgrade your cluster to use the latest dev release.

### Before You Begin

To complete this lab, you need a [local cluster of 3 nodes](3-node-local-secure-cluster.html).

### Step 1. Install the dev binary

[Download and install the latest dev binary](../dev/install-cockroachdb.html).

You can overwrite the existing `cockroach` binary with this new version.

### Step 2. Perform the rolling upgrade

1. Stop node 1 the `cockroach` process:

    {% include copy-clipboard.html %}
    ~~~ shell
    $ cockroach quit --certs-dir=certs --port=26257
    ~~~

    Then verify that the process has stopped:

    {% include copy-clipboard.html %}
    ~~~ shell
    $ cockroach node status --certs-dir=certs --port=26258
    ~~~

2. Restart the node (which will use the new binary and update the version of the node):

    {% include copy-clipboard.html %}
    ~~~ shell
    $ cockroach start --join=localhost:26257,localhost:26258,localhost:26259
    ~~~

3. Verify the node has rejoined the cluster:

    {% include copy-clipboard.html %}
    ~~~ shell
    $ cockroach node status --certs-dir=certs
    ~~~

4. Wait at least one minute after the node has rejoined the cluster, and then repeat these steps for the next node. Note that you will need to change the values of both `--port` flags in step 1 to quit a node, and then perform `cockroach node status` on another node that is still up.

## Up Next

- [Decommission Nodes](decommission-nodes.html)
