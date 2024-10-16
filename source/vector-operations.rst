###################
 Vector Operations
###################

This page describes the supported vector operations.

**********
 Overview
**********

Basic Vector Functions
======================

-  vector_dims(vector) -> integer
-  vector_dimension_count(vector) -> integer
-  vector_norm(vector) -> double
-  vector_dimension_format(vector) -> INT8 \| FLOAT32 \| FLOAT64
-  ☐ TODO: Discuss vector_dimension_format output

Vector Distance Functions
=========================

-  l2_distance()
-  l2_normalize()
-  l1_distance()
-  cosine_distance()
-  inner_product()

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

-  VECTOR_DIMS and VECTOR_DIMENSION_COUNT are equivalent.
-  It returns the number of dimensions of a vector as a NUMBER.
-  EXPR must evaluate to a vector.
-  If EXPR is NULL, NULL is returned.
-  ☐ TODO: should it return a NUMBER?

.. _vector_dimsvector---integer-1:

VECTOR_NORM (vector) -> double
==============================

.. code:: sql

   VECTOR_NORM ( EXPR )

VECTOR_DIMENSION_FORMAT (vector) -> INT8 | FLOAT32 | FLOAT64
============================================================

.. code:: sql

   VECTOR_DIMENSION_FORMAT ( EXPR )

***************************
 Vector Distance Functions
***************************

-  ☐ TODO: Should the distance functions return float or double?

L2_DISTANCE (vector, vector) -> double
======================================

.. code:: sql

   L2_DISTANCE ( EXPR )

L2_NORMALIZE (vector, vector) -> double
=======================================

.. code:: sql

   L2_NORMALIZE ( EXPR )

L1_DISTANCE (vector, vector) -> double
======================================

.. code:: sql

   L1_DISTANCE ( EXPR )

COSINE_DISTANCE (vector, vector) -> double
==========================================

.. code:: sql

   COSINE_DISTANCE ( EXPR )

INNER_PRODUCT (vector, vector) -> double
========================================

.. code:: sql

   INNER_PRODUCT ( EXPR )
