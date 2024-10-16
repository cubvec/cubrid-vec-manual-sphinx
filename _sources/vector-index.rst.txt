##############
 Vector Index
##############

This page outlines the CRUD operations for vector indices in the CUBRID
database.

Currently, CUBRID only supports the **Hierarchical Navigable Small World
(HNSW)** index for vector data.

***********************
 Create a Vector Index
***********************

To create a vector index, you must specify the following:

#. The table and column name where the index will be created.

#. The metric function used for distance calculation (e.g., cosine
   distance).

#. The index construction parameters:

   -  ``ef_construction``: Controls the number of neighbors checked
      during index construction.
   -  ``M``: Determines the maximum number of edges (connections) per
      node in the graph.

**Search Parameter:** When querying with the HNSW index, you must
specify:

-  ``ef_search``: This controls the number of nearest neighbor
   candidates to explore during a query.

BNF
===

The formal grammar for creating a vector index in CUBRID is as follows:

.. code:: bnf

   <create_vector_index> ::= "CREATE VECTOR INDEX" <index_name>
   "ON" <table_name> "(" <column_name> <metric_function> ")" <with_options>

   <index_name> ::= <identifier>
   <table_name> ::= <identifier>
   <column_name> ::= <identifier>

   <metric_function> ::= <cosine> | <l2> | <inner_product>

   <cosine> ::= "cosine"
   <l2> ::= "l2"
   <inner_product> ::= "inner_product"

   <with_options> ::= "WITH" "(" <option_list> ")"

   <option_list> ::= <option> | <option> "," <option_list>
   <option> ::= <parameter> "=" <value>

   <parameter> ::= "M" | "ef_construction"
   <value> ::= <integer>

   <identifier> ::= [A-Za-z_][A-Za-z_0-9]*
   <integer> ::= [0-9]+

Parameter Descriptions
======================

-  **M**: This is the maximum number of connections (edges) each node
   (vector) can have in the graph at each layer of the HNSW structure.
   This is also known as the *degree* of each node in the graph. A
   higher value of `M` increases recall accuracy but also increases
   memory usage and construction time.

-  **ef_construction**: This controls the size of the dynamic candidate
   list during the index construction phase. It defines how many
   neighbors are checked when inserting elements into the graph. A
   higher value results in more accurate indices but longer construction
   time.

Example
=======

To create an HNSW index with cosine similarity:

.. code:: sql

   CREATE VECTOR INDEX embedding_hnsw_idx
   ON items (embedding cosine)
   WITH (M = 16, ef_construction = 64);

-  This creates a vector index on the `embedding` column of the `items`
   table, using cosine similarity with `M` set to 16 and
   `ef_construction` set to 64.

******************************
 Inspect Vector Index Options
******************************

You can inspect the index configuration by selecting the index from the
table.

.. code:: sql

   SELECT embedding_hnsw_idx
   FROM items;

This query retrieves the details of the `embedding_hnsw_idx` index.

****************************
 Configure Search Parameter
****************************

The `ef_search` parameter is used during search queries to control the
number of nearest neighbor candidates that are explored.

-  **ef_search**: This is the exploration factor during a search. It
   determines how many candidate nodes the algorithm should explore when
   searching for the nearest neighbors. A higher value for `ef_search`
   typically improves recall accuracy but at the cost of slower query
   times.

-  By default, if `ef_search` is not specified, it is set to the same
   value as the `ef_construction` used during index creation.

Example
=======

To configure the `ef_search` parameter in a query:

.. code:: sql

   SELECT /*+ VECTOR_INDEX_SCAN (embedding) */ name
   FROM items
   ORDER BY l2_distance(embedding, '[3, 1, 2]')
   LIMIT 5
   WITH (ef_search = 64);

-  In this query, the vector index is forced using the
   `VECTOR_INDEX_SCAN` hint, and the `ef_search` value is set to 64 to
   control the search accuracy.

*********************
 Update Vector Index
*********************

At the moment, updating vector indices is not supported in CUBRID. You
must recreate the index if any parameters need to be changed.

*********************
 Delete Vector Index
*********************

To delete a vector index, use the `DROP INDEX` command.

.. code:: sql

   DROP INDEX embedding_hnsw_idx;

-  This command deletes the `embedding_hnsw_idx` index from the `items`
   table.

****************************
 Vector Index Configuration
****************************

Memory Usage
============

The memory usage for vector indices can be configured in the
`cubrid.conf` configuration file. The parameter `vector_memory_size`
sets the memory allocated for vector index operations.

.. code:: toml

   # filename: cubrid.conf
   vector_memory_size = 8G

-  This example sets the memory size allocated for vector indices to 8
   GB.

********************************
 Additional Configuration Notes
********************************

-  For large datasets, ensure that `vector_memory_size` is sufficiently
   high to avoid memory overflow during index creation or query
   processing.

-  Tuning `M`, `ef_construction`, and `ef_search` allows you to balance
   between search accuracy, speed, and memory usage, depending on your
   application requirements.
