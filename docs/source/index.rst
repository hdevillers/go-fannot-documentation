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
   :caption: Detailed usage

   ./usage/retrieve_data.rst
