Depreciated Scripts
===================

This section contains depreciated scripts. This means that they carry a different vesrion number and may not be compatible with the most up-to-date code. Please refer to the compatabilty section to chech with which version they may be compatible.

.. note::
    
    It is possible that older scripts are not referenced in this documenattion!

    

runAbaqus4
----------

Description
+++++++++++

The ``runAbaqus4`` script is designed to run abaqus from ``Matlab`` over the command line using the ``.inp`` files and corresponding ``.mat`` files stored in the same directory. To run in its basic versions it does not require an input, i.e. the function only needs to be called.

**Working Principle**

#. It scans the directory for ``.inp`` and ``.mat`` files. Once it has generated a list it will submit all files to ABAQUS.
#. There is a gatekeeper function to prevent CPU and Memory overload. The parameter ``NumberOfModels = 6`` which means that it can run 6 models in parallel. It will do this on one single core, which provides the computer with two cores overhead.
#. Once all models are solved ``runAbaqus4`` will call the ``dataSort`` function to append the reults from the ``.dat`` files into the corresponding ``.mat`` file.

.. warning::
    There are known cases where this function fails to complete, this mostly happens when the ``dataSort`` function is called. This is caused by some files the are not properly closed by Abaqus (the causes are at the time of writing unknown). If this is the case open the Windows Task Manager (``CTRL + ALT + DEL``) and look for the process/es Name **SMAStaMain**. Right-click on the process and select ``End Process``. Repeat if there is more than one of those still running. If they cannot be found then they may be hidden under the **MatlabR20xxx** tab. Once this is done run ``dataSort`` from the command line, noting that you may need to run the preamble to index the scripts directory.


Optional Parameters
+++++++++++++++++++
#. ``NumberOfModels`` which limits the number of simulations to be run in parallel. Default == :math:`6`

Syntax
++++++

.. code-block:: matlab
    
    runAbaqus4;