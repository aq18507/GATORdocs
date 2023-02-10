GATORcell Documentation
=======================

Welcome to the GATORcell documentation. This website is here to aid you through the GATORcell Matlab wrapper. This essentially consists of a series of scripts designed to create ABAQUS input files for GATOR panels, using GMSH as a 3D CAD design and meshing algorithm.

The complete GATORcell code can be found here `GATORcell GitHub page <https://github.com/aq18507/GATORcell>`_. This code relies on three software packages to run. `ABAQUS/Standard <https://www.3ds.com/products-services/simulia/products/abaqus/abaqusstandard/>`_ as an FEA solver ideally with the capability of having access to ABAQUS/CAE for debugging. `GMSH <https://gmsh.info/>`_ as a meshing and command line CAD tool and finally `Matlab <https://uk.mathworks.com/>`_ to run this GATORcell scripts in. Refer to the :ref:`Compatibility` page for the tested software versions.

.. note::
   This site is still a work in progress. Hence why there will be some errors, some areas which are inclomplete and a large number of typos. If you think that some of the documentation is wrong log this here `GATORdocs GitHub Issues page <https://github.com/aq18507/GATORdocumentation/issues>`_.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   Working_Principle
   Compatability
   Syntax
   ShapeFile
   Scripts
   Depreciated_Scripts

 
.. Indices and tables
.. ------------------

.. * :ref:`genindex`
.. * :ref:`modindex`
.. * :ref:`search`
