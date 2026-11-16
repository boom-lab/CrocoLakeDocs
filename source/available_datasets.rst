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

CrocoLake contains the core variables of `GLODAP <https://glodap.info/>`_'s most recent data product (`GLODAPv3 <https://glodap.info/index.php/merged-and-adjusted-data-product-v3/>`_), which are already quality controlled. A description of the dataset is provided in `Lauvset et al (2024) <https://doi.org/10.5194/essd-16-2047-2024>`_.

The mapping between GLODAP's variables names and CrocoLake's is the following:

.. list-table::
   :header-rows: 1

   * - GLODAP variable
     - CrocoLake variable
     - Notes
   * - ``expocode``
     - ``PLATFORM_NUMBER``
     - Cruise expocode
   * - ``year``, ``month``, ``day``, ``hour``, ``minute``
     - ``JULD``
     - Combined into a single timestamp
   * - ``latitude``, ``longitude``
     - ``LATITUDE``, ``LONGITUDE``
     -
   * - ``pressure``
     - ``PRES``
     - No QC flag in GLODAP; fill values (-9999) are discarded, and rows without pressure are dropped
   * - ``temperature``
     - ``TEMP``
     - No QC flag in GLODAP; fill values (-9999) are discarded
   * - ``salinity``
     - ``PSAL``
     -
   * - ``oxygen``
     - ``DOXY``
     -
   * - ``nitrate``
     - ``NITRATE``
     -
   * - ``silicate``
     - ``SILICATE``
     -
   * - ``phosphate``
     - ``PHOSPHATE``
     -
   * - ``tco2``
     - ``TCO2``
     -
   * - ``talk``
     - ``TOT_ALKALINITY``
     -
   * - ``phtsinsitutp``
     - ``PH_IN_SITU_TOTAL``
     -
   * - ``cfc11``, ``cfc12``, ``cfc113``
     - ``CFC11``, ``CFC12``, ``CFC113``
     -
   * - ``ccl4``, ``sf6``
     - ``CCL4``, ``SF6``
     -
   * - ``chla``
     - ``CHLA``
     -
   * - ``<param>f``
     - ``<PARAM>_QC``
     - Original GLODAP flag; only flags 0 and 2 are kept

Spray Gliders
-------------

CrocoLake contains `Spray Glider <https://spraydata.ucsd.edu/>`_ data `Level 3 <https://spraydata.ucsd.edu/data-access>`_, which are already quality controlled.

The mapping between Spray Glider data variables names and CrocoLake's is the following:

.. list-table::
   :header-rows: 1

   * - Spray Gliders variable
     - CrocoLake variable
     - Notes
   * - ``mission_name``
     - ``PLATFORM_NUMBER``
     - Mission name
   * - ``profile``
     - ``CYCLE_NUMBER``
     -
   * - ``lat``, ``lon``
     - ``LATITUDE``, ``LONGITUDE``
     -
   * - ``time``
     - ``JULD``
     -
   * - ``depth``
     - ``DEPTH``, ``PRES``
     - Depth is kept and also converted to pressure (see below)
   * - ``temperature`` or ``temp``
     - ``TEMP``
     - Variable name differs between files
   * - ``salinity`` or ``sal``
     - ``PSAL``
     - Variable name differs between files
   * - ``chlorophyll_a``
     - ``CHLA``
     - 
   * - ``doxy``
     - ``DOXY``
     - 

Spray Gliders data report depth and not pressure. For consistency with the :ref:`CrocoLake variables <crocolake_conventions>`, ``depth`` values are converted to pressure (and stored in ``PRES``) using the `Python implementation of the Gibbs SeaWater (GSW) Oceanographic Toolbox of TEOS-10 <https://teos-10.github.io/GSW-Python/intro.html>`_ (specifically, its ``gsw.conversions.p_from_z()`` method).

OleanderXBT
-------------

CrocoLake contains data from the `OleanderXBT project <http://erddap.oleander.bios.edu:8080/erddap/files/oleanderXbtNcFiles/>`_, which collects oceanographic temperature observations along the shipping route between New Jersey and Bermuda. The data contains yearly archives of temperature profile data from 1977 to present.

