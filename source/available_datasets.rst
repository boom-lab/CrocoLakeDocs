.. _available_datasets:

Available datasets
==================

CrocoLake contains several datasets. This page illustrates the datasets that are currently available, specifying sources, what variables are carried over, what quality control is performed, and any other relevant steps.

Argo
----

`Argo <https://argo.ucsd.edu/>`_'s observations are retrieved from the profile files hosted in the `GDAC <https://usgodae.org/pub/outgoing/argo/dac/>`_:

* physical observations are extracted from the core profile files (i.e. <PLATFORM_NUMBER>_prof.nc files);
* biogeochemical observations are extracted from the synthetic profile files (i.e. <PLATFORM_NUMBER>_Sprof.nc files).

For each observed parameter <PARAM>, Argo's profiles report both real time (<PARAM>, <PARAM>_QC), and adjusted (<PARAM>_ADJUSTED, <PARAM>_ADJUSTED_QC, and <PARAM>_ADJUSTED_ERROR) variables. CrocoLake keeps the best measurement available, i.e. CrocoLake's <PARAM>, <PARAM>_QC and <PARAM>_ERROR are assigned the values

* <PARAM>_ADJUSTED, <PARAM>_ADJUSTED_QC, and <PARAM>_ADJUSTED_ERROR, if <PARAM>_ADJUSTED_QC is in [1,2,5,8] and <PARAM>_DATA_MODE is in ['A','D'];
* <PARAM>, <PARAM>_QC, and pd.NA, if <PARAM>_ADJUSTED_QC is not in [1,2,5,8], <PARAM>_DATA_MODE is 'R', and <PARAM>_QC is in [1,2,5,8];
* pd.NA otherwise.

This allows to keep the best value available at each location. For best practices for scientific analysis, Argo's guidelines are `here <https://argo.ucsd.edu/data/how-to-use-argo-files/>`_ and `also here <https://argo.ucsd.edu/data/data-faq/>`_.

Argo's observations are refreshed weekly.

GLODAP
------

CrocoLake contains the core variables of `GLODAP <https://glodap.info/>`_'s most recent data product v2.2023, which are already quality controlled. A description of the dataset is provided in `Lauvset et al (2024) <https://doi.org/10.5194/essd-16-2047-2024>`_.

The mapping between GLODAP's variables names and CrocoLake's is the following::

    params["GLODAP2CROCOLAKE"] = {
        'G2expocode' : 'PLATFORM_NUMBER',
        'G2latitude' : 'LATITUDE',
        'G2longitude' : 'LONGITUDE',
        'G2pressure' : 'PRES',
        'G2temperature' : 'TEMP',
        'G2salinity' : 'PSAL',
        'G2oxygen' : 'DOXY',
        'G2nitrate' : 'NITRATE',
        'G2silicate' : 'SILICATE',
        'G2phosphate' : 'PHOSPHATE',
        'G2tco2' : 'TCO2',
        'G2talk' : 'TOT_ALKALINITY',
        'G2phtsinsitutp' : 'PH_IN_SITU_TOTAL',
        'G2cfc11' : 'CFC11',
        'G2cfc12' : 'CFC12',
        'G2cfc113' : 'CFC113',
        'G2ccl4' : 'CCL4',
        'G2sf6' : 'SF6',
        'G2chla' : 'CHLA',
        'G2salinityf' : 'PSAL_QC',
        'G2oxygenf' : 'DOXY_QC',
        'G2nitratef' : 'NITRATE_QC',
        'G2silicatef' : 'SILICATE_QC',
        'G2phosphatef' : 'PHOSPHATE_QC',
        'G2tco2f' : 'TCO2_QC',
        'G2talkf' : 'TOT_ALKALINITY_QC',
        'G2phtsinsitutpf' : 'PH_IN_SITU_TOTAL_QC',
        'G2cfc11f' : 'CFC11_QC',
        'G2cfc12f' : 'CFC12_QC',
        'G2cfc113f' : 'CFC113_QC',
        'G2ccl4f' : 'CCL4_QC',
        'G2sf6f' : 'SF6_QC',
        'G2chlaf' : 'CHLA_QC',
    }

Spray Gliders
-------------

CrocoLake contains `Spray Glider <https://spraydata.ucsd.edu/>`_ data `Level 3 <https://spraydata.ucsd.edu/data-access>`_, which are already quality controlled.

The mapping between Spray Glider data variables names and CrocoLake's is the following::

    params["SprayGliders2CROCOLAKE"] = {
        'mission_name' : 'PLATFORM_NUMBER',
        'lat' : 'LATITUDE',
        'lon' : 'LONGITUDE',
        'temperature' : 'TEMP',
        'salinity' : 'PSAL',
        'time': 'JULD',
    }

All measurements' quality flags are assigned equal to 1.

Spray Gliders data report depth and not pressure. For consistency with the :ref:`CrocoLake variables <crocolake_conventions>`, ``depth`` values are converted to pressure (and stored in ``PRES``) using the `Python implementation of the Gibbs SeaWater (GSW) Oceanographic Toolbox of TEOS-10 <https://teos-10.github.io/GSW-Python/intro.html>`_ (specifically, its ``gsw.conversions.p_from_z()`` method).
