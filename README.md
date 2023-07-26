# LDEO_Ocean_CO2_Residual

The purpose of this code is to calculate the CO2 flux between ocean and air globally using the CO2-Residual Method (Bennington et al. 2022, JAMES, doi:10.1029/2021MS002960). Data is downloaded from online sources and processed in these scripts. Output from this will be submitted to the Global Carbon Budget.

## Installation and Setup
This code was developed in Python 2.7.5 and is compatible with Python 3.9.12. The requirements.yml details the packages and environment needed to execute this code. The initial scripts download data from online sources and require 25 GB of space. 

After cloning this repository, please rename the config.yml.example to "config.yml" and edit the contents. This file requires you to specify account details required for some online data sources needing an API key. Specifically, the following are needed:
  download_folder_local or download_folder_cloud - location for saving data (either locally or on the cloud)
  reconstruction_folder_local or reconstruction_folder_cloud - location for saving output of machine learning (either locally or on the cloud)
  cds_api_key - API key used for sea level pressure. See https://cds.climate.copernicus.eu/api-how-to for details
  chl_user and chl_psw - Account username for chlorophyll data. See https://hermes.acri.fr/index.php?class=ftp_access for details
  ESMFMKFILE - location for the path variable required for the XESMF package. See https://earthsystemmodeling.org/docs/release/latest/ESMF_usrdoc/node10.html for details.

## Execution
Scripts should be executed in sequence beginning with 01_data_aquisition.ipynb and ending with 05_calculate_flux.ipynb. The 00_custom_functions.ipynb script is a reference script with functions that are called elsewhere. NetCDF files will be created in the locations specified in the config.yml file after each script. 

## Acknowledgements
Co-authors of this code include: Amanda Fay, Thea Heimdal, Val Bennington, and Luke Gloege with support from Galen McKinley and the Columbia University LEAP Center.
