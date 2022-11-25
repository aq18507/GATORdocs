Scripts
=======

This section documents individual scripts that either are necessary or useful in running GATORcell simulations. Each script is described in its most current version which is outlined in Compatability section in this documentation.

runAbaqus4
----------

Desctiption
+++++++++++

The ``runAbaqus4`` script is designed to run abaqus from ``Matlab`` over command line using the ``.inp`` files and corrsponding ``.mat`` files stored in the same directory. To run in its basic versions it does not require an inptut, i.e. the function only needs to be called.

**Working Priniciple**

#. It scans the directory for ``.inp`` and ``.mat``files. Once it has generated a list it will submit all files to ABAQUS.
#. There is a gatekeeper function to prevent CPU and Memory overlaod. The parameter ``NumberOfModels = 6`` which means that it can run 6 models in parallel. It will do this on one single core, which provides the computer with two cores overhead.
#. Once all models are solved ``runAbaqus4`` will call the ``dataSort`` function to append the reults from the ``.dat`` files into the corresponding ``.mat`` file.

.. warning::
    There are known cases where this fuction fails to complete, this mostly happens when the ``dataSort`` function is called. This is caused by some files the are not properly closed. This can only be solved by restarting the computer and re-run the ``dataSort`` function.


Optional Parameters
+++++++++++++++++++
#. NumberOfModels which limits the number of simulations to be run in parallel. Default == :math:`6`

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








