``uniprot-create-refdb``
========================

Description of ``uniprot-create-refdb``
***************************************

This programme converts an **UniProt** data file into a ``refdb`` structure.

Usage of ``uniprot-create-refdb``
*********************************

.. code-block::

    uniprot-create-refdb         \
        -i uniport.dat.gz        \
        -r refdb_id              \
        -d refdb_dir/            \
        -D "Data description..."    

Arguments of ``uniprot-create-refdb``
*************************************

+------------------+---------+-----------+----------------------------------------------------------+
| Argument         | Default | Mandatory | Description                                              |
+==================+=========+===========+==========================================================+
| ``-i`` or |br|   | None    | Yes       | Input **UniProt** data file. It can be compressed or not.|
| ``-input``       |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-r`` or |br|   | None    | Yes       | ID of the ``refdb`` to create.                           |
| ``-refdb-id``    |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-d`` or |br|   | None    | Yes       | Directory that contains the ``refdbs``.                  |
| ``-refdb-dir``   |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-e`` or |br|   | False   | No        | Boolean arguement indicating that the ``refdb`` can |br| |
| ``-equal``       |         |           | contain genes that are in the genome to be annotated.    |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-w`` or |br|   | False   | No        | Boolean argument indicating that entries from the |br|   |
| ``-overwrite``   |         |           | ``refdb`` can be used to overwrite other hits            |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-u`` or |br|   | False   | No        | Boolean argument indicating that entries from the |br|   |
| ``-unreviewed``  |         |           | ``refdb`` are unreviewed.                                |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-g`` or |br|   | False   | No        | Boolean argument indicating that gene names from |br|    |
| ``-gene-name``   |         |           | entries from the ``refdb`` can be transfered.            |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-D`` or |br|   | None    | No        | Short description of the ``refdb``.                      |
| ``-description`` |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+

.. |br| raw:: html

    <br>