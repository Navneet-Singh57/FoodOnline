
GDAL/OGR in Python
==================

This Python package and extensions are a number of tools for programming and
manipulating the GDAL_ Geospatial Data Abstraction Library.

The GDAL project maintains SWIG generated Python
bindings for GDAL/OGR. Generally speaking the classes and methods mostly
match those of the GDAL and OGR C++ classes. There is no Python specific
reference documentation, but the https://gdal.org/api/python_bindings.html#tutorials includes Python examples.

Dependencies
------------

 * libgdal (3.9.3 or greater) and header files (gdal-devel)
 * numpy (1.0.0 or greater) and header files (numpy-devel) (not explicitly
   required, but many examples and utilities will not work without it)

Installation
------------

Conda
~~~~~

GDAL can be quite complex to build and install, particularly on Windows and MacOS.
Pre built binaries are provided for the conda system:

https://docs.conda.io/en/latest/

By the conda-forge project:

https://conda-forge.org/

Once you have Anaconda or Miniconda installed, you should be able to install GDAL with:

``conda install -c conda-forge gdal``


pip
~~~

Due to the complex nature of GDAL and its components, different bindings may require additional packages and installation steps.
GDAL can be installed from the `Python Package Index <https://pypi.org/project/GDAL>`__:

::

    pip install gdal


In order to enable numpy-based raster support, libgdal and its development headers must be installed as well as the Python packages numpy, setuptools, and wheel.
To install the Python dependencies and build numpy-based raster support:


::

    pip install numpy>1.0.0 wheel setuptools>=67
    pip install gdal[numpy]=="$(gdal-config --version).*"


Users can verify that numpy-based raster support has been installed with:

::

    python3 -c 'from osgeo import gdal_array'


If this command raises an ImportError, numpy-based raster support has not been properly installed:

::

    Traceback (most recent call last):
    File "<string>", line 1, in <module>
    File "/usr/local/lib/python3.12/dist-packages/osgeo/gdal_array.py", line 10, in <module>
      from . import _gdal_array
    ImportError: cannot import name '_gdal_array' from 'osgeo' (/usr/local/lib/python3.12/dist-packages/osgeo/__init__.py)


This is most often due to pip reusing a cached GDAL installation.
Verify that the necessary dependencies have been installed and then run the following to force a clean build:

::

    pip install --no-cache --force-reinstall gdal[numpy]=="$(gdal-config --version).*"


Potential issues with GDAL >= 3.9, Python >= 3.9 and NumPy 2.0
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The pyproject.toml file of GDAL 3.9 requires numpy >= 2.0.0rc1 (for Python >= 3.9)
at build time to be able to build bindings that are compatible of both NumPy 1
and NumPy 2.
If for some reason the numpy >= 2.0.0rc1 build dependency can not be installed,
it is possible to manually install the build requirements, and invoke ``pip install``
with the ``--no-build-isolation`` flag.

::

    pip install numpy==<required_version> wheel setuptools>=67
    pip install gdal[numpy]=="$(gdal-config --version).*" --no-build-isolation


Building as part of the GDAL library source tree
------------------------------------------------

Python bindings are generated by default when building GDAL from source.
For more detail, see `Python bindings options <https://gdal.org/development/building_from_source.html#building-python-bindings>`__

The GDAL Python package is built using `SWIG <https://www.swig.org>`__. The currently supported version
is SWIG >= 4

Usage
-----

Imports
~~~~~~~

There are five major modules that are included with the GDAL_ Python bindings.::

  >>> from osgeo import gdal
  >>> from osgeo import ogr
  >>> from osgeo import osr
  >>> from osgeo import gdal_array
  >>> from osgeo import gdalconst

API
~~~

API documentation is available at https://gdal.org/api/python/osgeo.html

Numpy
-----

One advanced feature of the GDAL Python bindings not found in the other
language bindings is integration with the Python numerical array
facilities. The gdal.Dataset.ReadAsArray() method can be used to read raster
data as numerical arrays, ready to use with the Python numerical array
capabilities.

Tutorials
---------

See https://gdal.org/api/python_bindings.html#tutorials

Gotchas
-------

Although GDAL's and OGR's Python bindings provide a fairly "Pythonic" wrapper around the underlying C++ code, there are several ways in which the Python bindings differ from typical Python libraries.
These differences can catch Python programmers by surprise and lead to unexpected results. These differences result from the complexity of developing a large, long-lived library while continuing to maintain
backward compatibility. They are being addressed over time, but until they are all gone, please review this list of https://gdal.org/api/python_gotchas.html

Examples
--------

* An assortment of other samples are available in the `Python github samples directory <https://github.com/OSGeo/gdal/tree/master/swig/python/gdal-utils/osgeo_utils/samples>`__
  with some description in the https://gdal.org/api/python_bindings.html#examples.
* Several `GDAL utilities <https://github.com/OSGeo/gdal/tree/master/swig/python/gdal-utils/osgeo_utils/>`__
  are implemented in Python and can be useful examples.
* The majority of GDAL regression tests are written in Python. They are available at
  `https://github.com/OSGeo/gdal/tree/master/autotest <https://github.com/OSGeo/gdal/tree/master/autotest>`__

One example of GDAL/numpy integration is found in the `val_repl.py <https://github.com/OSGeo/gdal/tree/master/swig/python/gdal-utils/osgeo_utils/samples/val_repl.py>`__ script.

.. note::
   **Performance Notes**

   ReadAsArray expects to make an entire copy of a raster band or dataset
   unless the data are explicitly subsetted as part of the function call. For
   large data, this approach is expected to be prohibitively memory intensive.


.. _GDAL: https://gdal.org
