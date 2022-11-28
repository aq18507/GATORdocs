Scripts
=======

This section documents individual scripts that either are necessary or useful in running GATORcell simulations. Each script is described in its most current version which is outlined in Compatability section in this documentation. Any depreciated scripts are documented in the **Depreciated Script** section of this documentation.

runAbaqus4
----------

Desctiption
+++++++++++

The ``runAbaqus4`` script is designed to run abaqus from ``Matlab`` over command line using the ``.inp`` files and corrsponding ``.mat`` files stored in the same directory. To run in its basic versions it does not require an inptut, i.e. the function only needs to be called.

**Working Priniciple**

#. It scans the directory for ``.inp`` and ``.mat`` files. Once it has generated a list it will submit all files to ABAQUS.
#. There is a gatekeeper function to prevent CPU and Memory overlaod. The parameter ``NumberOfModels = 6`` which means that it can run 6 models in parallel. It will do this on one single core, which provides the computer with two cores overhead.
#. Once all models are solved ``runAbaqus4`` will call the ``dataSort`` function to append the reults from the ``.dat`` files into the corresponding ``.mat`` file.

.. warning::
    There are known cases where this fuction fails to complete, this mostly happens when the ``dataSort`` function is called. This is caused by some files the are not properly closed. This can only be solved by restarting the computer and re-run the ``dataSort`` function.


Optional Parameters
+++++++++++++++++++
#. ``NumberOfModels`` which limits the number of simulations to be run in parallel. Default == :math:`6`

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


dataRead4
---------

Description
+++++++++++

This function reads data from all ``.mat`` files in a directory and sorts in a single array. Since the outputs that are required may vary quite significantly it is written in such an array that all data can be accessed with relative ease. To make a request an input structured array must be created; lets call it ``Request`` that contains a request variable that follows the convention ``r1``, ``r2``, ``r3``, etc. Each request consists of a string array which is structured as follows

.. code-block:: matlab

    r[N] = ["[Function]" "[FileIdentifier]" "[StructuredArrayPath]" "[RequestedVariable]"]

**Function:** The following functions are so far available:
#. ``i`` which denotes a unique identifer, or identifiers acting as an unique finger print to associate outputs from different data sets to one single array entry. For instance for a mesh density study where the variable that is changed is defined by a ``MeshSizeMax`` then this will be the variable to track. But likewise if there are for instance :math:`6` unique variables that would idnetify a models are changed, then they need to be identified as such. The data must be in the follwing format :math:`1 \times 1`

#. ``s`` denotes a single output that is not an identifer. This may be some mesh data, or model data. It has to be noted that this data set will not be conditioned and must be of following format :math:`1 \times 1`. 





