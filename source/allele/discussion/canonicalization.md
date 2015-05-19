---
title: Canonicalization 
description: Understanding both the need and challenges for canonicalizing alleles to support the needs of reliably aggregating curated variant knowledge and clinical/research genotypic data.
 
---

A specific allele may have multiple representations, both because of the variety of reference sequences to which the allele can be mapped and because not all alleles can be uniquely mapped to a given sequence. The [CanonicalAllele](/allele/conceptual/canonical_allele) concept is meant to provide a representation that can aggregate these representations for the purpose of recognizing common entities on which assertions can be made.

## Types of alleles that that can be mapped to one CanonicalAllele

### The same allele in the context of different ReferenceSequences

ReferenceSequences are not completely stable; over time, new sequences are discovered and old sequences are modified.  If an `Allele` is defined against an old sequence, and against the new sequence, both position and reference allele may vary across these representations. Furthermore, if a genomic variant occurs in a transcript, the allele may be defined with respect to either the genomic sequence, or the transcript sequence.

The problem of multiple representations of the same entity is compounded by the fact that none of these representations may be chosen as stable: any one of the reference sequences may be updated in the future, tying any preferred representation to an obsolete reference sequence.

### Different "spellings" of the same allele

Multiple representations of an allele are often produced when tandem repeats occur in the genome. For instance, if a region with three contiguous T bases is replaced with only 2 T bases, differing tools have several ways to represent the change (for instance, the removal of each of the three bases independently could all be reported, and would often be seen as different alleles, though they are indistinguishable, and should be aggregated together.

## The process of canonicalization

Implementors must choose a canonicalization method: a procedure for determining whether a given allele defined with respect to a particular ReferenceSequence can be aggregated into a previously known `CanonicalAllele`, and to provide a new `CanonicalAllele` when it cannot.   Because `CanonicalAllele` represents a potentially useful way to communicate about alleles with external systems, it would be beneficial to choose a canonicalization scheme that is externally implemented and can be used consistently by multiple parties.

One such canonicalization scheme (for DNA Alleles) would be to use dbSNP ids and the alternate allele as the `CanonicalAllele` identifier.  To canonicalize an allele, the NCBI Variation Reporter API would be queried for that allele.  If the alleles is known, then the dbSNP id would be returned and could be used.    In the case where the allele is not returned, however, a submission to dbSNP would then need to be instituted.  Because rsids are not immediately awarded, a temporary identifier would have to be used by the implementing system, to be updated once an rsid was granted, which may take as long as one month.  Another limitation of such an approach is that dbSNP ids can not be used for CanonicalProteinAlleles, so a different canonicalization approach must be made for that case.

`CanonicalAllele` contains both an id and the name of the system that created this id.  This allows for transmission of information about alleles between systems using different canonicalization schemes.  For instance, suppose that system A and system B each canoicalize alleles themselves, and system A wants to retrieve allele information from system B.  System B can send a message containing canonical alleles, as defined by system B.  Because the system namespace is different, there is no confusion that the id given to the allele will be the same id in the two systems.  Indeed, system A may choose to dissociate the different representations of the allele and re-canonicalize them using its own canonicalizer.  If system A wants to maintain system B's id for the canonical allele, it can add a CanonicalAlleleIdentifier.