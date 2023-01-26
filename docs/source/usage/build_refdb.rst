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

        uniprot-create-refdb -i S288c.dat.gz -r S288c -d my_refdb -g \
            -desc "Saccharomyces cerevisiae S288c genes"

    * ``-i`` is for the input data file,
    * ``-r`` is the ID we want to give to the ``refdb``,
    * ``-d`` is the path to the directory that contains the different ``refdb``,
    * ``-g`` indicates that gene names from this datasets can be copied (boolean argument),
    * ``-desc`` is a short description of the ``refdb``.

#. Create the second ``refdb`` with the second dataset:

    .. code-block::

        uniprot-create-refdb -i Sacch_expval.dat.gz -r Sacch_expval -d my_refdb -g \
            -desc "Saccharomycetaceae family, experimental validation" -w 

    * ``-w`` indicates that entries from this dataset can **overwrite** matches found with entries from previsou datasets.

#. Create the last ``refdb`` with the third dataset:

    .. code-block::

        uniprot-create-refdb -i Sacch_trembl.dat.gz -r Sacch_trembl -d my_refdb \
            -desc "Saccharomycetaceae family, from TrEMBL" -u

    * ``-u`` indicated the annotations from this dataset are **unreviewed**.

At that step, the three ``refdb`` are created and can be used to run the main script of **go-FAnnoT**.
The different options ``-g``, ``-w`` and ``-u`` have to used according to the strategy 
chosen to find gene function. More details are given in the next section: :ref:`Formating rules <Formating rules>`.

A last option can be activated in a particuler case: if the genes we want to annotate are
possibly present in the dataset...