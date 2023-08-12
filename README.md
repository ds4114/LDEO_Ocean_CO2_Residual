# LDEO_Ocean_CO2_Residual

The purpose of this code is to calculate the CO2 flux between ocean and air globally using the CO2-Residual Method (Bennington et al. 2022, JAMES, doi:10.1029/2021MS002960). Data is downloaded from online sources and processed in these scripts.

## Installation and Setup
There are two requirements yaml files with details on the packages and environment needed to execute this code. The "requirements.yml" file contains the minimum package specification independent of operating system. The "requirements-linux-python-3.8.5.yml" contains the  build and versioning that was used in one stable version of development on a Linux system. 

With anaconda, you can create an environment with "conda env create --name [envname] --file=requirements.yml" and then attach it to Jupyter after activating with "ipython kernel install --user --name=[kernelenvname]". IMPORTANT: after installing via conda, please activate the environment; then run "python"; then "import xesmf". This will help complete the installation of the package (see https://xesmf.readthedocs.io/en/latest/installation.html#notes-about-esmpy). While in the python terminal, also run "import os"; then "os.environ['ESMFMKFILE']" to display the path of a variable that will be needed in the configuration file detailed below. 

Before executing any script, please rename the config.yml.example to "config.yml" and edit the contents. This file requires you to specify account details required for some online data sources needing an API key. Specifically, the following are needed:
  
  - download_folder: desired location path for saving data (please end the path with a forward slash). The initial scripts download data from online sources and require 25 GB of space. Note that data will be saved to subfolders such as SST/originals/ and SST/processed/. There is support for Google cloud storage, but this documentation does not include linking and authenticating a bucket within your notebook. 
  - reconstruction_folder: location for saving output of machine learning (please end the path with a forward slash).
  - cds_api_key: API key used for sea level pressure. See https://cds.climate.copernicus.eu/api-how-to for details.
  - chl_user and chl_psw: Account username for chlorophyll data. See https://hermes.acri.fr/index.php?class=ftp_access for details.
  - ESMFMKFILE: location for the path variable required for the XESMF package. See https://earthsystemmodeling.org/docs/release/latest/ESMF_usrdoc/node10.html for details. The requisite file may be found in the installation folder ...\{environment_name}\Library\lib\esmf.mk or copied from the path found above during the installation. 
  

By default, files are downloaded and outputted as NetCDFs. However, there is support for Zarr files which are better suited to cloud storage. If a Google Storage location is specified as the download folder (i.e. beginning with 'gs://'), .zarr file extension will be used by default. During data acquisition, a tmp folder is created in the git repository and used for temporary storage (5 GB max) while converting .nc files from online sources to .zarr. Files in this folder will be purged after script execution. See 01_data_acquisition.ipynb for more details. 


## Execution
Scripts should be executed in sequence beginning with 01_data_aquisition.ipynb and ending with 05_calculate_flux.ipynb. The 00_custom_functions.ipynb script is a reference script with functions that are called elsewhere. NetCDF or Zarr files will be created in the locations specified in the config.yml file after each script. 
In scripts 02_data_processing.ipynb onward, the file names of the downloaded files have been hardcoded. If you make any changes to the file names, please update the appropriate filepath. Additionally, there are several data attributes that are appended to xarray datasets in 03_dataframe.ipynb and 04_ml_for_pco2_residual.ipynb which include hardcoded metadata. Please update these for your execution.

Trouble with XESMF? If you recieve an error, "module 'ESMF' has no attribute 'api'" when regridding, try restarting the kernel and re-running. If you receive an error "KeyError: 'ESMFMKFILE" or "No module named 'ESMF'", please confirm you have specified the correct location for the ESMFMKFILE in config.yml and have set that variable in Jupyter before importing the package. 


## Methodology Updates
This script calculates fCO2 rather than pCO2 and uses a long term fCO2 mean to derive the fCO2 temperature component. Additionally the coefficient for the temperature difference has been updated to 0.0413 per Wanninkhof et al. 2022 resulting in the formula:
 - fCO2_T = fCO2_mean * exp( 0.0413 ( SST - SST_mean ))

For flux calculations, partial fCO2 data along coast lines is currently enhanced using interpolation averaged from 1982 - 2022. 


## Acknowledgements
Co-authors of this code include: Amanda Fay, Thea Heimdal, Val Bennington, Luke Gregor, and Luke Gloege with support from Galen McKinley and the Columbia University LEAP Center.
