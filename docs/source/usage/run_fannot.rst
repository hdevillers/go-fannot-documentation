Find functional annotations
===========================

This section concerns the main program of **go-FAnnoT** tool suite, which is ``fannot-run``.
This command controls the algorithm that investigates the different ``refdbs`` for each 
input proteins to find putative functions.

Prerequisite
************

The main input of ``fannot-run`` is a **fasta** file that contains the proteins from the genes that have to be annotated.
The names (IDs) of these proteins must be unique. If different proteins share the same name, collisions may occure during
the last step of the process, when rewritting final annotation files.

In addition, the protein names in the **fasta** file should refer to an existing **qualifier** in the current annotation
file, such as ``/locus_tag``, ``/protein_id`` or ``/standard_name`` 