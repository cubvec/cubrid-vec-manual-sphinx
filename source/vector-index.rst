Vector Index
============

This page outlines the CRUD operations for vector indices in CUBRID
database.

CUBRID only supports HNSW index at the moment.

Create a Vector Index
---------------------

In order to create a vector index, one must specify:

1. the table and the column name where the index will be created onto,
2. the metric function used for distance calculation,
3. the index construction parameters (for HNSW, this will be
   ``ef_construction`` and ``M``).

In order to effectively use the HNSW index, one must specify:

-  the search parameter (for HNSW, this will be ``ef_search``).

BNF
~~~

.. code:: bnf

   <create_vector_index> ::= "CREATE VECTOR INDEX" <index_name>
   "ON" <table_name> "(" <column_name> <metric_function> ")" <with_options>

   <index_name> ::= <identifier>
   <table_name> ::= <identifier>
   <column_name> ::= <identifier>

   <metric_function> ::= <cosine>

   <cosine> ::= "cosine"

   <with_options> ::= "WITH" "(" <option_list> ")"

   <option_list> ::= <option> | <option> "," <option_list>
   <option> ::= <parameter> "=" <value>

   <parameter> ::= "m" | "ef_construction"
   <value> ::= <integer>

   <identifier> ::= [A-Za-z_][A-Za-z_0-9]*
   <integer> ::= [0-9]+

-  ``m`` is the maximum number of connections (edges) each node (vector)
   can have in the graph at each layer of the HNSW structure. It is also
   known as the degree of each node in the graph.
-  ``ef_construction`` controls the size of the dynamic candidate list
   when inserting elements during the index construction phase. It
   defines how many neighbors are checked during the construction of the
   graph when new vectors are added.

Examples
~~~~~~~~

.. code:: sql

   CREATE VECTOR INDEX embedding_hnsw_idx
   ON items (embedding cosine)
   WITH (m = 16, ef_construction = 64);

Inspect Vector Index Options
----------------------------

.. code:: sql

   SELECT embedding_hnsw_idx
   FROM items;

Configure Search parameter
--------------------------

-  ``ef_search`` is the exploration factor during search. It is a
   parameter in the HNSW algorithm that controls the number of nearest
   neighbor candidates to explore during a search query. If ef_search is
   not specified, it is set to the same value as the ``ef_construction``
   of the index by default.

-  ☐ TODO: Discuss the default value of ef_search if not specified.

.. code:: sql

   SELECT /*+ VECTOR_INDEX_SCAN (embedding) */ name
   FROM items
   ORDER BY l2_distance(embedding, '[3, 1, 2]')
   LIMIT 5
   WITH (ef_search = 64);

Update Vector Index
-------------------

Not implemented yet.

Delete Vector Index
-------------------

.. code:: sql

   DROP INDEX embedding_hnsw_idx;

Vector Index Configuration
--------------------------

Memory Usage
~~~~~~~~~~~~

.. code:: toml

   # filename: cubrid.conf
   vector_memory_size=8G
