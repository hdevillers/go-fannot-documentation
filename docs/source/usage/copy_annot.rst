Copy annotation in sequence files
=================================

This final step consists in copying the generated annotation from the tabulated file (output of ``fannot-run`` programme), into
existing **EMBL** or **GenBank** files.

.. note::

    Currently, this script is not written in Go but in Python as no **EMBL/GenBank** parser is available
    in Go.

The command line to use is the following:

.. code-block::

    AnnotationsToSeqFiles.py            \
        -a annotations.tsv              \
        -s <input_annotation_sequences> \
        -o output_direcory              \
        -c 2

Where ``annotations.tsv`` is the output of ``fannot-run`` programme and ``<input_annotation_sequences>`` corresponds
to the initial annotated sequence(s). It can be a single file, a directory path containing only sequence files or a
*pattern* path such as ``"mydata/*.embl"``.

Here is the details of all the command arguments:

* ``-a``: Input annotation file from ``fannot-run``,
* ``-s``: Input annotation sequence file(s),
* ``-f``: Input sequence file format, can be **embl** or **genbank** [default: **embl**],
* ``-o``: Output directory [default: ./],
* ``-F``: Output sequence file format, can be **embl** or **genbank** [default: **embl**],
* ``-t``: Feature type targeted to write annotations [default: CDS],
* ``-i``: Qualifier type used to name proteins [default: locus_tag],
* ``-k``: Keep previous annotations [default: false (i.e., qualifiers listed in argument ``-q`` are erased.)],
* ``-q``: List of qualifier to transfer (among **note**, **product**, **gene** and **function**), coma separator [default: note,product,gene],
* ``-c``: Minimal hit status to copy gene names [default: 1].

.. important::

    The minimal hit status refers to the ``Hit_sta`` parameters from the different matching rules, see :ref:`Formatting rule <Formatting rules>`).
    For example, the default matching rules consist of two levels, *highly similar* with a hit status of **2** and *similar* with a hit status
    of **1**. If we consider that it is necessary to have a *highly similar* match to allow the transfer of the gene name, then the argument 
    ``-c`` of the ``AnnotationsToSeqFiles.py`` has to be set to 2.