Refine datasets
###############

It is possible to use directly a complete dataset downloaded from **UniProt** (see the :ref:`Retrieve datasets <Retrieve datasets>` section).
However, working with a single large reference database will require a lot of memory and computation time.
In addition, it can result in less consistent annotations as these large datasets contain entries 
with variable annotation quality and from a wide range of organisms.

**go-FAnnoT** offers the possibility to split these large datasets into smaller ones according to 
different criteria (eg., taxonomy, annotation completeness, experimental evidence, etc.), and then
to build a hierarchy between these different sub-datasets.

Thus, for example, let's say that we have to annotation the protein coding genes of the
yeast species *Saccharomyces uvarum*. One possible dataset hierarchy could be:

#. All the **Swiss-Prot** entries from *Saccharomyces uvarum*,
#. All the **Swiss-Prot** entries from reference yeast *Saccharomyces cerevisiae*, strain S288c,
#. All the **Swiss-Prot** entries (excluding those from the two previous datasets) from the *Saccharomycetacea* family, **with experimental evidence**,
#. All the **Swiss-Prot** entries (excluding those from the three previous datasets) from the *Saccharomycetacea* family, **without experimental evidence**,
#. All the **TrEMBL** entries from *Saccharomycetacea* family.

Such a strategy will produce a more consistent and robust functional annotations.

**go-FAnnoT** provides two programs to refine datasets from **UniProt**: ``uniprot-subset`` and ``uniprot-prune``.

Filter datasets with ``uniprot-subset``
***************************************

The binary ``uniprot-subset`` allows to make selection/restriction on the entries of
**UniProt** datasets according 4 different type of information:

* Taxonomic lineage,
* Function, gene description,
* Level of evidence of protein existence (see the note below for details),
* Protein length.

.. note::

    The **level of evidence of protein existence** corresponds to the ``PE`` value indicated 
    in each entry. It consists in 5 distinct scores:

    #. Experimental evidence at protein level
    #. Experimental evidence at transcript level
    #. Protein inferred from homology
    #. Protein predicted
    #. Protein uncertain

    For more details, see `here <https://www.uniprot.org/help/protein_existence>`_.

Keep and/or skip
================

With ``uniprot-subset`` accepts both *keeping* and/or *skipping* instructions
for the taxonomy, the gene description and the protein existence evidence. The
corresponding arguments are the following:

* ``-t``: keeping instruction for taxonomy,
* ``-T``: skipping instruction for taxonomy,
* ``-d``: keeping instruction for gene description,
* ``-D``: skipping instruction for gene description,
* ``-e``: keeping instruction for protein existence evidence,
* ``-E``: skipping instruction for protein existence evidence.

Thus, for example, to extract all the entries associated to the genus *Saccharomyces*:

.. code-block:: console

    uniprot-subset -i fungi.dat.gz -o saccharomyces.dat.gz -t "Saccharomyces"

To exclude the species *S. cerevisiae* from the previous selection:

.. code-block:: console

    uniprot-subset -i saccharomyces.dat.gz -o sacch_no_cerevisiae.dat.gz -T "cerevisiae"

Obviously, it is possible to combine multiple *skipping* and *keeping* instruction:

.. code-block:: console

    uniprot-subset -i fungi.dat.gz -o my_selection.dat.gz \
        -t "Saccharomyces" \
        -T "S288c" \
        -d "lipase" \
        -E "5" \

In the above example, we select all the entries from the genus *Saccharomyces*, but
excluding those from the reference strain of *S. cerevisiae* S288c. This selection is
restricted to genes that have the word *lipase* in their description or function. Last,
entries with protein existence evidence score of 5 are discarded.

.. important::

    *Skipping* instructions are stronger than *keeping* instructions. It means that, 
    even if an entry fits the *keeping* instruction, if it is also concerned by the 
    *skipping* instruction, then it will be discard.

The power of regular expressions
================================

The *keeping* and *skipping* instructions presented above are applied through
regular expressions. Hence, input arguments of ``uniprot-subset`` can be 
regular expressions.

Here is a simple example:

.. code-block:: console

    uniprot-subset -i fungi.dat.gz -o my_selection.dat.gz -e "[12]"

The expression ``"[12]"`` means either the characters "1" or "2". In that case,
only the entries with protein existence evidence of 1 or 2 will be kept.
This is equivalent to:

.. code-block:: console

    uniprot-subset -i fungi.dat.gz -o my_selection.dat.gz -e "1|2"

The expression ``"1|2"`` is another way to match the characters "1" or "2".

However, the following instruction will return an empty output file:

.. code-block:: console

    uniprot-subset -i fungi.dat.gz -o my_selection.dat.gz -e "12"

The expression ``"12"`` simply means the value "12", and hence, no match will be found.

Regular expressions allow to build quite complex queries, such as:

.. code-block:: console

    uniprot-subset -i fungi.dat.gz \
        -t "Ascomycota" \ # Keep entries from Ascomytoca phylum
        -T "CBS|ACTT" \ # Discard strains from the CBS or the ACTT collections
        -d "iron|zinc" \ # Only include entries that deal with iron or zinc
        -E "[345]" \ # Exclude entries without experimental evidence

Minimal protein length
======================

By default ``uniprot-subset`` filter entries whose proteins are smaller than **30** amino acids.
We recommend to discard too small proteins, especially when they are from **TrEMBL**.

This parameter can be controlled with the argument ``-l``.

Prune datasets with ``uniprot-prune``
*************************************

The selected datasets obtained with ``uniprot-subset`` may contain a few spurious entries
such as partial genes or genes without function or description. To discard these entries,
we provide the script ``uniprot-prune``. Three different element can be considered to
prune proteins:

* Protein must start with a methionine (argument ``-m``),
* Protein must have a description (argument ``-d``),
* Protein must have a known function (argument ``-f``).

Here is the command to apply these filters on a dataset:

.. code-block:: console

    uniprot-prune -i proteins.dat.gz -o pruned.dat.gz -m -d -f

Pruning datasets generally result in more consistent annotation transfer, especially
when including data from **TrEMBL**.
