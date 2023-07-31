Scripts
=======

This section documents individual scripts that either is necessary or useful in running GATORcell simulations. Each script is described in its most current version which is outlined in the Compatability section in this documentation. Any depreciated scripts are documented in the **Depreciated Script** section of this documentation.

runAbaqus5
----------

Description
+++++++++++

The ``runAbaqus5`` script is designed to run Abaqus from ``Matlab`` over the command line using the ``.dat`` files and corresponding ``.mat`` files stored in the same directory. To run in its basic versions it does not require an input, i.e. the function only needs to be called.

**Basic working Principle**

#. It scans the directory for ``.dat`` and ``.mat`` files. Once it has generated a list it will submit all files to ABAQUS.
#. There is a gatekeeper function to prevent CPU and Memory overload. The parameter ``NumberOfModels = 6`` which means that it can run 6 models in parallel. It will do this on one single core, which provides the computer with two cores overhead. This is backed up by the licence counter which prevents Abaqus from checking out too many licences.
#. If there are 10 or fewer models to be solved then it will run in sequential mode, to save the time to spool up the parallel computing component of Matlab.
#. There are a number of settings that can be changed if desired.
#. Once all models are solved ``runAbaqus5`` will call the ``dataSort`` function to append the results from the ``.dat`` files into the corresponding ``.mat`` file.

.. warning::
    There are known cases where this function fails to complete, this mostly happens when the ``dataSort`` function is called. This is caused by some files which are not properly closed by Abaqus (the causes are at the time of writing unknown). If this is the case open the Windows Task Manager (``CTRL + ALT + DEL``) and look for the process/es Name **SMAStaMain**. Right-click on the process and select ``End Process``. Repeat if there is more than one of those still running. If they cannot be found then they may be hidden under the **MatlabR20xxx** tab. Once this is done run ``dataSort`` from the command line, noting that you may need to run the preamble to index the scripts directory.


Optional Parameters
+++++++++++++++++++
#. ``Settings.numCores`` defines the number of cores on which each model should be run. The default setting is :math:`1`. There cannot be a total number of models more than the number of cores.
#. ``Settings.runMode`` enables the interactive mode which can be useful for debugging. It is by default set to off i.e. :math:`0`. To turn it on it has to be set to :math:`1`.
#. ``Settings.Memory`` the total amount of memory can be set for each model in ``mb``. It is by default turned off.
#. ``Settings.Gpus`` the number of GPUs can be defined here. The default setting is off. There cannot be a total number of models more than the number of cores.
#. ``Settings.Timeout`` a timeout can be set with this variable. The default setting is off.
#. ``Settings.Parallel`` this setting allows the user to force it to parallel computing at a different number of models. The default setting is :math:`10`. If it is set to :math:`0` then it will force it into parallel mode straight away. 

.. note::
    These parameters are not required. If they are not set, then the default settings are used, which are the ideal settings for most cases.

Syntax
++++++

.. code-block:: matlab
    
    Setting.numCores = 10;
    runAbaqus5(Setting);

findRunningPorcess
------------------

Description
+++++++++++

This function determines whether Abaqus ``standard.exe`` is still running. If it is a dialogue window is called up in Matlab notifying that it is still running. Click ``OK`` to proceed. The next window will present the option of either waiting until all models are finished or killing all Abaqus processes.

Syntax
++++++

.. code-block:: matlab

    findRunningProcess;

.. warning::

    Clicking ``OK`` will terminate all running ``standard.exe`` processes. This means that the model in question may not get completed.


dataSort
--------

Description
+++++++++++

The ``dataSort`` script takes all data from an analysis and sorts it into directories. This script does not require any inputs. It indexes all ``.dat`` files. It then appends the results into the corresponding ``.mat`` files with the structured array named ``Output``. This script is stored in the ``Script`` directory, which means that if it is run for the first time it will require the working directory to be indexed. This can either be done manually or by running the ``preamble;`` command, providing the file exists in the working directory.

Syntax
++++++

.. code-block:: matlab
    
    dataSort;

.. warning::
    If there is a results directory named ``Abaqus_NaN`` then this will be overwritten! Ensure that this is appropriately renamed.


dataRead5
---------

Description
+++++++++++

This function reads data from all ``.mat`` files in a directory and sorts them in a single array. Since the outputs that are required may vary quite significantly it is written in such an array that all data can be accessed with relative ease. To make a request an input structured array must be created; let's call it ``Request`` that contains a request variable that follows the convention ``r1``, ``r2``, ``r3``, etc. Each request consists of a string array which is structured as follows

.. code-block:: matlab

    Request.r[N] = ["[Function]" "[FileIdentifier]" "[Option]" "[StructuredArrayPath]" "[RequestedVariable]"]

**Function:** The following functions are so far available:

#. ``i`` denotes a unique **Identifier**, or identifiers acting as a unique fingerprint to associate outputs from different data sets to one single array entry. Important to note here is that they must be present in all data sets. For instance for a mesh density study where the variable that is changed is defined by a ``MeshSizeMax`` then this will be the variable to track. But likewise if there are for instance :math:`6` unique variables that would idnetify a models are changed, then they need to be identified as such. The data must be in the follwing format :math:`1 \times 1`

