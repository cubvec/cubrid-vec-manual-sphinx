############
 Vector DDL
############

.. code:: sql

   vector -- Equivalent to vector(2000)

   vector(512) -- 512-dimensional vector of float32

   vector(float64, 128) -- 128-dimensional vector of float64

*****
 BNF
*****

.. code:: bnf

   <declaration> ::= "vector" [ "(" [ <vector_type> "," ] <dim> ")" ]
   <vector_type> ::= "float32" | "binary"
   <dim> ::= <positive_integer>
   <positive_integer> ::= {digit}+

-  ``<dim>`` is a positive integer value

   -  between 1 and 2000 inclusive if and only if ``<vector_type>`` is
      float32.
   -  between 1 and 64000 inclusive if and only if ``<vector_type>`` is
      binary.

**********
 Examples
**********

Create table
============

.. code:: sql

   CREATE TABLE items (
     host_year INT NOT NULL PRIMARY KEY,
     embedding vector(3)
   );

Create column
=============

.. code:: sql

   ALTER TABLE items
   ADD COLUMN embedding vector(3);
