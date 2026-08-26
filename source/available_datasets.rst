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

Saildrones
-------------

CrocoLake contains data from `Saildrone missions <https://www.pmel.noaa.gov/ocs/saildrone/data-access>`_ conducted as part of NOAA's Tropical Pacific Observing System (TPOS) program. The Saildrones data are quality-controlled by NOAA PMEL, with light quality control applied to remove outliers using standard deviation thresholds, manual inspection, and gross error checks since 2021.

The mapping between Saildrone variable names and CrocoLake's is the following::

    params["Saildrones2CROCOLAKE"] = {
        'wmo_id': 'PLATFORM_NUMBER',
        'latitude': 'LATITUDE',
        'longitude': 'LONGITUDE',
        'time': 'JULD',
        'depth': 'DEPTH',
        'TEMP_CTD_MEAN': 'TEMP',
        'TEMP_CTD_RBR_MEAN': 'TEMP',
        'TEMP_SBE37_MEAN': 'TEMP',
        'TEMP_DEPTH_HALFMETER_MEAN': 'TEMP',
        'SAL_SBE37_MEAN': 'PSAL',
        'SAL_RBR_MEAN': 'PSAL',
        'SAL_MEAN': 'PSAL',
        'O2_CONC_SBE37_MEAN': 'DOXY',
        'O2_CONC_MEAN': 'DOXY',
        'O2_CONC_UNCOR_MEAN': 'DOXY',
        'O2_RBR_CONC_MEAN': 'DOXY',
        'O2_CONC_RBR_MEAN': 'DOXY',
        'O2_AANDERAA_CONC_UNCOR_MEAN': 'DOXY',
        'O2_CONC_AANDERAA_MEAN': 'DOXY',
        'CHLOR_WETLABS_MEAN': 'CHLA',
        'CHLOR_RBR_MEAN': 'CHLA',
        'CHLOR_MEAN': 'CHLA',
        'CDOM_MEAN': 'CDOM',
        'BKSCT_RED_MEAN': 'BBP700',
        'TEMP_SBE37_STDDEV': 'TEMP_ERROR',
        'TEMP_DEPTH_HALFMETER_STDDEV': 'TEMP_ERROR',
        'TEMP_CTD_STDDEV': 'TEMP_ERROR',
        'TEMP_CTD_RBR_STDDEV': 'TEMP_ERROR',
        'SAL_SBE37_STDDEV': 'PSAL_ERROR',
        'SAL_RBR_STDDEV': 'PSAL_ERROR',
        'SAL_STDDEV': 'PSAL_ERROR',
        'O2_CONC_SBE37_STDDEV': 'DOXY_ERROR',
        'O2_CONC_STDDEV': 'DOXY_ERROR',
        'O2_CONC_UNCOR_STDDEV': 'DOXY_ERROR',
        'O2_RBR_CONC_STDDEV': 'DOXY_ERROR',
        'O2_CONC_RBR_STDDEV': 'DOXY_ERROR',
        'O2_AANDERAA_CONC_UNCOR_STDDEV': 'DOXY_ERROR',
        'O2_CONC_AANDERAA_STDDEV': 'DOXY_ERROR',
        'CHLOR_WETLABS_STDDEV': 'CHLA_ERROR',
        'CHLOR_RBR_STDDEV': 'CHLA_ERROR',
        'CHLOR_STDDEV': 'CHLA_ERROR',
        'CDOM_STDDEV': 'CDOM_ERROR',
        'BKSCT_RED_STDDEV': 'BBP700_ERROR',
    }

Saildrone measurements do not include depth or pressure as variables. Instead, sensor installation depths are extracted from the metadata attributes (e.g., ``installed_height`` in variable attributes or the ``summary`` attribute in the dataset). To address inconsistencies in metadata availability, depths were compiled from the ``summary`` attribute when available, as it is considered more reliable, and supplemented with ``installed_height`` values when necessary. A detailed table containing depths from both sources is available in the `pull request discussion <https://github.com/boom-lab/crocolaketools-public/pull/6/#issuecomment-3018702245>`_. These depths are then converted to pressure (stored in ``PRES``) using the `Python implementation of the Gibbs SeaWater (GSW) Oceanographic Toolbox of TEOS-10 <https://teos-10.github.io/GSW-Python/intro.html>`_ (specifically, its ``gsw.conversions.p_from_z()`` method).

OleanderXBT
-------------

CrocoLake contains data from the `OleanderXBT project <http://erddap.oleander.bios.edu:8080/erddap/files/oleanderXbtNcFiles/>`_, which collects oceanographic temperature observations along the shipping route between New Jersey and Bermuda. The data contains yearly archives of temperature profile data from 1977 to present.

The mapping between OleanderXBT variable names and CrocoLake's is the following::

    params["Oleander2CROCOLAKE"] = {
        'trajectory' : 'PLATFORM_NUMBER',
        'profile' : 'CYCLE_NUMBER',
        'latitude' : 'LATITUDE',
        'longitude' : 'LONGITUDE',
        'time' : 'JULD',
        'depth' : 'DEPTH',
        'temp' : 'TEMP',
    }

The OleanderXBT dataset provides depth measurements directly, which are retained in CrocoLake as the ``DEPTH`` variable. For consistency with other datasets, these depth values are also converted to pressure (and stored in ``PRES``) using the `Python implementation of the Gibbs SeaWater (GSW) Oceanographic Toolbox of TEOS-10 <https://teos-10.github.io/GSW-Python/intro.html>`_ (specifically, its ``gsw.conversions.p_from_z()`` method).
