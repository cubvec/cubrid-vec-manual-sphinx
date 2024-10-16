###################
 Vector Operations
###################

This page describes the supported vector operations.

**********
 Overview
**********

Basic Vector Functions
======================

-  ``vector_dims(vector) -> integer``
-  ``vector_dimension_count(vector) -> integer``
-  ``vector_norm(vector) -> double``
-  ``vector_dimension_format(vector) -> INT8 | FLOAT32 | FLOAT64``
-  ☐ TODO: Discuss `vector_dimension_format` output

Vector Distance Functions
=========================

-  ``l2_distance(vector1, vector2) -> double``
-  ``l1_distance(vector1, vector2) -> double``
-  ``cosine_distance(vector1, vector2) -> double``
-  ``inner_product(vector1, vector2) -> double``

******************
 Vector Operators
******************

Basic Vector Operators
======================

-  ``+``: element-wise addition
-  ``-``: element-wise subtraction
-  ``*``: element-wise multiplication
-  ``||``: concatenation

Distance Operators
==================

-  ``<->``: L2 distance
-  ``<#>``: negative inner product
-  ``<=>``: cosine distance
-  ``<+>``: L1 distance

****************
 Specifications
****************

VECTOR_DIMS (vector) -> integer
===============================

VECTOR_DIMENSION_COUNT (vector) -> integer
==========================================

.. code:: sql

   VECTOR_DIMS ( EXPR )
   VECTOR_DIMENSION_COUNT ( EXPR )

-  ``VECTOR_DIMS`` and ``VECTOR_DIMENSION_COUNT`` are equivalent.
-  Returns the number of dimensions of a vector as an INTEGER.
-  ``EXPR`` must evaluate to a vector.
-  If ``EXPR`` is NULL, NULL is returned.
-  ☐ TODO: should it return an INTEGER or a NUMBER?

VECTOR_NORM (vector) -> double
==============================

.. code:: sql

   VECTOR_NORM ( EXPR )

-  Returns the norm (magnitude) of a vector.
-  If ``EXPR`` is NULL, NULL is returned.

VECTOR_DIMENSION_FORMAT (vector) -> INT8 | FLOAT32 | FLOAT64
============================================================

.. code:: sql

   VECTOR_DIMENSION_FORMAT ( EXPR )

-  Returns the data type format of the vector elements: ``INT8``,
   ``FLOAT32``, or ``FLOAT64``.
-  If ``EXPR`` is NULL, NULL is returned.

***************************
 Vector Distance Functions
***************************

-  ☐ TODO: Should the distance functions return float or double?

L2_DISTANCE (vector, vector) -> double
======================================

.. code:: sql

   L2_DISTANCE ( EXPR1, EXPR2 )

-  Returns the L2 (Euclidean) distance between two vectors.
-  Both expressions must evaluate to vectors.
-  If any of the expressions is NULL, NULL is returned.

L1_DISTANCE (vector, vector) -> double
======================================

.. code:: sql

   L1_DISTANCE ( EXPR1, EXPR2 )

-  Returns the L1 (Manhattan) distance between two vectors.
-  Both expressions must evaluate to vectors.
-  If any of the expressions is NULL, NULL is returned.

COSINE_DISTANCE (vector, vector) -> double
==========================================

.. code:: sql

   COSINE_DISTANCE ( EXPR1, EXPR2 )

-  Returns the cosine distance between two vectors.
-  Both expressions must evaluate to vectors.
-  If any of the expressions is NULL, NULL is returned.

INNER_PRODUCT (vector, vector) -> double
========================================

.. code:: sql

   INNER_PRODUCT ( EXPR1, EXPR2 )

-  Returns the inner product (dot product) of two vectors.
-  Both expressions must evaluate to vectors.
-  If any of the expressions is NULL, NULL is returned.