The mapping between OleanderXBT variable names and CrocoLake's is the following:

.. list-table::
   :header-rows: 1

   * - OleanderXBT variable
     - CrocoLake variable
     - Notes
   * - ``trajectory``
     - ``PLATFORM_NUMBER``
     -
   * - ``profile``
     - ``CYCLE_NUMBER``
     -
   * - ``latitude``, ``longitude``
     - ``LATITUDE``, ``LONGITUDE``
     -
   * - ``time``
     - ``JULD``
     -
   * - ``depth``
     - ``DEPTH``, ``PRES``
     - Depth is kept and also converted to pressure (see below)
   * - ``temp``
     - ``TEMP``
     -

The OleanderXBT dataset provides depth measurements directly, which are retained in CrocoLake as the ``DEPTH`` variable. For consistency with other datasets, these depth values are also converted to pressure (and stored in ``PRES``) using the `Python implementation of the Gibbs SeaWater (GSW) Oceanographic Toolbox of TEOS-10 <https://teos-10.github.io/GSW-Python/intro.html>`_ (specifically, its ``gsw.conversions.p_from_z()`` method).

SPOTS
-----

CrocoLake contains the ship-based biogeochemical time-series data from the `Synthesis Product for Ocean Time Series (SPOTS) <https://doi.org/10.5194/essd-16-1901-2024>`_, which compiles bottle and CTD observations collected at fixed time-series sites (e.g. ALOHA, CARIACO, K2, Irminger Sea, Iceland Sea). A description of the dataset is provided in `Lange et al. (2024) <https://doi.org/10.5194/essd-16-1901-2024>`_. SPOTS data are included in both the PHY and BGC versions of CrocoLake.

The mapping between SPOTS variable names and CrocoLake's is the following:

.. list-table::
   :header-rows: 1

   * - SPOTS variable
     - CrocoLake variable
     - Notes
   * - ``TimeSeriesSite``
     - ``PLATFORM_NUMBER``
     - Name of the time-series site
   * - ``LATITUDE``, ``LONGITUDE``
     - ``LATITUDE``, ``LONGITUDE``
     -
   * - ``DATE`` + ``TIME``
     - ``JULD``
     - ``DATE`` (YYYYMMDD) and ``TIME`` (HHMM, UTC) are combined; a missing ``TIME`` is set to 00:00
   * - ``CTDPRS``
     - ``PRES``
     -
   * - ``CTDTMP``
     - ``TEMP``
     -
   * - ``SALNTY``, or ``CTDSAL`` if ``SALNTY`` is missing
     - ``PSAL``
     - Bottle salinity is preferred; CTD salinity is used where bottle salinity is missing. ``PSAL_QC`` and ``PSAL_ERROR`` come from the same source as ``PSAL``
   * - ``OXYGEN``
     - ``DOXY``
     - Bottle oxygen
   * - ``NITRAT``
     - ``NITRATE``
     -
   * - ``SILCAT``
     - ``SILICATE``
     -
   * - ``PHSPHT``
     - ``PHOSPHATE``
     -
   * - ``ALKALI``
     - ``TOT_ALKALINITY``
     -
   * - ``PH_TOT``
     - ``PH_IN_SITU_TOTAL``
     -
   * - ``<PARAM>_FLAG_W``
     - ``<PARAM>_QC``
     - Original WOCE flag; only flag 2 is kept
   * - ``<PARAM>_A`` or ``<PARAM>_P``
     - ``<PARAM>_ERROR``
     - Accuracy if available, otherwise precision

Quality control: SPOTS reports WOCE quality flags (``<PARAM>_FLAG_W``). CrocoLake carries over these flags unchanged into ``<PARAM>_QC``. Measurements whose flag is not 2 ("acceptable") are discarded, so all retained ``<PARAM>_QC`` values are 2. SPOTS fill values are dropped.

CrocoLake assigns ``CYCLE_NUMBER`` by numbering each unique cast (``TimeSeriesSite``, ``CRUISE``, ``STNNBR``, ``CASTNO``) sequentially within each site. These numbers are not guaranteed to be stable across SPOTS releases.

``DATE_UPDATE`` is set to the SPOTS release date (2024-02-22).
