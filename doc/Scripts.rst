Scripts
=======

This section documents individual scripts that either are necessary or useful in running GATORcell simulations. Each script is described in its most current version which is outlined in Compatability section in this documentation.

runAbaqus4
----------
The ``runAbaqus4`` script is designed to run abaqus from ``Matlab`` over command line using the ``.inp`` files and corrsponding ``.mat`` files stored in the same directory.

Desctiption
+++++++++++

Optional Parameters
+++++++++++++++++++
- 

Syntax
++++++

.. code-block:: matlab
    
    runAbaqus4;


dataSort
--------

Description
+++++++++++

The ``dataSort`` script takes all data from an analysis and sorts it into directiores. This script does not require any inputs. It indexes all ``.dat`` files. It then appends the results into the corresponding ``.mat`` files with the structured array named ``Output``. This scrpit is stored in the ``Script`` directory, which means that if it is run for the first time it will required the working directory to be indexted. This can either be done manually or by running the ``preamble;`` command, providing the file exists in the working directory.

Syntax
++++++

.. code-block:: matlab
    
    dataSort;








