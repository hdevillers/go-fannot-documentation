``uniprot-download``
====================

Description of ``uniprot-download``
***********************************

This programme downloads **UniProt** data files directly from official repositories.

Usage of ``uniprot-download``
*****************************

.. code-block::

    uniprot-download -d fungi
        
Arguments of ``uniprot-download``
*********************************

+---------------------+---------+-----------+----------------------------------------------------------+
| Argument            | Default | Mandatory | Description                                              |
+=====================+=========+===========+==========================================================+
| ``-d`` or |br|      | None    | Yes       | Taxonomic division. To see available divisions, |br|     |
| ``-division``       |         |           | use argument ``-list-divisions``.                        |
+---------------------+---------+-----------+----------------------------------------------------------+
| ``-s`` or |br|      | sprot   | No        | Data source. It can be either **sprot** to target |br|   |
| ``-source``         |         |           | **UniProt** data or **trembl** for **TrEMBL** data.      |
+---------------------+---------+-----------+----------------------------------------------------------+
| ``-m`` or |br|      | us      | No        | Download mirror. To see available download mirrors, |br| |
| ``-mirror``         |         |           | use argument ``-list-mirror``.                           |
+---------------------+---------+-----------+----------------------------------------------------------+
| ``-o`` or |br|      | ./      | No        | Output directory.                                        |
| ``-output-dir``     |         |           |                                                          |
+---------------------+---------+-----------+----------------------------------------------------------+
| ``-S`` or |br|      | False   | No        | Boolean argument to display the list of available |br|   |
| ``-list-sources``   |         |           | sources.                                                 |
+---------------------+---------+-----------+----------------------------------------------------------+
| ``-M`` or |br|      | False   | No        | Boolean argument to display the list of available |br|   |
| ``-list-mirror``    |         |           | mirrors.                                                 |
+---------------------+---------+-----------+----------------------------------------------------------+
| ``-D`` or |br|      | False   | No        | Boolean argument to display the list of available |br|   |
| ``-list-divisions`` |         |           | taxonomic divisions.                                     |
+---------------------+---------+-----------+----------------------------------------------------------+

.. |br| raw:: html

    <br>