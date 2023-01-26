Welcome to go-FAnnoT's documentation!
=====================================

About
-----

**go-FAnnoT** is a Functional Annotation Transfer tool suite based on protein homology.
Briefly, the workflow consists in:

* Building consistant reference databases, extracting data from **UniProt** databases or from other resources.
* Comparing an input proteom (fasta file with multiple protein entries) to annotate againts the selected reference dabases, using both **local** and **global** sequence alignment tools.
* Formating annotation information using customizable formating rules.
* (Optionaly) completing annotation with **InterProScan** predictions.
* Producing standard sequence annotation files (eg, **embl**, **genbank**).

It is written in `Go`. 

.. toctree::
   :hidden:
   :maxdepth: 2
   :caption: Get started

   ./get_started/install.rst

.. toctree::
   :hidden:
   :maxdepth: 2
   :caption: General usage

   ./usage/retrieve_data.rst
   ./usage/refine_data.rst
   ./usage/build_refdb.rst
   ./usage/formating_rules.rst
   ./usage/run_fannot.rst
   ./usage/copy_annot.rst

.. toctree::
   :hidden:
   :maxdepth: 2
   :caption: Command line arguments

   ./cmd/fannot_run.rst
   ./cmd/refdb_info.rst
   ./cmd/up_count.rst
   ./cmd/up_create.rst
   ./cmd/up_download.rst
   ./cmd/up_prune.rst
   ./cmd/up_split.rst
   ./cmd/up_subset.rst

