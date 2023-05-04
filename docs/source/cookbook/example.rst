Illustrative example
====================

To illustrate the functioning of **go-FAnnoT**, here is a complete case
example that consists in annotating a proteome from a yeast species.

Context
-------

We want to functionally annotate the genome of *Nakaseomyces glabratus* (formerly *Candida glabrata*), a yeast
that belongs to the *Saccharomycetaceae* family. The genus, *Nakaseomyces* is 
closely related genus from the model yeast *Saccharomyces cerevisiae*.

We have to types of files as input:

* **Sequence files**, in **EMBL** or **Genbank**, which contain the structural annotation obtained with a gene prediction tool such as **Augustus** or **Maker**. For the following example, let considers that these files are stored in a directory named ``SeqFile/``. The different sequence file paths will be ``SeqFile/Contig01.embl``, ``SeqFile/Contig02.embl``, and so on.
* **Proteome file**, in **Fasta**, which contain all the protein sequences that are structurally defined in the **Sequence files**. This file will be denoted ``proteins.fasta`` in the following sections.

.. important::

    To ensure a link between the proteins in ``proteins.fasta`` and the features defined in the different **sequence files**,
    it is important to name proteins using an existing **qualifier** from the **sequence files**. 
    We highly recommend to use the ``locus_tag`` qualifier.

There several ways to prepare such files. **go-FAnnoT** does not provide solution for that purpose, especially 
because no **EMBl/GenBank** parser are available in **Go**. It is quite easy to find programs to do that or
to develop script based on **BioPerl** or **BioPython**.

Step 1: Retrieve datasets
-------------------------

This preliminary step consists in download datasets from **UniProt**. To do so, we will use ``uniprot-download``.
To list the available taxonomic divisions, use the following command:

.. code-block::

    uniprot-download -D

This should display something similar to this list:

.. code-block::

    Available taxonomic divisions:
    -  invertebrates
    -  archaea
    -  plants
    -  unclassified
    -  mammals
    -  human
    -  rodents
    -  fungi
    -  viruses
    -  bacteria
    -  vertebrates

In our case, we are working on a fungi species, hence we will download data from this division.
First, we can download the fungi dataset from **SwissProt**:

.. code-block::

    uniprot-download -d fungi -s sprot

Then, the fungi dataset from **TrEMBL** (that contains unreviewed genes):

.. code-block::

    uniprot-download -d fungi -s trembl

These two commands will download **UniProt** data into directory named according to the version in **UniProt** data.
This version is usually a year and a month (format YYYY_MM). Hence, at the time of writing this tutorial, version was
**2023_01**, yielding to the following output directory: ``uniprot_v2023_01``.

This directory will contains two distinct files:
* ``uniprot_sprot_fungi.dat.gz`` (the **SwissProt** fungi dataset),
* ``uniprot_trembl_fungi.dat.gz`` (the **TrEMBL** fungi dataset).

.. note::

    If you the same download command twice in the same directory, then the program will not download anything
    if the **UniProt** version is the same.

Step 2: Define a data hierarchy
-------------------------------

The idea here is to design an ordered list of data subset built from the datasets retrieved in *step 1*.
The aim of this step is twofold:

* Consider first reference proteins that are more reliable or/and adapted to the species to annotate,
* Decrease computation time by limiting the size of reference databases.

For the genome we want to annotate in this example, here is a possible data hierarchy:

1. Proteins from **SwissProt**, associated to *Nakaseomyces glabratus*,
2. Proteins from **SwissProt**, associated to the model yeast *Saccharomyces cerevisiae* with experimental evidence of existence,
3. Proteins from **SwissProt**, associated to species from *Saccharomycetaceae* family, with experimental evidence, excluding *S. cerevisiae* and *N. glabratus*,
4. Proteins from **SwissProt**, associated to species from *Saccharomycetaceae* family, without experimental evidence,
5. Proteins from **TrEMBL**, associated to *Saccharomycetaceae* family.

.. important::

    There is no rule of thumb to build a dataset hierarchy. It is also possible to work directly with complete datasets from **UniProt** without hierarchy.
    We can suggest to follow these rules:

    * Avoid dataset with too few proteins (less than a few dozen),
    * Avoid to many layers in the hierarchy, this will be too complex to manage, more time consuming and probable useless,
    * Do no include entries from too distant taxa, this can only produce spurious annotation,
    * Be careful with **TrEMBL** datasets, they contains unreviewed data and hence they are less reliable. 

Step 3: Split **UniProt** datasets
----------------------------------

The datasets downloaded in *step 1* will be splitted into into sub-files considering the hierarchy defined in *step 2*.
For sake of simplicity, the five levels defined in *step 2* will be denoted ``dt01`` to ``dt05``.

The first selection concerns proteins from **SwissProt** associated to *Nakaseomyces glabratus* (*Candida glabrata*):

.. code-block::

    uniprot-subset                     \
        -i uniprot_sprot_fungi.dat.gz  \
        -o dt01.dat.gz                 \
        -t "Candida glabrata"

This will create the file ``dt01.dat.gz`` and return the number of entries selected.

Then, the second selection includes the entries associated to *S. cerevisiae* whose evidence of existence have experimentally
validated either at the protein level or the transcript level. Here is the command line to create ``dt02.dat.gz``:

.. code-block::

    uniprot-subset                    \
        -i uniprot_sprot_fungi.dat.gz \
        -o dt02.dat.gz                \
        -t "cerevisiae"               \
        -e "1|2"

