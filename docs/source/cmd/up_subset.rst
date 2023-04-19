``uniprot-subset``
==================

Description of ``uniprot-subset``
*********************************

This programme allows to create subset of data from an **UniProt** data file, applying plain text search and regular expression tests on the different entries.

Usage of ``uniprot-subset``
***************************

.. code-block::

    uniprot-subset         \
        -i uniport.dat.gz \
        -o subset.dat.gz  \
        -e "1|2"          \
        -t Yarrowia   

Arguments of ``uniprot-subset``
*******************************

+------------------+---------+-----------+----------------------------------------------------------+
| Argument         | Default | Mandatory | Description                                              |
+==================+=========+===========+==========================================================+
| ``-i`` or |br|   | None    | Yes       | Input **UniProt** data file. It can be compressed or not.|
| ``-input``       |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-o`` or |br|   | None    | Yes       | Output **UniProt** pruned data file.                     |
| ``-output``      |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-m`` or |br|   | False   | No        | Boolean arguement to keep only entries from ``refdb``    |
| ``-methionine``  |         |           | that starts with a methionine.                           |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-d`` or |br|   | False   | No        | Boolean arguement to keep only entries from ``refdb``    |
| ``-description`` |         |           | that has a short description (found at **DE** lines).    |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-f`` or |br|   | False   | No        | Boolean arguement to keep only entries from ``refdb``    |
| ``-function``    |         |           | that has a function description (found at **CC** lines). |
+------------------+---------+-----------+----------------------------------------------------------+

.. |br| raw:: html

    <br>