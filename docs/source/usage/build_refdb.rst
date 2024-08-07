Build reference databases
#########################

Once the different required datasets are generated (see :ref:`Refine datasets <Refine datasets>` section),
they have to be converted into reference databases (``refdb``). It is structure containing different
data files and a configuration file that a use by the main script of **go-FAnnoT** to find 
functional annotations (see :ref:`Find functional annotations <Find functional annotations>` section).
Generating ``refdb`` from **UniProt** datasets is the job of the script ``uniprot-create-refdb``.

To illustrate its use, let's consider the three following datasets:

* ``S288c.dat.gz``: proteins from *Saccharomyces cerevisiae*,
* ``Sacch_expval.dat.gz``: proteins from *Saccharomycetaceae* family, with experimental evidence,
* ``Sacch_trembl.dat.gz``: proteins from *Saccharomycetaceae* family, from **TrEMBL**.

Here are the steps to build the three corresponding ``refdb``;

#. First, create a directory that will contain the different ``refdb``:

    ..  code-block::

        mkdir my_refdb

#. Create a first ``refdb`` with the first dataset:

    .. code-block::

        uniprot-create-refdb -i S288c.dat.gz -r S288c -d my_refdb \
            -D "Saccharomyces cerevisiae S288c genes"

    * ``-i`` is for the input data file,
    * ``-r`` is the ID we want to give to the ``refdb``,
    * ``-d`` is the path to the directory that contains the different ``refdb``,
    * ``-D`` is a short description of the ``refdb``.

#. Create the second ``refdb`` with the second dataset:

    .. code-block::

        uniprot-create-refdb -i Sacch_expval.dat.gz -r Sacch_expval -d my_refdb \
            -D "Saccharomycetaceae family, experimental validation" -w 

    * ``-w`` indicates that entries from this dataset can **overwrite** matches found with entries from previous datasets.

#. Create the last ``refdb`` with the third dataset:

    .. code-block::

        uniprot-create-refdb -i Sacch_trembl.dat.gz -r Sacch_trembl -d my_refdb \
            -D "Saccharomycetaceae family, from TrEMBL" -u

    * ``-u`` indicated the annotations from this dataset are **unreviewed**.

At that step, the three ``refdb`` are created and can be used to run the main script of **go-FAnnoT**.

.. important::
    The different options ``-w`` and ``-u`` have to be used according to the strategy 
    chosen to find gene functions. More details are given in the next section: :ref:`Formatting rules <Formatting rules>`.

A last option can be activated in a particular case: if the genes we want to annotate are
possibly present in the retrieved dataset. This can occur, for example, if specific genes have been
published before the release of the complete genome annotation. In that case, ``uniprot-create-refdb``
should be run with the boolean argument ``-e`` (or ``-equal``). With that option, all the input genes that
have a 100% identity match with a ``refdb`` entry will be directly linked to the corresponding **UniProt** entry.

