# Research
This repository contains research papers written by the iden3 team.

# Structure
Up to date there are three different directories: `articles`, `documentation` and `ZKProof-Workshop-2019`.

## `documentation`

It contains the description of different cryptographic protocols used in iden3. The documents are written in MarkDown.

## `articles`

It contains the articles that are or will be submitted to scientific journals and magazines. These documents are written in LaTeX.

## `ZKProof-Workshop-2019`
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
