# Research
This repository contains research papers written by the iden3 team.

# Structure
Up to date there are two different directories: `pedersen-hash` and `ZKProof-Workshop-2019`. All research documents are writen in LaTeX. To generate a PDF, clone the repository and compile the `main.tex` files.

## pedersen-hash
It contains an old version of the description the Pedersen Hash function implementation using Baby Jubjub elliptic curve and 4-bit windows. This document is obsolete, as it has been shown by Zcash team that the usage of 3-bit windows is more efficient.

## ZKProof-Workshop-2019
It contains four standard proposals written for the ZKProof Workshop 2019, which took place in Berkeley last 10-12th of April, 2019.

- `preamble.tex` : packages, commands and margins of the documents.
- `lit.bib` : bibliography database.
- `intellectual-property.tex` : intellectual property rights.
- `figures` : directory containing all files with figures.
- `proposals` : directory containing four standard proposals.
    - `EdDSA/main.tex` : proposal of standarisation of the Edwards Digital Signature Algorithm for Baby Jubjub elliptic curve.
    - `GenerationTEdEC/main.tex` : proposal of standarisation of a deterministic methodology for generating Twisted Edwards and Montgomery Elliptic Curves for circuit use (accepted proposal).
    - `MerkleTree/main.tex` : proposal of standarisation of Sparse Merkle Trees implementation.
    - `PedersenHash/main.tex` : proposal of standarisation of Pedersen Hash function using Baby Jubjub elliptic curve and using 4-bit windows. Note: as 3-bit windows are more efficient (Zcash), this document is out-dated. 
