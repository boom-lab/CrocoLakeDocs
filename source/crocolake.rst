CrocoLake
=========

CrocoLake contains observations from different databases, converter and stored in a uniform schema in parquet format. This documentation describes both the database schema and how each observation dataset is brought into CrocoLake, identifying:

* the original sources for the data;
* any data transformation (data type conversion, variables renaming);
* quality-control considerations.

CrocoLake comes in two versions:

* PHY: contains physical measurements (temperature, salinity, pressure);
* BGC: contains physical and biogeochemical measurements (e.g. temperature, dissolved oxygent, chlorophyll, etc.).

CrocoLake's conventions
-----------------------

The naming convention is largely based on `Argo <https://argo.ucsd.edu/>`_'s convention.

Variables
^^^^^^^^^

Variables and their units are listed below with their presence in PHY and/or BGC CrocoLake versions.

Important notes:

* To each measured parameter <PARAM> also correspond a quality-control flag and an error variable, called <PARAM>_QC (uint8) and <PARAM>_ERROR (float32) respectively. This applies to all variables except LATITUDE, LONGITUDE, JULD, and DB_NAME.
* For the physical version, the variable DATA_MODE (string) reports the recording mode for Argo's measurements ('R' for real time, 'A' adjusted, 'D' delayed). This choice is to privilege the presence of the best measurements available, although the user should proceed with care when using the data for scientific analysis (see also `here <https://argo.ucsd.edu/data/how-to-use-argo-files/>`_ and `here <https://argo.ucsd.edu/data/data-faq/>`_).
* For the same purpose, the BGC version contains the <PARAM>_DATA_MODE variable for each measured parameter (except LATITUDE, LONGITUDE, JULD).
* The database is stored as a table, where each row contains measurements at a point in the <DB_NAME, LATITUDE, LONGITUDE, JULD, PRES> space. If a variable was not measured at a point, it is generally set as missing using pandas' NA dtype. This is great because it allows a consistent treatment of missing data across data types when generating CrocoLake. At the same time, when *reading* CrocoLake, each language and package deals with missing data in its own way, and it is recommended that you familiarize with the tools you are using so that you know how they perform stastics when these data are included (relevant discussions for `pandas here <https://pandas.pydata.org/docs/user_guide/missing_data.html>`_, and for `dask here <https://github.com/dask/dask/issues/9845>`_ and `here <https://github.com/dask/dask/issues/11235>`_).
* The data is generally stored using pyarrow as backend, and the variables' data type are then pyarrow's implementation. This should be of little relevance (if any) to most users, as common data analysis packages like pandas and dask are compatible with pyarrow's data types (and can convert them to numpy dtypes if needed). Matlab and Julia also appear to not have any issues with this.

.. csv-table::
   :header-rows: 1
   :file: _static/CrocoLakeDocs_Variables.csv


Quality-control
^^^^^^^^^^^^^^^

CrocoLake only contains quality-controlled measurements, allowing you to focus on the analysis steps.

Some notes:

* The quality-control flag <PARAM>_QC generally carries on the 'good data' value from the original source, and it might be removed in later versions.
* For how quality-control is performed, refer to each database that is imported into CrocoLake as it depends on the original data.
* Not all measurements have error estimates <PARAM>_ERROR, even if they are deemed of good quality. In these cases, the corresponding <PARAM>_ERROR value is represented as missing.

Download links
^^^^^^^^^^^^^^

CrocoLake is re-generated weekly overnight between Saturday and Sunday (UTC time). You can download the most recent versions at these links:

* `CrocoLake-PHY <https://whoi-my.sharepoint.com/:u:/g/personal/enrico_milanese_whoi_edu/ETVsmC-RKnlIpH_cWf1fSHcBCfeAPGT9QOCv7Qxxrbt4Mg?e=3ihaW7&download=1>`_ (approx. 17 GB; all tools except Matlab)
* `CrocoLake-BGC <https://whoi-my.sharepoint.com/:u:/g/personal/enrico_milanese_whoi_edu/EYu01zZNqjJLi9ep8eM3SNwBAG98weAgQWqlmNbYeuncRg?e=ie04C4&download=1>`_ (approx. 6 GB; all tools except Matlab)
* `CrocoLake-PHY <https://whoi-my.sharepoint.com/:u:/g/personal/enrico_milanese_whoi_edu/EZ5RMKSI1pVLoLamkiW4Jv0BKQv7T4ql2PKFiVm5ERHjow?e=4mAN9T&download=1>`_ (approx. 17 GB; Matlab)
* `CrocoLake-BGC <https://whoi-my.sharepoint.com/:u:/g/personal/enrico_milanese_whoi_edu/EYu01zZNqjJLi9ep8eM3SNwBAG98weAgQWqlmNbYeuncRg?e=ie04C4&download=1>`_ (approx. 6 GB; Matlab)

The Matlab version is identical to the other versions, except a few metadata files are removed as they are incompatible with Matlab's parser.
