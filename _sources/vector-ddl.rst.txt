############
 Vector DDL
############

This section describes the Data Definition Language (DDL) for vector
types.

.. code:: sql

   vector -- Equivalent to vector(2000)

   vector(512) -- 512-dimensional vector of float32

   vector(float64, 128) -- 128-dimensional vector of float64

*****
 BNF
*****

.. code:: bnf

   <declaration> ::= "vector" [ "(" [ <vector_type> "," ] <dim> ")" ]
   <vector_type> ::= "float32" | "float64" | "binary"
   <dim> ::= <positive_integer>
   <positive_integer> ::= {digit}+

-  ``<dim>`` is a positive integer value:

   -  between 1 and 2000 inclusive if and only if ``<vector_type>`` is
      ``float32``.
   -  between 1 and 1000 inclusive if and only if ``<vector_type>`` is
      ``float64``.
   -  between 1 and 64000 inclusive if and only if ``<vector_type`` is
      ``binary``.

Vector Type Specifications
==========================

-  The ``vector`` data type is a fixed-size multidimensional array that
   can store either floating-point numbers or binary values.
-  Default vector type is ``float32`` if no type is specified.
-  Dimensions are specified in parentheses after the type.

Dimension Constraints
---------------------

-  For vectors of type ``float32``, dimensions must be between 1 and
   2000.
-  For vectors of type ``float64``, dimensions must be between 1 and
   1000.
-  For binary vectors, dimensions must be between 1 and 64000.

Examples of Declaration
=======================

#. **Default Vector (float32)**

   .. code:: sql

      vector -- Equivalent to vector(2000)

#. **512-dimensional Vector (float32)**

   .. code:: sql

      vector(512)

#. **128-dimensional Vector (float64)**

   .. code:: sql

      vector(float64, 128)

**********
 Examples
**********

Create table with vector column
===============================

.. code:: sql

   CREATE TABLE items (
     host_year INT NOT NULL PRIMARY KEY,
     embedding vector(3)
   );

This example creates a table named `items` with a primary key
`host_year` and an `embedding` column of a 3-dimensional vector.

Alter table to add vector column
================================

.. code:: sql

   ALTER TABLE items
   ADD COLUMN embedding vector(3);

This example alters the existing `items` table by adding a new column
`embedding` of a 3-dimensional vector.

Additional Example: Binary Vector
=================================

.. code:: sql

   CREATE TABLE binary_items (
     item_id INT NOT NULL PRIMARY KEY,
     binary_embedding vector(binary, 512)
   );

This example creates a table with a binary vector of 512 dimensions.
