CrocoLake
=========

CrocoLake contains observations from different databases, converted and stored in a uniform schema in parquet format. This documentation describes both the :ref:`database schema <crocolake_conventions>` and :ref:`how each dataset is brought into CrocoLake <available_datasets>`.

CrocoLake comes in two versions:

* PHY: contains physical measurements (temperature, salinity, pressure);
* BGC: contains physical and biogeochemical measurements (e.g. temperature, dissolved oxygent, chlorophyll, etc.).

Download links
--------------

CrocoLake is refreshed every weekend. You can download the most recent versions at these links:

* `CrocoLake-PHY <https://whoi-my.sharepoint.com/:u:/g/personal/enrico_milanese_whoi_edu/IQDq9IvhvTxhRKkBRJXePgv7Aa4_HFGUxoIFiyE17wy3pzA?e=RaEVc7&download=1>`_ (approx. 19 GB)
* `CrocoLake-BGC <https://whoi-my.sharepoint.com/:u:/g/personal/enrico_milanese_whoi_edu/IQC2iSTeq9BBS64act2e6XmFAVq97kMYNDEs1QQWZ5yYEHo?e=MngvtE&download=1>`_ (approx. 7 GB)

Some softwares (e.g. earlier versions of Matlab) cannot parse all the storage-metadata at the links above and fail; if you encounter that issue, try downloading these:
* `CrocoLake-PHY <https://whoi-my.sharepoint.com/:u:/g/personal/enrico_milanese_whoi_edu/IQBevTxDjDisT4ALvPp4RgF2Adog8oRwfGtL15e1j9VrxnI?e=BGrlaX&download=1>`_ (approx. 19 GB)
* `CrocoLake-BGC <https://whoi-my.sharepoint.com/:u:/g/personal/enrico_milanese_whoi_edu/IQAw9Zl4yZomTJk5XY29whWVAY2ObAnriNk5pvGKZtOuWCU?e=yhmrng&download=1>`_ (approx. 7 GB)

.. _crocolake_conventions:

CrocoLake's conventions
-----------------------

The naming convention is largely based on `Argo <https://argo.ucsd.edu/>`_'s convention.

Variables
^^^^^^^^^

Variables and their units are listed below with their presence in PHY and/or BGC CrocoLake versions.

.. csv-table::
   :header-rows: 1
   :file: _static/CrocoLakeDocs_Variables.csv

Important notes:

* To each measured parameter <PARAM> also correspond a quality-control flag and an error variable, called <PARAM>_QC (uint8) and <PARAM>_ERROR (float32) respectively. This applies to all variables except LATITUDE, LONGITUDE, JULD, and DB_NAME.
* For the physical version, the variable DATA_MODE (string) reports the recording mode for Argo's measurements ('R' for real time, 'A' adjusted, 'D' delayed). This choice is to privilege the presence of the best measurements available, although the user should proceed with care when using real-time data for scientific analysis (see `here <https://argo.ucsd.edu/data/how-to-use-argo-files/>`_ and `also here <https://argo.ucsd.edu/data/data-faq/>`_).
* For the same purpose, the BGC version contains the <PARAM>_DATA_MODE variable for each measured parameter (except LATITUDE, LONGITUDE, JULD).
* The database is stored as a table, where each row contains measurements at a point in the <DB_NAME, LATITUDE, LONGITUDE, JULD, PRES> space. If a variable was not measured at a point, it is generally set as missing using pandas' NA dtype. This is great because it allows a consistent treatment of missing data across data types when generating CrocoLake. At the same time, when *reading* CrocoLake, each language and package deals with missing data in its own way, and it is recommended that you familiarize with the tools you are using, so that you know how they perform stastics when missing data are included (relevant discussions for `pandas here <https://pandas.pydata.org/docs/user_guide/missing_data.html>`_, and for `dask here <https://github.com/dask/dask/issues/9845>`_ and `here <https://github.com/dask/dask/issues/11235>`_).
* The data is generally stored using pyarrow as backend, and the variables' data type are then pyarrow's implementation. This should be of little relevance (if any) to most users, as common data analysis packages like pandas and dask are compatible with pyarrow's data types (and can convert them to numpy dtypes if needed). Matlab and Julia also appear to not have any issues with this.

Quality-control
^^^^^^^^^^^^^^^

CrocoLake only contains quality-controlled measurements, allowing you to focus on the analysis steps.

Some notes:

* The quality-control flag <PARAM>_QC generally carries on the 'good data' value from the original source; when missing in the original source, it is set to 1 if the source data is deemed good (bad data are discarded).
* For how quality-control is performed on each dataset, see :ref:`the dedicated page <available_datasets>`.
* Not all measurements have error estimates <PARAM>_ERROR, even if they are deemed of good quality. In these cases, the corresponding <PARAM>_ERROR value is represented as missing.

