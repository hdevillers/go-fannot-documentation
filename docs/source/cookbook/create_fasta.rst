Create your own reference **Fasta**
===================================

It is possible to build a ``refdb`` directly from a **Fasta** file instead 
of a **DAT** file from **UniProt**.

Preparing the **Fasta** file
----------------------------

To prepare your reference **Fasta** file, two possible options are available
to provide functional information about genes:

* Use formatted sequence descriptions,
* Use regular sequence descriptions.

Formatted sequence descriptions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In the **Fasta** format, the sequence description comes directly after the identifier of
the sequence, right after the first space of the line:

.. code-block::

    >Identifier Description of the sequence
    MVDLLSADFAPPTFQLKSIHFTMARQFFVGGNFKMNGTKDSVTKIIEGLNSADLPSNVQV
    VIAPPAPYLGLAVEVNKQSTVAISAQNVYDKASGAYTGEISANQVLDLGATWTLTGHSER
    RTLIKESDEFIAEKTKFAIEQGLSVILCIGETLEERKAGITLDVCA...

We propose a solution to format this description line in order to provide values to the deferent **fields**
defined in our template system (see section :ref:`Qualifier templates <Qualifier templates>`).
**Field** information are provided in the **Fasta** sequence description in a specific order, each **field**
being separated by double colons (``::``):

.. code-block::

    >Identifier ShortDesc::GeneName::LocusTag::Species::LongDesc

Here is a concrete example:

.. code-block::

    >Q99288 road-range acid phosphatase Det1p::DET1::YDR051C::Saccharomyces cerevisiae::metal-independent, broad-range acid phosphatase...
    MGKPKYILLVRHGESEGNLDKTVNSVTANHLIPLTSTGHKQAQDAGKVLRQFLNQDILQC
    SKGQKKQSILFYTSPYKRARQTCDNIIDGIKDVDGVEYSVKEEPRMREQDFGNFQSTTEE
    MEKIWEERAHYGHFFFRIPHGESAADVYDRISSFNESLFRQFQNPDFP...

This way, all these different information can be used to build annotation template (see :ref:`Formatting rules <Formatting rules>`)
like when using **UniProt** data. Note that when using these formatted sequence descriptions, it is also possible
to mix ``refdbs`` built from **Fasta** with ones built from **UniProt** data using the same :ref:`Formatting rules <Formatting rules>`.

Regular sequence descriptions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For sake of simplicity, it is also possible to provide a simple description of the protein function 
directly in the **Fasta** sequence description.

.. code-block::

    >XXXX00188 similar to uniprot|P38426 Saccharomyces cerevisiae TPS3, regulatory subunit of...
    MTVIIGSLFLPYTVQFEVGPQDSIPVDEAVPPQSHLRKRSSGTGVRPMPIRQSSILPSLS
    IANSHQQSPTSTPKPHEFRRRSSAHEGGQSVEDFFLKRIDEENNKEVGEIFAENLKEKLL
    PQYIQPKSKINVKDGSVVDLKKVNDDLGLPALKISR...

When defining directly a function description like in the above example, the value is associated to
the **field** ``LongDesc``. The other fields will be empty (see :ref:`Qualifier templates <Qualifier templates>`).
Hence, the **JSON** file that defines the annotation template has to be modified accordingly:

.. code-block::

    {
        "DefaultNote": "hypothetical protein",
        "DefaultProduct": "hypothetical protein",
        "DefaultGeneName": "",
        "DefaultFunction": "",
        "TemplateNote": "{LongDesc}",
        "TemplateProduct": "",
        "TemplateGeneName": "",
        "TemplateFunction": "",
        ...

Build a ``refdb`` from a **Fasta** file
---------------------------------------

Once you have your **Fasta**, you can simply create a ``refdb`` using the following command line:

.. code-block:: bash

    fasta-create-refdb -i custom_ref.fasta -r CUSTOM_REF -d my_refdb \
            -D "Set of custom reference annotations"
    
Note that excepting the input which is a **Fasta** file instead of a **UniProt** data file, 
all the arguments of this command are identical to those of ``uniprot-create-refdb`` 
(see section :ref:`Build reference databases <Build reference databases>`).