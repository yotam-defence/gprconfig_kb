GPRCONFIG KB
============

GPRCONFIG KB is a knowledge base for configuring GPR toolchains, used by
[GPRbuild](https://github.com/AdaCore/gprbuild) and
[GPR2](https://github.com/AdaCore/gpr) projects.

Installation and use
--------------------

Please refer to corresponding project documentation for instructions on how
gprconfig_kb is used by that project.

Doc & Examples
--------------

The documentation for this knowledge base is available
[online](http://docs.adacore.com/gprbuild-docs/html/gprbuild_ug/companion_tools.html#the-gprconfig-knowledge-base).

## CMake support

    cmake --preset default && cmake --build --preset default && ctest --preset default

* `ctest` runs `kb_structure` (`cmake/ValidateKB.cmake`): every `db/*.xml` is
  non-empty, starts with `<?xml` or `<gprconfig>` and has balanced `<gprconfig>` root tags.
* `cmake --install` copies `db/` to `share/gprconfig` and installs the package
  `gprconfig_kb` (`find_package(gprconfig_kb)`), exposing `gprconfig_kb_DB_DIR`
  and the interface target `gprconfig_kb::db`.
