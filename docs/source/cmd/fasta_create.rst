``fasta-create-refdb``
========================

Description of ``fasta-create-refdb``
***************************************

This programme converts an **Fasta** file into a ``refdb`` structure.

Usage of ``fasta-create-refdb``
*********************************

.. code-block::

    fasta-create-refdb         \
        -i reference.fasta       \
        -r refdb_id              \
        -d refdb_dir/            \
        -D "Data description..."    

Arguments of ``fasta-create-refdb``
*************************************

+------------------+---------+-----------+----------------------------------------------------------+
| Argument         | Default | Mandatory | Description                                              |
+==================+=========+===========+==========================================================+
| ``-i`` or |br|   | None    | Yes       | Input **Fasta** file.                                    |
| ``-input``       |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-r`` or |br|   | None    | Yes       | ID of the ``refdb`` to create.                           |
| ``-refdb-id``    |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-d`` or |br|   | None    | Yes       | Directory that contains the ``refdbs``.                  |
| ``-refdb-dir``   |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-e`` or |br|   | False   | No        | Boolean argument indicating that the ``refdb`` can |br|  |
| ``-equal``       |         |           | contain genes that are in the genome to be annotated.    |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-w`` or |br|   | False   | No        | Boolean argument indicating that entries from the |br|   |
| ``-overwrite``   |         |           | ``refdb`` can be used to overwrite other hits            |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-u`` or |br|   | False   | No        | Boolean argument indicating that entries from the |br|   |
| ``-unreviewed``  |         |           | ``refdb`` are unreviewed.                                |
+------------------+---------+-----------+----------------------------------------------------------+
| ``-D`` or |br|   | None    | No        | Short description of the ``refdb``.                      |
| ``-description`` |         |           |                                                          |
+------------------+---------+-----------+----------------------------------------------------------+

.. |br| raw:: html

    <br>