#. ``s`` denotes a **Single** output that is not an identifer. This may be some mesh data, or model data. It has to be noted that this data set will not be conditioned and must be of following format :math:`1 \times 1`. 

#. ``a`` offers the ability to extract data from an array that in not in the format of :math:`1 \times 1`. That said the output has to be in the format :math:`1 \times 1` format. The location needs to be specified in the options file where to get the data from. If there is a :math:`4 \times 4` matrix and the data from row :math:`2` culumn :math:`3` then the following needs to be entered as an option ``"2" "3"``.

#. ``dP_max_U3`` this gets the maximum out-of-plain displacement in the :math:`z`-direction. The original data must be in the format :math:`1 \times n`.

#. ``EA_max`` computes the *EA* at the maximum extension, or at the last interwall using the reaction forces using the following equation, where the original data must be in the format :math:`1 \times n`.

.. math::

    \frac{FL}{d}

#. ``EI_max`` computes the *EA* at the maximum extension, or at the last interwall using the reaction forces using the following equation, where the original data must be in the format :math:`1 \times n`.

.. math::
     
    \frac{FL^3}{48d}

#. ``RF_max`` Computes the sum of all reaction forces at maximum extension. The original data must be in the format :math:`1 \times n`.

**FileIdentifier:** The file identifer categorises the model according to a common pattern in the file name. For instance in a file name ``EA_test_1.inp`` where ``EA_`` is the common pattern.

**RequestedVariable:** Defines the path to the data. For instance if it is stored in ``Input.CoreParameters.Mesh`` then each level needs to be specified as a string like the following ``"Input" "CoreParameters" "Mesh"``.

**RequestedVariable:** This is the variable that is requested. It must be in a string format. Note that in the output prompt it will save the variable name if the ``i``, ``s``, ``a`` or ``RF_max`` functions are used. For the ``EA_max`` and ``EI_max`` functions the actual functions are printed for clarity.

Input Examples
++++++++++++++

.. code-block:: matlab

    Request.r1  = ["i" "DP_" "Input" "CoreParameters" "t"];
    Request.r2  = ["i" "DP_" "Input" "CoreParameters" "theta"];
    Request.r3  = ["i" "DP_" "Input" "CoreParameters" "z"];
    Request.r4  = ["i" "DP_" "Input" "CoreParameters" "h"];
    Request.r5  = ["i" "DP_" "Input" "CoreParameters" "dE"];
    Request.r6  = ["i" "DP_" "Input" "CoreParameters" "ts"];
    Request.r7  = ["a" "DP_" "1" "1" "Output" "TotalCpuTime"];
    Request.r8  = ["a" "DP_" "2" "1" "Output" "TotalCpuTime"];
    Request.r9  = ["s" "EA_" "Input" "CoreParameters" "xmax"];
    Request.r10 = ["s" "EA_" "Input" "CoreParameters" "ymax"];
    Request.r11 = ["EA_max" "EA_" "Output" "Step_1" "History_1" "RF2"];
    Request.r12 = ["EI_max" "EI_" "Output" "Step_1" "History_1" "RF3"];
    Request.r13 = ["dP_max_U3" "DP_" "Output" "Step_2" "History_1" "U3"];

Syntax
++++++

.. code-block:: matlab

    [ModelData] = dataRead5(Path,Dir,Request,moveFiles)


licenseCounter
--------------

Description
+++++++++++

This function counts the number of ABAQUS licences used by the computer from which the command is executed. There are no inputs to this function, it will find the computer name and then run the ABAQUS command ``abaqus licensing dslsstat -usage`` to return the number of licences in ``License_num_user``. ABAQUS takes out 50 tokens per licence and the fair usage policy is at the time of writing 400 tokens, which would equate to 8 individual licences. The ``License_availability`` returns the number of available in the first column and the licences used in the second column. The licence tags hard coded in this function are ``QXT`` and ``QSD``. The format is :math:`2\times n`.

Syntax
++++++

.. code-block:: matlab

    [License_num_user, License_availability] = licenseCounter();


existingFiles
-------------

Description
+++++++++++

This function counts the number of files of a certain type present in the directory in which it is executed. It requires a string input with the desired file suffix, for example for a ``.mlx`` file the string ``mlx`` is required. Note that it will internally convert it to the following expression ``*.mlx``. The output is a cell-array containing a file name ``Files`` per cell and the number of files ``Size`` as a secondary output.

Syntax
++++++

.. code-block:: matlab

    [Files,Size] = existingFiles('FileType');


dirCombine
----------

Description
+++++++++++

This function combines the data of two directories and deletes the predefined directory. This function has two inputs and no output. The inputs have to be in string format. 

**dir_1** The first directory is the one that is the one the second directory is copying all its data to.
**dir_2** The second directory is the one that is going to be deleted.

Syntax
++++++

.. code-block:: matlab

    dirCombine('dir_1','dir_2');


