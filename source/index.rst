.. CrocoLake documentation master file, created by
   sphinx-quickstart on Tue Mar  4 15:17:22 2025.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

CrocoLake documentation
=======================

CrocoLake is a database of oceanographic observations that is developed and maintained within the framework of the NSF-sponsored project CROCODILE (CESM Regional Ocean and Carbon cOnfigurator with Data assimilation and Embedding).

Its strength is to offer a uniform interface to a suite of oceanographic observations, which are all maintained in the same format (parquet), offering the user with the opportunity to retrieve and manipulate observations while remaining agnostic of their original data format, thus avoiding the extra-learning required to deal with multiple formats (netCDF, CSV, parquet, etc.) and merging different sources.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   crocolake
   available_datasets
   tools
