*****************************************************************
DFTB+: general package for performing fast atomistic calculations
*****************************************************************

|lgpl badge|

#### This is the DFTB+ fork used in the mobile app PHREEQC plus ####

# Changes in this fork #

* all source files (including the build/_deps subfolders) containing function ieee 
substituted by huge() function (due to Android compiler limitations)

* added the stamp defining the release (formatout.F90)

* IMPORTANT: dftb+ binaries linked with libopenblas.a do not work in x86 devices - it is recommended to use liblapack.a and libblas.a

# Compilation #

unzip _deps.zip
mkdir build
mv _deps ./build/
cd build
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/path/to/libblas.a/and/liblapack.a/and/static_python_libs
export PATH=$PATH:/path/to/include
cmake .. -DCMAKE_INSTALL_PREFIX=/path/to/install -DWITH_MPI=FALSE -DWITH_OMP=FALSE -DWITH_ARPACK=TRUE -DWITH_PLUMED=FALSE -DWITH_SOCKETS=FALSE -DWITH_GPU=FALSE -DWITH_MAGMA=FALSE -DWITH_POISSON=FALSE -DARPACK_LIBRARY=/path/to/libarpack.a -DWITH_TBLITE=TRUE -DWITH_MBD=TRUE -DWITH_TRANSPORT=FALSE -DWITH_SDFTD3=TRUE -DWITH_API=TRUE -DWITH_CHIMES=FALSE -DCMAKE_VERBOSE_MAKEFILE:BOOL=ON -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DFETCHCONTENT_FULLY_DISCONNECTED=ON
open link.txt, substitute -lpython3.9 by path /path/to/libpython3.7m.a
make install

####

DFTB+ is a software package for carrying out fast quantum mechanical atomistic
calculations based on the Density Functional Tight Binding method. The most
recent features are described in the (open access) `DFTB+ paper
<https://doi.org/10.1063/1.5143190>`_.

|DFTB+ logo|

DFTB+ can be either used as a standalone program or integrated into other
software packages as a library.


Installation
============

Obtaining via Conda
-------------------

The preferred way of obtaining DFTB+ is to install it via the conda package
management framework using `Miniconda
<https://docs.conda.io/en/latest/miniconda.html>`_ or `Anaconda
<https://www.anaconda.com/products/individual>`_. Make sure to add/enable the
``conda-forge`` channel in order to be able to access DFTB+, and ensure that
the ``conda-forge`` channel is the first repository to be searched for
packages. (Please consult the conda documentation for how to set-up your conda
environment.)

We recommend the use of the `mamba installer <https://mamba.readthedocs.io/>`_,
as we have experienced dependency resolution problems with the original conda
installer in the past::

  conda install -n base mamba

We provide several build variants, choose the one suiting your needs. For
example, by issuing ::

  mamba install 'dftbplus=*=nompi_*'

or ::

  mamba install 'dftbplus=*=mpi_mpich_*'

or ::

  mamba install 'dftbplus=*=mpi_openmpi_*'

to get the last stable release of DFTB+ with, respectively, serial
(OpenMP-threaded) build or with MPI-parallelized build using either the MPICH or
the Open MPI framework.


Downloading the binary
----------------------

A non-MPI (OpenMP-threaded) distribution of the latest stable release can be
found on the `stable release page
<http://www.dftbplus.org/download/dftb-stable/>`_.


Building from source
--------------------

**Note:** This section describes the building with default settings (offering
only a subset of all possible features in DFTB+) in a typical Linux
environment. For more detailed information on the build customization and the
build process, consult the **detailed building instructions** in `INSTALL.rst
<INSTALL.rst>`_.

Download the source code from the `stable release page
<http://www.dftbplus.org/download/dftb-stable/>`_.

You need CMake (>= 3.16) to build DFTB+. If your environment offers no CMake or
only an older one, you can easily install the latest CMake via Python's ``pip``
command::

  pip install cmake

Start CMake by passing your compilers as environment variables (``FC`` and
``CC``), and the location where the code should be installed and the build
directory (``_build``) as options::

  FC=gfortran CC=gcc cmake -DCMAKE_INSTALL_PREFIX=$HOME/opt/dftb+ -B _build .

If the configuration was successful, start the build with::

  cmake --build _build -- -j

After successful build, you should test the code. First download the files
needed for the test ::

  ./utils/get_opt_externals slakos
  ./utils/get_opt_externals gbsa

or ::
  
  ./utils/get_opt_externals ALL

and then run the tests with ::

  pushd _build; ctest -j; popd

If the tests were successful, install the package with ::

  cmake --install _build

For further details see the `detailed building instructions <INSTALL.rst>`_.


Parameterisations
=================

In order to carry out calculations with DFTB+, you need according
parameterisations (a.k.a. Slater-Koster files). You can download them from
`dftb.org <https://dftb.org>`_.


Documentation
=============

Consult following resources for documentation:

* `Step-by-step instructions with selected examples (DFTB+ Recipes)
  <http://dftbplus-recipes.readthedocs.io/>`_

* `Reference manual describing all features (DFTB+ Manual)
  <https://github.com/dftbplus/dftbplus/releases/latest/download/manual.pdf>`_


Citing
======

When publishing results obtained with DFTB+, please cite following works:

* `DFTB+, a software package for efficient approximate density functional theory
  based atomistic simulations; J. Chem. Phys. 152, 124101 (2020)
  <https://doi.org/10.1063/1.5143190>`_

* Reference publications of the Slater-Koster parameterization sets you
  used. (See `dftb.org <https://dftb.org>`_ for the references.)

* Methodological papers relevant to your calculations (e.g. excited states,
  electron-transport, third order DFTB etc.). References to these can be found
  in the `DFTB+ manual
  <https://github.com/dftbplus/dftbplus/releases/latest/download/manual.pdf>`_.


Contributing
============

New features, bug fixes, documentation, tutorial examples and code testing is
welcome in the DFTB+ developer community!

The project is `hosted on github <http://github.com/dftbplus/>`_.
Please check `CONTRIBUTING.rst <CONTRIBUTING.rst>`_ and the `DFTB+ developers
guide <https://dftbplus-develguide.readthedocs.io/>`_ for guide lines.

We are looking forward to your pull request!


License
=======

DFTB+ is released under the GNU Lesser General Public License. See the included
`LICENSE <LICENSE>`_ file for the detailed licensing conditions.



.. |DFTB+ logo| image:: https://www.dftbplus.org/fileadmin/DFTBPLUS/images/DFTB-Plus-Icon_06_f_150x150.png
    :alt: DFTB+ website
    :scale: 100%
    :target: https://dftbplus.org/

.. |lgpl badge| image:: http://www.dftbplus.org/fileadmin/DFTBPLUS/images/license-GNU-LGPLv3-blue.svg
    :alt: LGPL v3.0
    :scale: 100%
    :target: https://opensource.org/licenses/LGPL-3.0
