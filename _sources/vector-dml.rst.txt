############
 Vector DML
############

This page outlines the CRUD operations (Create, Read, Update, Delete)
for vector data in a database.

*****************************************
 Create and Insert a Vector into a Table
*****************************************

To insert vectors into a table, use the `INSERT` statement. Vectors are
represented as arrays within single quotes in SQL.

.. code:: sql

   INSERT INTO items (
     embedding
   )
   VALUES
     ('[1,2,3]'),
     ('[4,5,6]');

-  Here, we are inserting two vectors into the `embedding` column.
-  The vectors must be formatted as strings (`'[x,y,z]'`) where `x`,
   `y`, and `z` are the vector elements.

**************
 Read Vectors
**************

To retrieve vectors from the table, use the `SELECT` statement.

.. code:: sql

   SELECT *
   FROM items
   LIMIT 5;

-  This will fetch the first 5 records from the `items` table.
-  The result will include any vector columns stored in the table.

Read Nearest Neighbor Vectors
=============================

You can retrieve the nearest neighbors of a vector by using vector
distance operators or distance functions.

Using a distance operator:

.. code:: sql

   SELECT * FROM items
   ORDER BY embedding <-> '[3,1,2]'
   LIMIT 5;

-  This query orders the results by the L2 (Euclidean) distance between
   the vector in the `embedding` column and the vector `[3,1,2]`.

Using a distance function:

.. code:: sql

   SELECT name
   FROM items
   ORDER BY l2_distance(embedding, '[3, 1, 2]')
   LIMIT 5;

-  This query uses the `l2_distance` function to calculate the distance
   between vectors and orders by the nearest neighbors.

Both of these queries are equivalent in functionality.

Notes on Nearest Neighbor Search
--------------------------------

-  If no vector index is present, an **exact nearest neighbor** search
   will be performed.
-  If a vector index exists, the search will use the **approximate
   nearest neighbor (ANN)** algorithm, providing faster results.

Note on Multiple Vector Indices
-------------------------------

-  You can create multiple indices for a single vector column, but each
   index must correspond to a different distance operator (e.g., L2
   distance, cosine similarity).

-  A single vector column can only have one index per distance type.

****************************
 Control Vector Index Usage
****************************

You can influence whether the query optimizer uses or bypasses a vector
index by using query hints.

-  ``/*+ VECTOR_INDEX_SCAN (my_col_name) */``: Force the optimizer to
   use a vector index on `my_col_name`.
-  ``/*+ NO_VECTOR_INDEX_SCAN */``: Prevent the optimizer from using any
   vector index.

Force the Usage of a Vector Index
=================================

To explicitly tell the optimizer to use a vector index for a query,
first ensure that an ANN vector index exists on the `embedding` column.

Refer to the `Vector Index` :doc:`vector-index` page for index creation
details.

.. code:: sql

   SELECT /*+ VECTOR_INDEX_SCAN (embedding) */ name
   FROM items
   ORDER BY l2_distance(embedding, '[3, 1, 2]')
   LIMIT 5;

-  In this example, the hint `VECTOR_INDEX_SCAN` forces the use of the
   vector index on the `embedding` column.
-  If no index is available, the hint is silently ignored by the
   optimizer.

Prevent the Usage of a Vector Index
===================================

You can instruct the optimizer to avoid using any vector index by adding
the `NO_VECTOR_INDEX_SCAN` hint.

.. code:: sql

   SELECT /*+ NO_VECTOR_INDEX_SCAN */ name
   FROM items
   ORDER BY l2_distance(embedding, '[3, 1, 2]')
   LIMIT 5;

-  This forces the query to bypass any vector index and perform an exact
   search.

***************
 Update Vector
***************

To update a vector stored in a table, use the `UPDATE` statement.

.. code:: sql

   UPDATE items
   SET embedding = '[1, 2, 3]'
   WHERE id = 1;

-  This updates the `embedding` column for the row where the `id` is 1,
   replacing the old vector with the new vector `[1, 2, 3]`.

***************
 Delete Vector
***************

To delete a vector from the table, use the `DELETE` statement.

.. code:: sql

   DELETE FROM items
   WHERE id = 1;

-  This deletes the row from the `items` table where the `id` is 1,
   including its vector data.
