Vector DML
==========

This page outlines the CRUD operations (Create, Read, Update, Delete)
for vectors in a database.

Create and Insert a Vector into a Table
---------------------------------------

To insert vectors into a table:

.. code:: sql

   INSERT INTO items (
     embedding
   )
   VALUES
     ('[1,2,3]'),
     ('[4,5,6]');

Read Vectors
------------

To retrieve vectors from the table:

.. code:: sql

   SELECT *
   FROM items
   LIMIT 5;

Read Nearest Neighbor Vectors
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can retrieve the nearest neighbors using either vector distance
operators or functions.

Using a distance operator:

.. code:: sql

   SELECT * FROM items
   ORDER BY embedding <-> '[3,1,2]'
   LIMIT 5;

Using a distance function:

.. code:: sql

   SELECT name FROM items
   ORDER BY l2_distance(embedding, '[3, 1, 2]')
   LIMIT 5;

Both of the above queries are equivalent.

-  If no vector index is present, the “exact nearest neighbor” algorithm
   is applied.
-  If a vector index exists, it will be used automatically.

Note on Multiple Vector Indices
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can create multiple indices for a single vector column, but each
index must correspond to a different type of distance operator. A single
vector column can only have one index for each distance type.

Control Vector Index Usage
--------------------------

-  ``/*+ VECTOR_INDEX_SCAN (my_col_name) */``: force vector index
-  ``/*+ NO_VECTOR_INDEX_SCAN */``: force prevent vector index

Force the usage of vector index, if present
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First, you need to create an ANN vector index on ‘embedding’ column of
table ‘items’.

Refer to `Vector Index <./vector-index.md>`__ page.

You can place a hint to force the optimizer to use the vector index if
present. If no index is found, the optimizer silently ignores the hint.

.. code:: sql

   SELECT /*+ VECTOR_INDEX_SCAN (my_col_name) */ name
   FROM items
   ORDER BY l2_distance(embedding, '[3, 1, 2]')
   LIMIT 5;

Abort the usage of vector index
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   SELECT /*+ NO_VECTOR_INDEX_SCAN */ name
   FROM items
   ORDER BY l2_distance(embedding, '[3, 1, 2]')
   LIMIT 5;

Update vector
-------------

.. code:: sql

   UPDATE items
   SET embedding = '[1, 2, 3]'
   WHERE id = 1;

Delete vector
-------------

.. code:: sql

   DELETE FROM items
   WHERE id = 1;
