Retrieve data
=============

The transfer of functional annotations require high quality reference datasets.
While it is possible to build and use your own reference datasets, **go-FAnnoT** workflow 
has been design to use **UniProt** resources as reference.

Download from **UniProt** website
---------------------------------

Directly from the **UniProt** website, at the `download <https://www.uniprot.org/help/downloads#uniprotkblink>`_, 
you can download the full **Swiss-Prot** dataset and/or the full **TrEMBL** dataset.

.. important::

    **go-FAnnoT** tools require the **text** version of the datasets (format ``dat.gz``).

These full datasets represent very large files that include all the taxa while in most cases 
only a limited number of taxa/species are required to annotate a genome. Hence, it is possible to 
download different subsets of data from the **UniProt** 
`ftp <https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/>`_.


Download with ``uniprot-download``
----------------------------------

**go-FAnnoT** comes with a script (``uniprot-download``) dedicated to download data from **UniProt**
for a given taxonomic division. Hence, for example, to downlaod **Swiss-Prot** entries for 
**fungi** organisms:

.. code-block:: console

    uniprot-download -s sprot -d fungi

The script will retrieve the ``metalink`` file from **UniProt**, get the apropriate 
link that corresponds to the queried division as well as the ``md5`` checksum of the file.
The downlaoded file will be stored in a directory named by the current release number of **UniProt**.
Once downloaded, ``md5`` will be checked.

To list all the available taxonomic divisions, you can use the ``-D`` argument:

.. code-block:: console

    uniprot-download -D

There are three possible download mirrors (``us`` (default), ``uk`` and ``ch``). 
To swith between mirrors, use the ``-m`` argument:

.. code-block:: console

    uniprot-download -m uk -s trembl -d archaea

This command allows to download **TrEMBL** entries for **Archaea** organisms via the ``uk`` mirror.

.. note::

    If the checksum fails, we recommend to try another mirror. You can use the argument ``-M`` to 
    list available mirrors. Otherwise, it is possible to skip the checksum with the argument ``-skip-sum``.
