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

vector_dims(vector) -> integer
==============================

.. code:: bnf

   VECTOR_DIMS ( EXPR )
   VECTOR_DIMENSION_COUNT ( EXPR )

-  It returns the number of dimensions of a vector as a NUMBER.
-  EXPR must evaluate to a vector.
-  If EXPR is NULL, NULL is returned.
-  ☐ TODO: should it return a NUMBER?

.. _vector_dimsvector---integer-1:

vector_dims(vector) -> integer
==============================

vector_dimension_count(vector) -> integer
=========================================

vector_norm(vector) -> double
=============================

vector_dimension_format(vector) -> INT8 | FLOAT32 | FLOAT64
===========================================================
