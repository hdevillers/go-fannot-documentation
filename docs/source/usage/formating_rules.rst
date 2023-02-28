Formating rules
###############

This section deals with how the annotations will be formated in the final output.
**go-FAnnoT** comes with default formating rules. However, these rules are highly
customizable via a configuration file (in ``json`` format).

Before presenting this ``json`` and how to custom it, it is necessary to introduce 
the concept of ``feature`` and ``qualifier``. These two objects are used to define 
and describe genetique object in the genome annotation. The official nomenclature 
is available on the `INSDC <https://www.insdc.org/submitting-standards/feature-table/>`_.
Briefly, a ``feature`` corresponds to one genetique object that is characterized 
by a type (eg., *mRNA*, *CDS*, *tRNA*, etc.), by coordinates along a sequence and
by a collection of ``qualifiers`` that are used to describe its different characteristics,
such as */function*, */product*, */gene_name* and so on.

The current version of **go-FAnnoT** completes annotations of ``CDS`` features, which
indicate in annotated genomes the exact position of protein coding sequences. 
It allows to fill 4 different ``qualifiers``:

* ``/note``: Can contain any comment about or additional information,
* ``/product``: Is the name of the polypeptide associated to the ``CDS``,
* ``/gene``: Gene name symbol (i.e., short version),
* ``/function``: Function attributed to a locus.

Construction of the ``json`` configuration file is split into 4 parts:

#. Qualifier default values
#. Qualifier templates
#. Algorithm options
#. Matching rules

Qualifier default values
************************

The first part of the ``json`` concerns the definition of the default values
for the 4 ``qualifiers``. Default values are used when no match is found between
a queried proteins and all the entries of the different ``refdb``.
Here is the default values used by **go-FAnnoT** from the default ``json`` file:

.. code-block::

    {
        "DefaultNote": "hypothetical protein",
        "DefaultProduct": "hypothetical protein",
        "DefaultGeneName": "",
        "DefaultFunction": "",
        ...
    }

Hence, if a input protein has no hit in the different ``refdb``, then it 
annotation will be:

.. code-block:: console

    /note="hypothetical protein"
    /product="hypothetical protein"

.. important::

    If the value return by **go-FAnnoT** for a given ``qualifier`` is an empty string (``""``)
    then the qualifier will not be reported in the final annotation file.

Qualifier templates
*******************

**go-FAnnoT** proposes a templating syntaxe to format the different information retrieved 
from the matching entry of a ``refdb``. Thus, for example, the default template for the gene name
qualifier (``/gene``) is:

.. code-block::

    {
        ...
        "TemplateGeneName": "{GeneName}",
        ...
    }

``{GeneName}`` is a **field** that will be replaced by the value retrieved from the matching 
hit from a ``refdb``, in that present case, this corresponds to the gene name.

.. code-block::

    /gene="ACT1"

The current version of **go-FAnnoT** proposes the following **fileds** to custom annotation templates:

* ``{DbName}``: Name of the source database (label in ``refdb`` object),
* ``{DbId}``: Accession Id of the matching entry,
* ``{ShortDesc}``: Is a short description of the gene found at the ``DE`` lines of **UniProt** entries,
* ``{LongDesc}``: Is a long description of the gene found in the ``FUNCTION`` section at the ``CC`` lines of **UniProt** entries,
* ``{GeneName}``: Is the gene name of the matching entry,
* ``{ProteinName}``: Is the protein name of the matching entry,
* ``{LocusTag}``: Is the locus tag (Id) of the matching entry,
* ``{Species}``: Is the species of origin of the matching entry,
* ``{Prefix}``: Is the matching rule prefix (see below the :ref:`Matching rules <Matching rules>` section)
* ``{Unreviewed}``: Is a string that is added when the matching entry is from a ``refdb`` that have the label *unreviewed* (see :ref:`Build reference databases <Build reference databases>` page).

.. note::

    * If a value associated to a **field** is empty, meaning that the information is missing from the ``refdb`` entry, then an empty string (``""``) is returned in the template.
    * If the ``refdb`` entry has no ``FUNCTION`` section, then ``{LongDesc}`` **field** will be a copy of ``{ShortDesc}``.
    * If the ``refdb`` entry has no ``FUNCTION`` section and no ``DE`` lines, then no descriptive text will be available. To avoid this situation, use ``uniprot-prune`` script.

Obviouly, it possible to combine several fields in a single template:

.. code-block::

    {
        ...
        "TemplateNote": "gene similar to {DbId}, {LongDesc} ({Species}, {GeneName})"
        ...
    }

This will yield to this kind of annotation:

.. code-block::

    /note="gene similar to P60010, Actin, structural protein... (Saccharomyces cerevisiae, ACT1)"

