For machines that are set up with [SEMS development environment modules](https://sems.sandia.gov/confluence/display/SEMSKB/SEMS+NFS+TPL+System) (e.g. usually nfs mounted or rsynced under `/project/sems/`) the configuration of Trilinos is easy.  One just sources a single environment script then loads a single `*.cmake` configurations file and then select the desired enables.

To take advantage of this SEMS env, first source the script [load_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/load_sems_dev_env.sh) as:

```
$ cd <some-build-dir>/
$ source $TRILINOS_DIR/cmake/load_sems_dev_env.sh
``` 

(where `TRILINOS_DIR` points to the base Trilinos git repo source dir, e.g. `$HOME/Trilinos`).  This does a `module purge` then loads SEMS modules for compilers, MPI, Git, CMake, Python, and several key TPLs that can be used by Trilinos like Boost, Zlib, HDF5, Netcdf, ParMETIS, and SuperLU.  To see what is loaded, run `module list` which should return something similar to:

```
$ module list
Currently Loaded Modulefiles:
  1) sems-env                       8) sems-yaml_cpp/0.5.3/base
  2) sems-python/2.7.9              9) sems-zlib/1.2.8/base
  3) sems-cmake/3.5.2              10) sems-hdf5/1.8.12/parallel
  4) sems-git/2.10.1               11) sems-netcdf/4.3.2/parallel
  5) sems-gcc/4.8.4                12) sems-parmetis/4.0.3/parallel
  6) sems-openmpi/1.6.5            13) sems-superlu/4.3/base
  7) sems-boost/1.63.0/base
```

(NOTE: Actual modules and versions may be different for the current version of Trilinos.)

Once a SEMS Dev Env has been loaded, then one can configure, build, and test as (for example):

```
$ cmake \
  -C $TRILINOS_DIR/cmake/std/SEMSDevEnv.cmake
  -DBUILD_SHARED_LIBS=ON \
  -DCMAKE_BUILD_TYPE=DEBUG \
  -DTPL_ENABLE_MPI=ON \
  -DTrilinos_ENABLE_<PKG0>=ON \
  -DTrilinos_ENABLE_<PKG1>=ON \
  $TRILNOS_DIR
$ make -j10
$ ctest -j10
 ```

(NOTE: the `SEMSDevEnv.cmake` file can alternatively be loaded using `-DTrilinos_CONFIGURE_OPTIONS_FILE:STRING=cmake/std/SEMSDevEnv.cmake`.  See [Trilinos_CONFIGURE_OPTIONS_FILE](https://trilinos.org/docs/files/TrilinosBuildReference.html#trilinos-configure-options-file) for details.)

The search paths for all of the TPLs supported by the loaded SEMS Dev Env will automatically be set.  All that is needed is to enable the desired TPLs.  The same loaded SEMS Dev Env can be used for MPI or serial (non-MPI) builds and shared lib or static lib builds.  The SEMS Dev Env only needs to be changed when one wants a different compiler/version and/or MPI/version and/or CMake/version using, for example:

```
$ source $TRILINOS_DIR/cmake/load_sems_dev_env.sh \
   sems-gcc/5.3.0  sems-openmpi/1.10.1  sems-cmake/3.3.2
```

Different compilers (GCC, Clang, Intel) and several versions of each can be selected.  Different versions of OpenMPI and CMake can also be selected (see modules starting with `sems-` from `module avail`).  See the default versions for each of these and more documentation in the script [load_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/load_sems_dev_env.sh) itself.  The defaults for GCC, OpenMPI, and CMake are selected match the standard CI build for Trilinos (see [checkin-test-sems.sh](Policies-%7C-Safe-Checkin-Testing) and the [post-push CI build](Policies--%7C-Testing#post_push_ci_testing)).

NOTES:

* **WARNING:** Sourcing `load_sems_dev_env.sh` clears out any modules one may have set before loading the new SEMS modules.  Therefore, to avoid messing up your current env, one may want to start a new shell.

* If the script `load_sems_dev_env.sh` is not used, then one can load the various SEMS modules manually and then configure as shown above.  The script `load_sems_dev_env.sh` does not have to be used.

* To unload a loaded dev env and get back to no loaded modules, source the script [unload_sems_dev_env.sh](https://github.com/trilinos/Trilinos/blob/develop/cmake/unload_sems_dev_env.sh).  This will do `module purge`.

* On Mac OSX, this script will load an env but it loads a different env from Linux and Trilinos currently does not build with that env.  This script can really only be used for Linux machines with the SEMS env and `sh` compatible shells (e.g. `bash`, sorry no `(t)csh` versions are currently available).
