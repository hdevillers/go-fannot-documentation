``fannot-run``
==============

Description of ``fannot-run``
*****************************

This is the main program that runs the different proteins alignments, identifying the putative best hits between
input proteins and the different ``refdbs``.

Usage of ``fannot-run``
***********************

.. code-block::

    fannot-run                  \
        -i proteins.fasta       \
        -o annotations.tsv      \
        -r refdb1,refdb2,refdb3 \
        -d refdb_dir/           

Arguments of ``fannot-run``
***************************

+----------------+---------+-----------+----------------------------------------------------------+
| Argument       | Default | Mandatory | Description                                              |
+================+=========+===========+==========================================================+
| ``-i`` or |br| | None    | Yes       | Input **fasta** file containing the protein to annotate. |
| ``-input``     |         |           |                                                          |
+----------------+---------+-----------+----------------------------------------------------------+
| ``-o`` or |br| | None    | Yes       | Output file names (tabulated text file).                 |
| ``-output``    |         |           |                                                          |
+----------------+---------+-----------+----------------------------------------------------------+
| ``-r`` or |br| | None    | Yes       | Coma separated list of ``refdb`` IDs.                    |
| ``-refdb-id``  |         |           |                                                          |
+----------------+---------+-----------+----------------------------------------------------------+
| ``-d`` or |br| | None    | Yes       | Directory that contains the ``refdbs``.                  |
| ``-refdb-dir`` |         |           |                                                          |
+----------------+---------+-----------+----------------------------------------------------------+
| ``-rules``     | None    | No        | Custom parameters (**JSON**).                            |
+----------------+---------+-----------+----------------------------------------------------------+
| ``-ips``       | None    | No        | Tabulated text file from **InterProScan**.               |
+----------------+---------+-----------+----------------------------------------------------------+
| ``-t`` or |br| | 4       | No        | Number of threads.                                       |
| ``-threads``   |         |           |                                                          |
+----------------+---------+-----------+----------------------------------------------------------+

.. |br| raw:: html

    <br>
