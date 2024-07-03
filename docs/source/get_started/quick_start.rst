Quick start
===========

In this section is provided a short illustration of the functioning of **go-FAnnoT**. For a 
more complete illustration, please see the :ref:`Illustrative example <Illustrative example>`
from the **COOKBOOk** section or to have comprehensive explanations about
every steps of the pipeline, see section **GENERAL USAGE**.

The starting point of this *quick start* example is a **Fasta** file that
contains the proteins you want to functionally annotate: ``proteins.fasta``.
Let says these proteins are from a fungi organism of the *Saccharomycetaceae* family.

Here are the command lines to run to get functional annotations for each proteins 
from the ``proteins.fasta`` file:

.. code-block:: bash

    # Download fungi data from SwissProt
    uniprot-download -d fungi -s sprot

    # Enter into the folder that contain the downloaded data file
    # Note that the version of uniprot will be different
    cd unprot_v2023_01

    # Create a subset of the downloaded file that contain only
    # proteins whose existence have been experimentally validated
    # and that belong to an organism from Saccharomycetaceae family
    uniprot-subset -i uniprot_sprot_fungi.dat.gz -o SACC_VAL.dat.gz \
        -t Saccharomycetaceae -e "1|2"

    # Create a second subset without experimental evidence of existence
    uniprot-subset -i uniprot_sprot_fungi.dat.gz -o SACC_NOVAL.dat.gz \
        -t Saccharomycetaceae -e "3|4"

    # Prune the two subsets (remove spurious or non informative proteins)
    uniprot-prune -i SACC_VAL.dat.gz -o SACC_VAL_pruned.dat.gz -m -d
    uniprot-prune -i SACC_NOVAL.dat.gz -o SACC_NOVAL_pruned.dat.gz -m -d

    # Create a directory to contain reference databases
    mkdir refdbs

    # Create the two reference databases
    uniprot-create-refdb -i SACC_VAL_pruned.dat.gz -r SACC_VAL -d refdbs/
    uniprot-create-refdb -i SACC_NOVAL_pruned.dat.gz -r SACC_NOVAL -d refdbs/

    # Run the main programme to get functional annotations
    fannot-run -i proteins.fasta -o annotations.tsv -r SACC_VAL,SACC_NOVAL -d refdbs/

With these command lines, you will obtain a tabulated file with the predicted 
functional annotations for all the proteins of the ``proteins.fasta`` file.
These protein functions were preferably retrieved from the set of reference proteins that
come from *Saccharomycetaceae* organisms and that have experimental evidence of existence.
Functional annotation formatting will be the default one. 

To copy these annotations into annotated genome files, such as **EMBL** files, you
can run the **Python** script:

.. code-block:: bash

    AnnotationsToSeqFiles.py -a annotations.tsv -s "*.embl" -o NEW_EMBL/ -c 2

Obviously, **go-FAnnoT** offers a lot of options and possibilities to custom 
your functional annotations. Please, visit the other articles of this
documentation for a complete overview of the capabilities of our tool.