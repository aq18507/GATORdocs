Comapatbility
=============

Current Version
---------------

The latest GATORcell version relies on the following scripts and versions:

+---------------------------------------------------------------+
| GATORcell Version: v.6.221119                                 |
+-------------------+---------------+---------------------------+
| Script            | Major Version | Script Name               |
+===================+===============+===========================+
| makeModel         |   4           | ``makeModel4.mlx``        |
+-------------------+---------------+---------------------------+
| InputCondtioner   |   6           | ``InputCondtioner6.mlx``  |
+-------------------+---------------+---------------------------+
| Matlab2Abaqus     |   1           | ``Matlab2Abaqus.mlx``     |
+-------------------+---------------+---------------------------+
| runAbaqus         |   4           | ``runAbaqus4.mlx``        |
+-------------------+---------------+---------------------------+
| unitcell          |   7           | ``unitcell7.geo           |
+-------------------+---------------+---------------------------+

GATORcell relies on a  number of scripts that are compatible with each other. The version of each scripts is strcutured as ``v.[MainVersion].[Subversion]``. The compatability is only ever referrs to the main version. since there might be models that are created on older versions it was decided to create new scripts / function names with specifying the version in their name with the following convention: ``[ScriptName][Version].mlx`` noting that version ``1`` is shown as ``[ScriptName].mlx``. The same applies to the GMSH shape files with the ``.geo`` suffix.

.. warning::
    The current versions of GATORcell have only ever been tested with the script version combination shown above. While great care was taken that they are backwards compatible it has not been tested.

Compatability Archive
---------------------

This section lists all older versions. This is for reference only.

GATORcell Version: v.6.220609
+++++++++++++++++++++++++++++

+-------------------+---------------+---------------------------+
| Script            | Major Version | Script Name               |
+===================+===============+===========================+
| makeModel         |   4           | ``makeModel4.mlx``        |
+-------------------+---------------+---------------------------+
| InputCondtioner   |   6           | ``InputCondtioner6.mlx``  |
+-------------------+---------------+---------------------------+
| Matlab2Abaqus     |   1           | ``Matlab2Abaqus.mlx``     |
+-------------------+---------------+---------------------------+
| runAbaqus         |   4           | ``runAbaqus4.mlx``        |
+-------------------+---------------+---------------------------+

GATORcell Version: v.5.220109
+++++++++++++++++++++++++++++

+-------------------+---------------+---------------------------+
| Script            | Major Version | Script Name               |
+===================+===============+===========================+
| makeModel         |   4           | ``makeModel4.mlx``        |
+-------------------+---------------+---------------------------+
| InputCondtioner   |   5           | ``InputCondtioner5.mlx``  |
+-------------------+---------------+---------------------------+
| Matlab2Abaqus     |   1           | ``Matlab2Abaqus.mlx``     |
+-------------------+---------------+---------------------------+
| runAbaqus         |   4           | ``runAbaqus4.mlx``        |
+-------------------+---------------+---------------------------+
