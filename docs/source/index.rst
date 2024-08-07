Welcome to go-FAnnoT's documentation!
=====================================

About
-----

**go-FAnnoT** is a Functional Annotation Transfer tool suite based on protein homology.
Briefly, the workflow consists in:

* Building consistent reference databases, extracting data from **UniProt** databases or from other resources.
* Comparing an input proteome (fasta file with multiple protein entries) to annotate against the selected reference databases, using both **local** and **global** sequence alignment tools.
* Formatting annotation information using customizable formatting rules.
* (Optionally) completing annotation with **InterProScan** predictions.
* Producing standard sequence annotation files (eg, **embl**, **genbank**).

It is written in `Go`. 

.. toctree::
   :hidden:
   :maxdepth: 2
   :caption: Get started

   ./get_started/principles.rst
   ./get_started/install.rst
   ./get_started/quick_start.rst
   ./get_started/release_notes.rst
   ./get_started/how_cite.rst
   ./get_started/bug_report.rst
   ./get_started/genomes.rst

.. toctree::
   :hidden:
   :maxdepth: 2
   :caption: General usage

   ./usage/retrieve_data.rst
   ./usage/refine_data.rst
   ./usage/build_refdb.rst
   ./usage/formatting_rules.rst
   ./usage/run_fannot.rst
   ./usage/copy_annot.rst

.. toctree::
   :hidden:
   :maxdepth: 2
   :caption: Cookbook

   ./cookbook/example.rst
   ./cookbook/create_fasta.rst
   ./cookbook/cpy_annot.rst

.. toctree::
   :hidden:
   :maxdepth: 2
   :caption: Command line arguments

   ./cmd/fannot_run.rst
   ./cmd/refdb_info.rst
   ./cmd/up_count.rst
   ./cmd/up_create.rst
   ./cmd/fasta_create.rst
   ./cmd/up_download.rst
   ./cmd/up_prune.rst
   ./cmd/up_split.rst
   ./cmd/up_subset.rst
   ./cmd/annot_seqfile.rst
   ./cmd/seqfile_fasta.rst
   ./cmd/add_comments.rst

