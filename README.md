# eb_cray
EasyBuild configs for the Cray platform

## Installing EasyBuild

Find detailed instructions at  http://easybuild.readthedocs.io/en/latest/Installation.html. 

 1. Set EASYBUILD_PREFIX

```
export EASYBUILD_PREFIX=$HOME/easybuild
```

 2. Download and install the code 
```
curl -O https://raw.githubusercontent.com/easybuilders/easybuild-framework/develop/easybuild/scripts/bootstrap_eb.py
python bootstrap_eb.py $EASYBUILD_PREFIX
```

 3. Put this in your .bashrc
```
export EASYBUILD_PREFIX=$HOME/easybuild

export EASYBUILD_MODULES_TOOL=EnvironmentModulesC
export EASYBUILD_MODULE_SYNTAX=Tcl

if hostnamectl | grep -q SUSE; then
  export EASYBUILD_OPTARCH=broadwell
fi

module use $EASYBUILD_PREFIX/modules/all
module load EasyBuild
```

Note: Some modules require the easyblocks delivered with easybuild to be modified. To tell easybuild how to find your own version of those easyblocks you can follow the instructions at [https://github.com/easybuilders/easybuild/wiki/Setting-up-your-own-easyblocks-repository]. This assumes the directory above easyblocks is called easybuild. Alternatively you can just copy the easyblock to the relevant site-packages directory.

 4. Install toolchains for Cray, Intel and GNU compilers

In the eb_cray directory
```
eb --toolchain-name=dummy easyconfigs/CrayIntel-2017.06.eb
eb --toolchain-name=dummy easyconfigs/CrayCCE-2017.06.eb
eb --toolchain-name=dummy easyconfigs/CrayGNU-2017.06.eb
```

## Creating an easybuild config

 1. Find a suitable configuration file (.eb file) and use it as a template
 2. Adapt toolchain and other settings in .eb file, as required

## Building 

In directory easyconfigs, type
```
eb package-pkgversion-toolchain-toolchainversion.eb --robot
```

E.g.,  
```
eb gnuplot-4.6.0-CrayGNU-1.4.10.eb --robot
```

Note: the build may fail for some toolchains (currently CrayGNU-1.4.10) if module craype-x86-skylake is loaded. To fix the problem just unload this module:
```
module unload craype-x86-skylake
``` 
