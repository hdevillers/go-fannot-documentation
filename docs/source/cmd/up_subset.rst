``uniprot-subset``
==================

Description of ``uniprot-subset``
*********************************

This program allows to create subset of data from an **UniProt** data file, applying plain text search and regular expression tests on the different entries.

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

+-----------------------+---------+-----------+----------------------------------------------------------+
| Argument              | Default | Mandatory | Description                                              |
+=======================+=========+===========+==========================================================+
| ``-i`` or |br|        | None    | Yes       | Input **UniProt** data file. It can be compressed or not.|
| ``-input``            |         |           |                                                          |
+-----------------------+---------+-----------+----------------------------------------------------------+
| ``-o`` or |br|        | None    | Yes       | Output **UniProt** subset data file.                     |
| ``-output``           |         |           |                                                          |
+-----------------------+---------+-----------+----------------------------------------------------------+
| ``-e`` or |br|        | None    | No        | Protein evidence keeping instruction. See details |br|   |
| ``-evidence-keep``    |         |           | section :ref:`Refine datasets <Refine datasets>`.        |
+-----------------------+---------+-----------+----------------------------------------------------------+
| ``-E`` or |br|        | None    | No        | Protein evidence skipping instruction. See details |br|  |
| ``-evidence-skip``    |         |           | section :ref:`Refine datasets <Refine datasets>`.        |
+-----------------------+---------+-----------+----------------------------------------------------------+
| ``-t`` or |br|        | None    | No        | Taxonomy keeping instruction. See details |br|           |
| ``-taxonomy-keep``    |         |           | section :ref:`Refine datasets <Refine datasets>`.        |
+-----------------------+---------+-----------+----------------------------------------------------------+
| ``-T`` or |br|        | None    | No        | Taxonomy skipping instruction. See details |br|          |
| ``-taxonomy-skip``    |         |           | section :ref:`Refine datasets <Refine datasets>`.        |
+-----------------------+---------+-----------+----------------------------------------------------------+
| ``-d`` or |br|        | None    | No        | Description keeping instruction. See details |br|        |
| ``-description-keep`` |         |           | section :ref:`Refine datasets <Refine datasets>`.        |
+-----------------------+---------+-----------+----------------------------------------------------------+
| ``-D`` or |br|        | None    | No        | Description skipping instruction. See details |br|       |
| ``-description-skip`` |         |           | section :ref:`Refine datasets <Refine datasets>`.        |
+-----------------------+---------+-----------+----------------------------------------------------------+
| ``-l`` or |br|        | 30      | No        | Minimal protein length to keep **UniProt** entries.      |
| ``-min-length``       |         |           |                                                          |
+-----------------------+---------+-----------+----------------------------------------------------------+


.. |br| raw:: html

    <br>