Complete example
================

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

