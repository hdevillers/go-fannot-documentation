``uniprot-split``
=================

Description of ``uniprot-split``
********************************

This programme splits an **UniProt** data file into *n* files. It can be used to optimize some data treatments.

Usage of ``uniprot-split``
**************************

.. code-block::

    uniprot-split         \
        -i uniport.dat.gz \
        -o splited_data   \
        -n 10             \
        -c

Arguments of ``uniprot-split``
******************************

+------------------+---------+-----------+----------------------------------------------------------+
| Argument         | Default | Mandatory | Description                                              |
+==================+=========+===========+==========================================================+
| ``-i`` or |br|   | None    | Yes       | Input **UniProt** data file. It can be compressed or not.|
| ``-input``       |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-o`` or |br|   | None    | Yes       | Output basename.                                         |
| ``-output``      |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-n`` or |br|   | 10      | No        | Number of slpits required.                               |
| ``-n-splits``    |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-c`` or |br|   | False   | No        | Boolean arguement to compress each output files.         |
| ``-compress``    |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+

.. |br| raw:: html

    <br>
