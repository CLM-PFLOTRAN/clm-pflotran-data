CLM-PFLOTRAN Data Repository
============================

This repository contains the input data for CLM-PFLOTRAN examples and automated testing system.


* Don't append creation dates to the file names. Replace the existing file with the same name and use revision control to revert back to an old version if necessary....?

The __cesm-inputdata__ directory is used as `"DIN_LOC_ROOT"` by CLM. It should be
layed out according to the standard [CESM input
data](https://svn-ccsm-inputdata.cgd.ucar.edu/trunk/inputdata/)
layout. It should be possible, but not necessary to run `link_dirtree`
in that directory.

*NOTE :* Any new tests that rely on standard datm forcing data should
 be configured to use the existing data in
 `atm_forcing.datm7.Qian.T62.c080727/` for the year 2000. This is to
 prevent the repository size from growing even larger that it already
 is.

The __pflotran__ directory is used to hold pflotran input files, meshes,
and clm-pflotran mesh-maps. Mesh directories are named __(u|s)grid-(mesh_size)-(id)__
where 

* __ugrid__ : is unstructured and sgrid is structured.
* __mesh_size__ : some description of the mesh size. 
    Use NXxNYxNZ for structured grids or unstructured meshes where it makes sense. 
    For other unstructured meshes use some proxy for size and shape.
* __id__ : an optional identifier to distinguish between similar meshes.

Inside the mesh directories, mesh files should be
named __(u|s)grid-(mesh_size)-(type).h5__. Mesh maps should use a similar
naming convention as the mesh but append the mapping type and use the
extension '.meshmap'. For example __(u|s)grid-(mesh_size)-(type).meshmap__.
Mapping types are `clm3D\_to\_pflotran3d`....

The pflotran input files should be named __(grid_name)-(mode)-(physics)-(ic).in__ where

* __mode__ : is subsurface|surface|surf-subsurface.
* __physics__ : is richards|th with any necessary qualifiers, e.g. no-ice,
* __ic__ : is a description of the initial conditions, e.g. may or december for TH, saturated or unsaturated for richards mode.


The directory layout is shown graphically below.

    $ tree -F --charset ascii -L 4

    .
    |-- README.md
    |-- cesm-inputdata/
    |   |-- atm/
    |   |   |-- cam/
    |   |   |   `-- chem/
    |   |   `-- datm7/
    |   |       |-- NASA_LIS/
    |   |       `-- domain.T62.050609.nc*
    |   |-- lnd/
    |   |   `-- clm2/
    |   |       |-- firedata/
    |   |       |-- ndepdata/
    |   |       |-- paramdata/
    |   |       |-- snicardata/
    |   |       `-- surfdata_map/
    |   `-- share/
    |       `-- domains/
    |           `-- domain.clm/
    `-- pflotran/
        |-- meshes-maps/
        |   `-- sgrid-1x1x10/
        |       |-- sgrid-1x1x10-clm2pf.meshmap
        |       |-- sgrid-1x1x10-clm2pf_surf.meshmap
        |       `-- sgrid-1x1x10-pf2clm.meshmap
        `-- sgrid-1x1x10-subsurface-th-noice-may.in
    
    

Notes
-----

* fsurdat is checked during cesm_setup, before the `user_nl_clm` file
  has been created. This means there must be a file with the
  `surfdata_${clm_usr_name}_simyr${sim_year}.nc` format, even if you
  don't want to use it and override it in `user_nl_clm`!


