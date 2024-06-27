``SeqFilesToRefFasta.py``
============================

Description of ``SeqFilesToRefFasta.py``
*******************************************

This program extract from sequence files in **EMBL** or **GenBank** format all the proteins as well as the associated functional annotations a create a reference **Fasta** file.

Usage of ``SeqFilesToRefFasta.py``
*************************************

.. code-block::

    SeqFilesToRefFasta.py    \
        -s "data/*.embl"     \
        -o reference.fasta


Arguments of ``SeqFilesToRefFasta.py``
*****************************************

+-------------------------+---------------+-----------+----------------------------------------------------------+
| Argument                | Default       | Mandatory | Description                                              |
+=========================+===============+===========+==========================================================+
| ``-s`` or |br|          | None          | Yes       | Path to sequence files. It can be a single file, |br|    |
| ``--seq-files``         |               |           | a directory or a pattern with "\*".                      |
+-------------------------+---------------+-----------+----------------------------------------------------------+
| ``-f`` or |br|          | embl          | No        | Input sequence file format (**embl** or |br|             |
| ``--in-format``         |               |           | **genbank**).                                            |
+-------------------------+---------------+-----------+----------------------------------------------------------+
| ``-o`` or |br|          | ./            | No        | Output **Fasta** filename.                               |
| ``--output``            |               |           |                                                          |
+-------------------------+---------------+-----------+----------------------------------------------------------+
| ``-i`` or |br|          | locus_tag     | No        | Qualifier used to identify CDS.                          |
| ``--id-qualifier``      |               |           |                                                          |
+-------------------------+---------------+-----------+----------------------------------------------------------+


.. |br| raw:: html

    <br>