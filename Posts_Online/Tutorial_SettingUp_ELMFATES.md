---
marp: true
---

# Setting up ELM-Fates!

by Bharat Sharma
*bharat.sharma.neu@gmail.com*

---
### Updated on Jun 20, 2023

Optional:
The following instructions are compatible with the Docker.
Please follow the instructions to install the [docker](https://docs.google.com/document/d/13hU_wQ4N39bsTjgDUlKEm8Egr-behDOH-TlJQbwfeoc/edit).

----
### Updated on Jun 20, 2023

### Cloning E3SM from Ryan's Github
`git clone git@github.com:rgknox/E3SM.git` 
`cd E3SM`

### Checkout E3SM branch compatible w/FATES nutrients
`git checkout rgknox/lnd/update-suppl-fates-uptake-api25`

### Update submodules which downloads FATES
`git submodule update --init`

---
### Updated on Jun 20, 2023
### Add Ryan’s FATES repository as a source
`cd components/elm/src/external_models/fates/` <br>
`git remote add rgknox_repo git@github.com:rgknox/fates.git` <br>
`git remote -v`<br>
`git fetch rgknox_repo` <br>

---
### Updated on Jun 20, 2023
### Checkout the nutrient branch
`git fetch --all --tags` (fetching all tags) <br>

`git checkout tags/sci.1.61.0_api.25.0.0`

### Creating branch from the tag

`git checkout -b bharat-tag-sci.1.61.0_api.25.0.0 sci.1.61.0_api.25.0.0`

---
### Updated on Jun 20, 2023
### Getting the updated logging fix

`git remote add anthony_repo git@github.com:walkeranthonyp/fates.git`
`git remote -v`
`git fetch anthony_repo`

`git checkout logging-bugfix` <br>
`git fetch`

---
## Feb 13, 2022

E3SM: `rgknox/lnd/update-suppl-fates-uptake-api25`
FATES: `tags/sci.1.61.0_api.25.0.0`
Logging: `walkeranthonyp/logging-bugfix`

files modified:
```
modified:   cime_config/machines/cmake_macros/gnu_cades.cmake
modified:   components/elm/cime_config/config_component.xml
modified:   components/elm/cime_config/config_compsets.xml
modified:   components/elm/src/external_models/fates (new commits)
modified:   externals/scorpio (modified content)
```

----
### Updated on Jun 20, 2023
### Details on the modifications

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

**Added following lines in `gnu_cades.cmake`**
```
set(SCXX "g++")
set(CXX_LINKER "CXX")    
```
**Commented following lines in `externals/scorpio/src/clib/pioc_support.c`**
```
/*if ((file->mode & NC_64BIT_DATA) == NC_64BIT_DATA)
{
file->mode &= ~NC_64BIT_DATA;
}*/
```      
----
### Updated on Jun 20, 2023
Use Call Scripts to run FATES model via OLMT

---
### Updated on Jun 20, 2023
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