Installing go-FAnnoT
====================

Requierments
------------

**go-FAnnoT** requires two third-party tools:

* **NCBI-BLAST+** tool suite,
* **EMBOSS** tool suite.

.. note::
    These tools are included in the **Conda** recipe, and hence users
    do not have to install them if they use **Conda** to install **go-FAnnoT**.

Binaries of these tools can be directly downloaded from their official
Websites (links: `NCBI-BLAST+ <https://blast.ncbi.nlm.nih.gov/doc/blast-help/downloadblastdata.html>`_; 
`EMBOSS <https://emboss.sourceforge.net/download/>`_) or, on linux systems, they can be installed
via most of the recent package manager:

.. code-block:: console

    # Example with Ubuntu
    apt install ncbi-blast+ emboss

**go-FAnnoT** workflow optionaly includes results from **InterProScan**.

Using the **Conda** recipe
--------------------------

Just create a **Conda** environment and run the recipe from **BioConda** channel:

.. code-block:: console

    # Create the dedicated environment
    conda create -n go-FAnnoT

    # Activate it
    conda activate go-FAnnoT

    # Run the recipe
    conda install -c bioconda go-fannot


Build from sources
------------------

To build **go-FAnnoT** from source, you must have **Go** installed.
See official instruction `here <https://go.dev/doc/install>`_.

Then, get the source code from our `Github repository <https://github.com/hdevillers/go-fannot>`_:

.. code-block:: console

    # Clone the repository
    git clone git@github.com:hdevillers/go-fannot.git

Enter the clone repositon and run ``make`` to build binaries:

.. code-block:: console

    cd go-fannot/
    make

    # Run test (optional)
    make test

    # Install binaries
    make install

By default, binaries a installed in 