recreateInp
-----------

Description
+++++++++++

This function recreates an input file (``.inp``), providing there is a healthy corresponding ``.mat`` file present. Note that this script does not need any input. It needs to be run from the directory in which the corrupted ``.inp`` files and the corresponding ``.mat`` are located.

Syntax
++++++

.. code-block:: matlab

    recreateInp();



printFigure
-----------

Description
+++++++++++

This function creates figures in ``EPS``, ``PDF`` and ``SVG`` formats. It has two inputs, the figure ``Figure`` and the file name ``Name`` (note that this is without the file suffix). The figure needs to be defined using the `Matlab figure <https://uk.mathworks.com/help/matlab/ref/figure.html>`_ command. For instance in its simplest form:

.. code-block:: matlab

    fig = figure;

The ``name`` must be in a string format. The relevant suffix will be added by the ``printFigure`` function. To keep all figures in one place this function will check whether there is a directory with the name ``FIGURE`` present within the directory in which it is executed. If there is no such directory present the function will create one and save all figures in there. Note that this function has no output.

Syntax
++++++

.. code-block:: matlab

    printFigure(fig,'Name');



setLaTeX
--------

Description
+++++++++++
Running this function will set all formatting to :math:`\mathrm{ \LaTeX }`. This function requires to define the desired font size which is a numerical input in the format of :math:`1 \times 1`.

Syntax
++++++

.. code-block:: matlab

    setLaTeX(FontSize);


NodeExtract
-----------

Description
+++++++++++

This function extracts nodes which are enclosed in a volume.
    
**NodeArray:** Is an array containing all the nodes which must be in the format of :math:`N \times 3`, with the following arrangement :math:`x`, :math:`y` and :math:`z`.

**Range:** Defines the box, or area from which the nodes need to be extracted. This must be in the following format: ``[xmin ymin zmin xmax ymax zmax]``.

**Precision:** This is an offset value, which can be used to increase the size of the box. It has to be noted that it will add this in all directions. If the box is defined properly, then it may not be needed and can be set to :math:`0`.

**Nodes:** This will return the Node number, which is within the bounded box.

Syntax
++++++

.. code-block:: matlab

    Nodes = NodeExtract(NodeArray,Range,Precision);





simCounter
----------

Description
+++++++++++

This function counts the number of simulations by monitoring and counting the number of a specified file or directory. This should either be a ``*.lck`` file or a ``*.simdir`` directory. If the number is greater than the specified maximum number in the ``NumberOfModels`` inputs then the script will pause, in other way enter a while loop until the number of monitored files falls below ``NumberOfModels``.
    
**NumberOfModels:** This is a numeric input and must be an integer.

**FileToMonitor:** This must be a string, for instance ``".lck"``. The function looks for a file that ends with the provided string.


Syntax
++++++

.. code-block:: matlab

    simCounter(NumberOfModels,"FileToMonitor");



licensePause
------------

Description
+++++++++++

This script pauses the simulation if a maximum of **8** licenses is used on the license server. This script has no input and no output. It relies on the ``licenseCounter.mlx`` script to count the actual license


Syntax
++++++

.. code-block:: matlab

    licensePause();




resultDir
---------

Description
+++++++++++

The ``resultDir`` script creates the directories in which the result data is going to be saved into. It does not need an input variable as it scans the directory in which it is executed for directories starting with the name *Results*. It will assign a new and unique Result directory name. It has an additional input which is in a string format which can be used if a directory is already defined to extract the paths for the ``OBD``, ``INP``, ``CAD``, ``MSG`` and ``DAT`` subdirectories. Note that this will **not** create new directories

**DirName:** Is a string containing the new directories or already defined directory name.

**odbName:** Path to the ``OBD`` subdirectory. 

**inpName:** Path to the ``INP`` subdirectory. 

**cadName:** Path to the ``CAD`` subdirectory. 

**msgName:** Path to the ``MSG`` subdirectory.

**datName:** Path to the ``DAT`` subdirectory.  

Syntax
++++++

.. code-block:: matlab

    ["DirName","odbName","inpName","cadName","msgName","datName"] = resultDir("DirName");



rcloneUpload
------------

Description
+++++++++++

This script creates a ``.bat`` file with the name ``rcloneUpload.bat``. This file will be created into the directory in which the ``rcloneUpload.mlx`` file is executed from. This file requires ``RCLONE`` to be installed and properly indexed. Furthermore, changes to the file may need to be made to account for different absolute locations in both the local and remote destinations. This script has no input or output.

.. warning::
    The ``rcloneUpload.bat`` should **never** copied into a different directory as it contains the path to the directory in which it was generated in. 

Syntax
++++++

.. code-block:: matlab

    rcloneUpload();



dirIndex
--------

Description
+++++++++++

This script contains all directories which need to be indexed in the ``preamble``. It requires the preamble to index the ``scripts`` directory as this is located in this directory. The Input for this script is the GATORcell root directory level which is determined by the preamble. 

Syntax
++++++

.. code-block:: matlab

    dirIndex(finalLevel);