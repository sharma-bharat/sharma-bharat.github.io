---
marp: true
---

# Setting up ELM-FATES!

by Bharat Sharma
*bharat.sharma.neu@gmail.com*

---
### Updated on Sep 01, 2023

Optional:
The following instructions are compatible with the Docker.
Please follow the instructions to install the [docker](https://docs.google.com/document/d/13hU_wQ4N39bsTjgDUlKEm8Egr-behDOH-TlJQbwfeoc/edit).

----
### Updated on Feb 14, 2024

<!--### Cloning E3SM from Ryan's Github
 `git clone git@github.com:rgknox/E3SM.git` -->

# Clone from Master
`git clone https://github.com/E3SM-Project/E3SM.git`

`cd E3SM`

# Make a copy of master

`git checkout -b E3SM_BS_config`
<!-- ### Checkout E3SM branch compatible w/FATES nutrients-->
<!-- `git checkout rgknox/lnd/update-suppl-fates-uptake-api25`-->

# Use the following command to download the submodules compatible with the E3SM
`git submodule update --init --recursive`

<!--
### Update submodules which downloads FATES
`git submodule update --init`-->

---
### Updated on Feb 08, 2024
<!--
### Add Ryan’s FATES repository as a source
`cd components/elm/src/external_models/fates/` <br>
`git remote add rgknox_repo git@github.com:rgknox/fates.git` <br>
`git remote -v`<br>
`git fetch rgknox_repo` <br>

---
### Updated on Sep 01, 2023
### Checkout the nutrient branch
`git fetch --all --tags` (fetching all tags) <br>

`git checkout tags/sci.1.61.0_api.25.0.0`

### Creating branch from the tag

`git checkout -b bharat-tag-sci.1.61.0_api.25.0.0 sci.1.61.0_api.25.0.0`

---
### Updated on Sep 01, 2023
### Getting the updated logging fix

`git remote add anthony_repo git@github.com:walkeranthonyp/fates.git`

`git remote -v`

`git fetch anthony_repo`

`git checkout logging-bugfix` <br>
`git fetch`


`git remote add ryan_e3sm https://github.com/rgknox/E3SM.git`

---
### Updated on Sep 01, 2023

E3SM: `rgknox/lnd/update-suppl-fates-uptake-api25` <br>
FATES: `tags/sci.1.61.0_api.25.0.0` <br>
Logging: `walkeranthonyp/logging-bugfix` <br>
-->
files modified:
```
modified:   cime_config/machines/cmake_macros/gnu_cades.cmake
modified:   cime_config/machines/config_machines.xml
modified:   components/elm/cime_config/config_component.xml
modified:   components/elm/cime_config/config_compsets.xml
modified:   components/elm/src/external_models/fates (new commits)
modified:   externals/scorpio/src/clib/pioc_support.c
```
----

### For Baseline

Following changes are required for ORNL Baseline:

https://github.com/E3SM-Project/E3SM/commit/db3790d66664e5f80945e4b46122c1ad1129b8c8

We need a conda env:
`conda env create -f /ccsopen/home/zdr/OLMT.yml`
then run the following command in OLMT env: <br>
`MPICC="mpicc -shared" pip install --no-cache-dir --no-binary=mpi4py mpi4py`



----

cime_config/machines/config_machines.xml

Edit the following line under `cades` machine options
<command name="load">cmake/3.20.3</command>

----
### Updated on Sep 01, 2023
### Details on the modifications
----

**`gnu_cades.cmake` should be**
```
string(APPEND FFLAGS " -O -fno-range-check")
set(HDF5_PATH "/software/dev_tools/swtree/cs400_centos7.2_pe2016-08/hdf5-parallel/1.8.17/centos7.2_gnu5.3.0")
set(NETCDF_PATH "/software/dev_tools/swtree/cs400_centos7.2_pe2016-08/netcdf-hdf5parallel/4.3.3.1/centos7.2_gnu5.3.0")
set(PNETCDF_PATH "/software/dev_tools/swtree/cs400_centos7.2_pe2016-08/pnetcdf/1.9.0/centos7.2_gnu5.3.0")
set(LAPACK_LIBDIR "/software/tools/compilers/intel_2017/mkl/lib/intel64")
string(APPEND SLIBS " -L${NETCDF_PATH}/lib -Wl,-rpath=${NETCDF_PATH}/lib -lnetcdff -lnetcdf")
set(MPICXX "mpic++")
set(SCXX "g++")
set(CXX_LINKER "CXX")
```

----

**Added following lines in `config_compsets.xml`**
```
<compset>
  <alias>I20TRELMFATES</alias>
  <lname>20TR_DATM%QIA_ELM%BGC-FATES_SICE_SOCN_MOSART_SGLC_SWAV</lname>
</compset>
```

**Added following lines in `config_component.xml`**
```
<!--value compset="20TR.*_ELM%[^_]*BGC">20thC_bgc_transient</value -->
<value compset="20TR.*_ELM%[^_]*BGC">20thC_transient</value>
```
---

**Commented following lines in `externals/scorpio/src/clib/pioc_support.c`**
```
/*if ((file->mode & NC_64BIT_DATA) == NC_64BIT_DATA)
{
file->mode &= ~NC_64BIT_DATA;
}*/
```
----
### Updated on Sep 01, 2023
Use Call Scripts to run FATES model via OLMT

---
### Updated on Sep 01, 2023

After the simulaions are run successfully, you can use the [ELM_postprocessR](https://github.com/walkeranthonyp/ELM_postprocessR),
developed by Dr. Anthony Walker,
to contatenate and plot some initial plots.

After the FATES sims are done:
 1. PostprocessorR: to create summary nc files and plots
   1.1. files to work on:
   `postprocessR_FACE_bharat.bs` or
   `postprocessR_FACE_bharat2.bs`
   Submit the post processor jobs:
   `submit_pp.sbatch`

   2. post postprocessor: FATESFACE
   These codes/repo uses the outputs of postprocessorR to generate plots for presentations.


---


### Data that I use to run ELM-FATES

ORNL and Duke FACE Experiment Data: [Google Drive link](https://drive.google.com/drive/folders/1i5NVpxDXfBsnRGCNNsuq_lD_u2bmYAK4?usp=drive_link)

---

### My personal branches

`E3SM`

E3SM_BS_config :  copy of master with config changes <br>
fates-cnp-lambda : ryan's cnp with ECA changes <br>
master : master <br>
ryan_bs_config :  config + ryans: in `E3SM_BS_config`, I pulled `fates-cnp-lambda` <br>

`fates`

FATES_BS_local: copy of the main <br>
main:  main/master <br>