The third selection consists in all the entries associated to *Saccharomycetaceae* family, with experimental evidence
but excluding *S. cerevisiae* and *N. glabratus*. The file ``dt03.dat.gz`` is obtained as follow:

.. code-block::

    uniprot-subset                    \
        -i uniprot_sprot_fungi.dat.gz \
        -o dt03.dat.gz                \
        -t "Saccharomycetaceae"       \
        -T "cerevisiae|glabrata"      \
        -e "1|2"

The fourth selection contains entries associated to *Saccharomycetaceae* family, without experimental evidence.
we can exclude entries from *N. glabratus* as they are already included in the first selection. The 
following instruction will produce the file ``dt04.dat.gz``:

.. code-block::

    uniprot-subset                    \
        -i uniprot_sprot_fungi.dat.gz \
        -o dt04.dat.gz                \
        -t "Saccharomycetaceae"       \
        -T "glabrata"                 \
        -E "1|2"

The last selection, from **TrEMBL**, contains all the entries from *Saccharomycetaceae* species.
Here is the command line to obtain ``dt05.dat.gz``:

.. code-block::

    uniprot-subset                     \
        -i uniprot_trembl_fungi.dat.gz \
        -o dt05.dat.gz                 \
        -t "Saccharomycetaceae"        \

Step 4: Prune the selected entries
----------------------------------

All the entries selected in the previous step may contain uncompleted annotations, pseudo-genes, and so on.
In order to avoid the transfer of spurious/inconsistent annotations, we recommend to use the ``uniprot-prune``
program.

To *prune* the 5 previously generated data files, it is possible to use the following **BASH** loop:

.. code-block::

    for i in $(seq 1 5)
    do
        uniprot-prune -i dt0${i}.dat.gz -o dt0${i}_pruned.dat.gz -m -d -f
    done

This will create the *pruned* files denoted ``dt01_pruned.dat.gz`` to ``dt05_pruned.dat.gz``.

Step 5: Create the different ``refdbs``
---------------------------------------

The ``refdbs`` are built from the different data selection performed in the previous steps
to be used directly by the main program of **go-FAnnoT**. They are stored in a dedicated 
directory, for example ``my_refdbs/``, an consist in a collection of different files, 
including a **Fasta** of the proteins they contain, **BLAST** database files and a configuration
file in **JSON**. To create them, use the program ``uniprot-create-refdb``:

.. code-block::

    uniprot-create-refdb                 \
        -i dt01_pruned.dat.gz            \
        -r dt01                          \
        -d my_refdbs/                    \
        -D "Candida glabrata, SwissProt" \
        -e

This command creates the ``refdb`` with the ID ``dt01`` in the directory ``my_refdbs``. The boolean
arguement ``-e`` indicate that this ``refdb`` may contains gene that are present in the genome
to annotate (i.e., at least from same species).

The four other ``refdbs`` have to be created the same way. Here are the instruction for ``dt02`` and ``dt03``:

.. code-block::

    uniprot-create-refdb                                    \
        -i dt02_pruned.dat.gz                               \
        -r dt02                                             \
        -d my_refdbs/                                       \
        -D "Saccharomyces cerevisiae, exp. val., SwissProt" \
        -w

    uniprot-create-refdb                                     \
        -i dt03_pruned.dat.gz                                \
        -r dt03                                              \
        -d my_refdbs/                                        \
        -D "Saccharomycetaceae family, exp. val., SwissProt" \
        -w

These two ``refdbs`` are created with the option ``-w`` meaning that entries from them can be used
to overwrite a hit from a previous ``refdb`` with a lower similarity level.

The following command is used to generate ``dt04``:

.. code-block::

    uniprot-create-refdb                                    \
        -i dt04_pruned.dat.gz                               \
        -r dt04                                             \
        -d my_refdbs/                                       \
        -D "Saccharomycetaceae family, no. val., SwissProt" \

And the last one, for ``dt5``:

.. code-block::

    uniprot-create-refdb                                    \
        -i dt05_pruned.dat.gz                               \
        -r dt05                                             \
        -d my_refdbs/                                       \
        -D "Saccharomycetaceae family, unreviewed, TrEMBL"  \
        -u

Here, the boolean arguement ``-u`` indicates that this ``refdb`` contains unreviewed entries.

Hence, with these 5 command lines, 5 ``refdbs`` denoted ``dt01`` to ``dt05`` have been created
in the directory ``my_refdbs/``.

Step 6: Run **go-FAnnoT** main program
---------------------------------------

This step consists in finding for each protein in the query file (``proteins.fasta``) a best hit
among the different ``refdbs``, taking into account their hierarchy (``dt01`` > ``dt02`` > ... > ``dt05``)
as well as the possible options such as ``-e`` and ``-w`` (see step 5).

.. code-block::

    fannot-run                      \
        -i proteins.fasta           \
        -o annotations.tsv          \
        -r dt01,dt02,dt03,dt04,dt05 \
        -d my_refdbs/               

The output of this program is a tabulated text file containing the predicted functional annotations (see details :ref:`here <The output file>`).

Step 7: Check and edit ``annotations.tsv``
------------------------------------------

This step is optional, but we highly recommend to check the output file ``annotations.tsv`` before copying
the functional annotations in the **sequence files**. This step can reveal that there are issues in the annotation templating,
that some thresholds have to be refined or that the selection of ``refdbs`` have to be amended.

Step 8: Copy annotations into **sequence files**
------------------------------------------------

This is the final step. The functional annotations reported in ``annotations.tsv`` 