---
title: Gene
layout: resource
alias: 
description: A region (or regions) that includes all of the sequence elements necessary to encode a functional transcript. A gene may include regulatory regions, transcribed regions and/or other functional sequence regions.
sequence_ontology: SO:0000704
model: allele
resource: gene
attributes:
  - name: identifier
    type: Identifier[]
    definition: The set of associated internal and external identifiers for the gene.
  - name: symbol
    type: string
    definition: The commonly accepted short-form symbol representing the gene (i.e.) HGNC symbol.
  - name: name
    type: string
    definition: The long-form gene name.
  - name: aliasSymbol
    type: string[]
    definition: The set of alternative symbols used to identify the gene.

---


Scope and Usage
---------------

A gene is identified by an external authority, typically HGNC, although the data model is configured to recognize identifiers from multiple authorities.

Context
-------

The Gene resource is described by the following attributes

* identifer: the set of identifiers from external authorities that identify the Gene.
* symbol: the string symbol identifying the gene.
* name: the human readable name for a gene.
* aliasSymbol: a list of alternative symbols used to identify the gene.

Notes
-----
