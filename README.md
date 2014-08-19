camkes-smaccm-manifest
===============
CAmkES is a component platform that provides support for developing and building static 
seL4 systems as a collection of interacting components.  The resulting systems are static, 
meaning that all the components and their connections (and thus all the kernel 
managed resources) are defined at design time and instantiated at system initialisation time.  
It is not possible to change the system (e.g., to create or destroy components or to change 
the connections between components) at runtime.  This CAmkES package includes various example 
systems that can be studied, and individually built and run.

This is a version of CAmkES that has some smaccm specific features and code.  Much of it comes from the public repositories at https://github.com/seL4 but some comes from repositories hosted at https://github.com/smaccm.

For general instructions on how to use this repository, see [sel4.systems](http://sel4.systems/Download/building).

For general information about CAmkES see [the CAmkES pages on seL4.systems](http://sel4.systems/CAmkES).

For detailed information about CAmkES see documentation in [the camkes-tool repo](https://github.com/smaccm/camkes-tool/blob/master/docs/index.md).

Prerequisites, in addition to a standard build system for your target, are:
* The Haskell compiler, ghc
* Haskell libraries missingH, split and data-ordlist
* Python
* Python libraries python-tempita, pyelftools, jinja2 and ply
* which, realpath and the libxml2 utilities.

For more specific instructions about checking ut and building CAmkES form this repository see [INSTALL.md](INSTALL.md).

For instructions about building for, and running on, the Odroid-XU see [HARDWARE.md](HARDWARE.md)

