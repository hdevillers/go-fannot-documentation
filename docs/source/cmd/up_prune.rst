``uniprot-prune``
=================

Description of ``uniprot-prune``
********************************

This program prunes (filters) an **UniProt** data file according to different criteria.

Usage of ``uniprot-prune``
**************************

.. code-block::

    uniprot-prune         \
        -i uniport.dat.gz \
        -o pruned.dat.gz  \
        -m                \
        -d                \   
        -f

Arguments of ``uniprot-prune``
******************************

+------------------+---------+-----------+----------------------------------------------------------+
| Argument         | Default | Mandatory | Description                                              |
+==================+=========+===========+==========================================================+
| ``-i`` or |br|   | None    | Yes       | Input **UniProt** data file. It can be compressed or not.|
| ``-input``       |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-o`` or |br|   | None    | Yes       | Output **UniProt** pruned data file.                     |
| ``-output``      |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-m`` or |br|   | False   | No        | Boolean argument to keep only entries from ``refdb``     |
| ``-methionine``  |         |           | that starts with a methionine.                           |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-d`` or |br|   | False   | No        | Boolean argument to keep only entries from ``refdb``     |
| ``-description`` |         |           | that has a short description (found at **DE** lines).    |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-f`` or |br|   | False   | No        | Boolean argument to keep only entries from ``refdb``     |
| ``-function``    |         |           | that has a function description (found at **CC** lines). |
+------------------+---------+-----------+----------------------------------------------------------+

.. |br| raw:: html

    <br>
