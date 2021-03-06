# This module has been archived, no further development is expected. 

    # This file is part of the dune-hdd-demos project:
    #   http://users.dune-project.org/projects/dune-hdd-demos
    # Copyright holders: Felix Albrecht
    # License: BSD 2-Clause License (http://opensource.org/licenses/BSD-2-Clause)

    dune-hdd-demos is a git supermodule which serves as a demonstration module
    for dune-hdd (http://users.dune-project.org/projects/dune-hdd). This module
    provides the correct versions of all relevant DUNE
    (http://www.dune-project.org) modules and external libraries as git
    submodules. It also provides a git submodule located in local/bin which
    containes some helper scripts to download and build the demos.

    To get started, clone this repository and initialize the submodules, i.e.:

        git clone http://users.dune-project.org/repositories/projects/dune-hdd-demos.git
        cd dune-hdd-demos
        git submodule update --init

    You can now check if one of the config.opts.?? files is named after a compiler
    that is available on your system. If not, copy an existing file and edit it
    accordingly (mainly CC, CXX and F77), i.e.:

        cp config.opts.gcc config.opts.mycppcompiler

    Please note that DUNE in general is known to work best with gcc. dune-hdd
    is tested to work with gcc-4.6 and clang-3.1-8. You can now set the CC
    environment variable to match your compiler and generate a PATH.sh file, i.e.:

        CC=clang ./local/bin/gen_path.py
        source PATH.sh

    It is convenient to source this PATH.sh file from now on whenever you want to
    work on dune-hdd, since many of the provided scripts require the CC variable
    to be set correctly.

    If your system has all dependencies installed you can now build everything by
    calling:

        ./local/bin/download_external_libraries.py
        CC=clang-release ./local/bin/build_external_libraries.py
        source PATH.sh
        ./local/bin/build_dune_modules.py

    This process can take several minutes (up to hours, depending on your system).
    There are several things worth noting:

    * We override the compiler definition with `CC=clang-release` to build the
      external libraries. This results in the `CXX_FLAGS` to be set from
      `config.opts.clang-release`, but just for the external libraries.
    * We need to source the `PATH.sh` again after setting up the external libraries
      (since the virtualenv is now set up, which we need for the python bindings).
    * There should now be a `build-cland-debug` directory wihtin each dune module
      containing the build files (see the respective `config.opts` for the
      `BUILDDIR` variable), so take a look at `dune-hdd/build-clang-debug/examples`
      for instance.
    * If you want to build the dune modules with another compiler variant you can
      call

          CC=gcc-release ./local/bin/build_dune_modules.py

      which will build everything in a `build-gcc-release` subdirectory.
    * If you want to temporarily change the `CXX_FLAGS`, go to the build directory
      and call `ccmake` and hit `t`, i.e.

          cd dune-hdd/build-clang-debug
          ccmake
          t

      Then go to `CMAKE_XCC_FLAGS`, hit return and change them accordingly. Finally
      press `c`, `e` and `g` and you are good to go...