.. warning::

    In that template, 4 **fields** are required (``{DbId}``, ``{LongDesc}``, ``{Species}`` and ``{GeneName}``). If one (or more) has an empty value,
    then the template solver will return an empty string for the whole line. This is a solution to avoid mal-formed sentences in the annotation. Below
    is the solution to overcome this problem.

Template can be divided into independant parts that can be filled or deleted depending on the availability of the data retrieved.
We use the double pipe ``||`` to indicate in the template the junctions between parts. Thus, for
example, the previous template can be adapted as follow:

.. code-block::

    {
        ...
        "TemplateNote": "gene similar to {DbId}||, {LongDesc}|| ({Species}, {GeneName})"
        ...
    }

With this template, if the **field** ``{LongDesc}`` is missing, then the annotation will be:

.. code-block::

    /note="gene similar to P60010 (Saccharomyces cerevisiae, ACT1)"

If one of the two **fileds** ``{Species}`` or ``{GeneName}`` is missing, the output will be:

.. code-block::

    /note="gene similar to P60010, Actin, structural protein..."

Here is the default template for the ``\note`` qualifier:

.. code-block::

    {
        ...
        "TemplateNote": "{Prefix}||{BdName}|{BdId} ||{Species} ||{LocusTag} ||{GeneName} ||{LongDesc}",
        ...
    }

This template will yield to this kind of output:

.. code-block::

    /note="higly similar to uniprot|P0CX24 Saccharomyces cerevisiae YOR312C RPL20B, component of the ribosome[...]"

The **field** ``{Prefix}`` depends on the different matching rules, see the dedicated :ref:`section <Matching rules>` for details.

Last, the templating syntaxe of **go-FAnnoT** also includes **transformers**, which correspond to an automatic text transformation applied of all the entries. Here is an example :

.. code-block::

    {
        ...
        "TemplateProduct": "{ShortDesc}::ToLwr",
        ...
    }

In this template, the **transformer** is ``::ToLwr``. The aim of this instruction is to lower case the first character of the output except
 if the whole first word is in upper case, such as a gene name or another acronym. Thus, for example, if the **field** ``{ShortDesc}`` contains
 the sentence ``Ribosome biogenesis protein BMS1``, then the generated output will be:

 .. code-block::

    /product="ribosome biogenesis protein BMS1"

But if the **field** ``{ShortDesc}`` contains the sentence ``NADH-ubiquinone oxidoreductase chain 3``, then the first character will be kept in upper case:

 .. code-block::

    /product="NADH-ubiquinone oxidoreductase chain 3"

The current version of **go-FAnnoT** proposes two **transformers**:

* ``::ToLwr``: to lower case the first character of a string (see above).
* ``::GnPn``: to convert gene name into protein names (see warning below).

.. warning::

    Gene name to protein name transformer ``::GnPn`` follows the *Saccharomyces cerevisiae*
    genetic nomenclature. Thus, for example, *ACT1* is a gene locus name (upper case, italicized).
    The corresponding protein name is Act1p (no italics, capitalied first letter, followed by a "p").
    Thus, considering the template ``{ShartDesc}::ToLwr::GnPn`` and the sentence ``Ribosome biogenesis protein BMS1``
    the output will be ``ribosome biogenesis protein Bms1p``. This may not fit the nomenclature of other organisms.

**Transformers** are hard-coded in **go-FAnnoT** sources. Users can develop their own **transformers** or
ask us to add new one via the github repository.

Algorithm options
*****************

Two parameters have to be set in the configuration file: 

.. code-block::

    {
        ...
        "NbHitCheck": 3,
        "MaxStatusOW": 1,
        ...
    }

The parameter ``NbHitCheck`` controls the number of blast hits for a given protein that are considered as 
possible candidates for annotation transfer. Indeed, because blast is a local alignment tool, the 
best hit is not necessary obtained with most globaly conserved protein from the blast database. One of the
originalities of **go-FAnnoT** is that the *n* first blast hits are reassessed using a global alignment 
algorithm (Needleman-Wunsch) and the final candidate is select on the basis of these global alignments.

The second parameter, ``MaxStatusOW`` deals with fact that, in particular conditions, a previously found annotation
can be overwritten by a better one. This process relies on the use of a hierarchy between the different ``refdbs``
 (see `Build reference databases <Build reference databases>`). These conditions are the following:

* The investigated gene has a possible candidate annotation found in one of the first ``refdbs`` of
the hierarchy. The similarity status (see `Matching rules <Matching rules>` for details)
associated to this candidate gene is less than or equal to ``MaxStatusOW``
* The investigated gene has a better hit in one of the following ``refdbs`` of the hierarchy and both the 
``refdb`` and the match status allow the overwriting.


Matching rules
**************

Blabla