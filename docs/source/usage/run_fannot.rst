Find functional annotations
===========================

This section concerns the main program of **go-FAnnoT** tool suite, which is ``fannot-run``.
This command controls the algorithm that investigates the different ``refdbs`` for each 
input proteins to find putative functions.

Prerequisite
************

**Fasta** file of proteins
--------------------------

The main input of ``fannot-run`` is a **fasta** file that contains the proteins from the genes that have to be annotated.

.. important::

    * The names (IDs) of these proteins must be unique. If different proteins share the same name, collisions may occure during the last step of the process, when rewritting final annotation files.
    * These IDs should refer to an existing **qualifier** in the source annotation file, such as ``/locus_tag``, ``/protein_id`` or ``/standard_name``.

List of ``refdbs``
------------------

The second requierment of ``fannot-run`` is a list of ``refdbs`` previouly built with the ``uniprot-create-refdb`` program. These ``refdbs`` are 
identified by their IDs, which correspond to the ``-r`` argument value of ``uniprot-create-refdb``. They are provided as a coma-separated list
to the the ``fannot-run`` program (see examples next section).

**InterProScan** outputs (optional)
-----------------------------------

**go-FAnnoT** can handle **InterProScan** domain predictions to enrich functional annotations for proteins that do not have significant homology in the
investigated ``refdbs``. We do not provide an **InterProScan** wrapper. It has to be done separately (see the `InterProScan website <https://www.ebi.ac.uk/interpro/download/InterProScan/>`_).
The ``fannot-run`` script can read the standard ``TSV`` output of **InterProScan**.

A custom configuration file (optional)
--------------------------------------

The ``fannot-run`` program has a default configuration and hence, it is not necessary to provide a **JSON** file if the default configurations fit your needs.
To write a custom configuration **JSON** file, check the :ref:`Formating rule <Formating rules>` section for details.

Running ``fannot-run``
**********************

Below is an example of command line for the ``fannot-run`` program:

.. code-block::

    fannot-run -i proteins.fasta \
        -r refdb1,refdb2,refdb3  \
        -d refdb_dir             \
        -o annotations.tsv       \
        -rules config.json       \
        -ips ips_output.tsv      \
        -t 6

where the different arguments correspond to:

* ``-i``: input **fasta** of the protein to annotate (**mandatory**),
* ``-r``: coma-separated list of ``refdb`` IDs (**mandatory**),
* ``-d``: path of the directory containing the different ``refdbs`` (**mandatory**),
* ``-o``: output file name, a tabulated text file (**TSV**) (**mandatory**),
* ``-rules``: custom configuration file (**JSON**),
* ``-ips``: InterProScan prediction file (**TSV**),
* ``-t``: Number of threads [default: 4].

The output file
***************

The ``fannot-run`` program provides one output file (``-o``), which is a tabulated text file. Headers of this file are the following:

* **GeneID**: ID of the input protein,
* **Product**: Formatted product qualifier annotation,
* **Note**: Formatted note qualifier annotation,
* **Function**: Formatted function qualifier annotation,
* **Organism**: Organism name (scientific name),
* **RefID**: Accession of the protein retrieved as best hit in ``refdbs``,
* **RefLocus**: Locus name of the best hit,
* **RefName**: Gene name of the best hit,
* **IPSID**: InterProScan Accession number(s) (coma separator), 
* **IPSAnnot**: InterProScan domain description(s) (coma separator),
* **Status**: Hit status (referring to ``Hit_sta`` value, see :ref:`Formating rule <Formating rules>`),
* **Similarity**: Protein similarity computed on from the global alignment,
* **LengthRatio**: Protein length ratio,
* **DBID**: ID of the ``refdb`` that contains the best hit,
* **HitNum**: The Blast hit number kept (referring to the parameter ``NbHitCheck``, see :ref:`Formating rule <Formating rules>`),
* **OverWritten**: Indicates if the retained hit has overwritten another hit.

.. important::

    Before copying these generated annotation in the final annotation files (see :ref:`Copy annotation in sequence files <Copy annotation in sequence files>`),
    we highly recommand an inspection of the tabulated file to check possible issues or to evaluate if matching parameters have to be refined. It
    is also possible to edit manually specific annotations if required.
