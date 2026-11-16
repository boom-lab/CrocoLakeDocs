.. CrocoLake documentation master file, created by
   sphinx-quickstart on Tue Mar  4 15:17:22 2025.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

CrocoLake documentation
=======================

CrocoLake is a regularly refreshed and analysis-ready collection of global physical and biogeochemical oceanographic datasets. It is designed for fast, cloud-optimized access to sparse in-situ ocean observations from a growing number of ocean observing networks and initiatives (Argo, GLODAP, Spray gliders, Oleander XBT, SPOTS, etc.). The dataset is in a uniform tabular structure, with homogenized units and quality-control flags. It is stored in Apache Parquet format across multiple partitions for efficient subsetting. It can be accessed with any language that supports Parquet (e.g. Python, MATLAB, Julia). It also preserves profile information where the underlying datasets provide it.

CrocoLake is supported by the National Science Foundation under Grant No. 2311383, “Collaborative Research: Frameworks: A community platform for accelerating observationally-constrained regional oceanographic modeling”.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   crocolake
   available_datasets
   tools
   people
