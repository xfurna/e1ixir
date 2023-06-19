+++
title = "ytdydts bhai"
overview = "kewal test  hijhfjf khi jki ojpohogoi;g ohoih and some more random text."
metaData = "2 June, 2023"
category = "d"
+++

Here I am developing a python package implementing **CoALa**(<ins>Co</ins>nvex-combination of <ins>A</ins>pproximate <ins>L</ins>aplacians) to make everyone's life easy.
Know more about CoALa [here](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjVr9TXhOjpAhWQXCsKHYHNDGMQFjABegQIARAB&url=https%3A%2F%2Fwww.ncbi.nlm.nih.gov%2Fpubmed%2F31603770&usg=AOvVaw2WCVqw4fcaxMLQHn6ub7_b). 

# This Repository
```
├── bin
│   └── local_tests.py
├── coalapy
│   ├── dfhandler.py
│   ├── helpers
│   │   ├── helper.py
│   │   ├── house_keeper.py
│   │   ├── __init__.py
│   │   └── utils.py
│   ├── __init__.py
│   ├── matrices
│   │   ├── getters.py
│   │   ├── __init__.py
│   │   ├── make_mat.py
│   │   └── similarity_mat.py
│   └── modalities.py
├── coalapy.egg-info
│   ├── dependency_links.txt
│   ├── PKG-INFO
│   ├── SOURCES.txt
│   └── top_level.txt
├── LICENSE
├── main.py
├── Pipfile
├── Pipfile.lock
├── README.md
├── requirements
│   ├── dev.txt
│   └── production.txt
├── resources
│   └── PITCH.md
├── setup.py
├── tests
│   └── test_coala.py
└── tweak.py

8 directories, 27 files
```
---
- **main.py:** A clean, step by step implementation of the algorithm. This script imports the package `src`.
- **resources:** This directory contains related resources. 
- **resources/PITCH.md:** This file has links to the pitching given by us about the algorithm.
- **coalapy:** This is the black box of the whole repository. While `main.py` implements the algorithm, all the processing is done by the scripts stored in this directory.
- **coalapy/dfhandler.py:** This module has the classes and methods relevant to the data files. 
- **coalapy/helpers/:** This collection of modules contains methods 1) for general house keeping 2) for general re-directions 3) of general utility. 
- **bin/local_test.py:** Unit tests.
- **coalapy/matrices/:** This collection of modules contains the methods relevant to matrix construction. 
- **coalapy/modalities.py:** This module has the classes relevant to the modalities and their laplacian.
- **tweak.py:** _Hidden_
---
# The algorithm

## Introduction
One of the important approaches of handling data heterogeneity in multimodal data clustering is modeling each modality using a separate similarity graph. Information from the multiple graphs is integrated by combining them into a unified graph. A major challenge here is how to preserve cluster information while removing noise from individual graphs. In this regard, a novel algorithm , termed as CoALa, is proposed that integrates noise-free approximations of multiple similarity graphs.

## How does it work?
* It approximates a graph using the most informative eigenpairs of its Laplacian which contain cluster information.
* The approximate Laplacians are then integrated for the construction of a low-rank subspace that best preserves overall cluster information of multiple graphs.
* However, this approximate subspace differs from the full-rank subspace which integrates information from all the eigenpairs of each Laplacian.
* Matrix perturbation theory is used to theoretically evaluate how far approximate subspace deviates from the full-rank one for a given value of approximation rank.
* Finally, spectral clustering is performed on the approximate subspace to identify the clusters.

## Matrices
1. Normalised Laplacian	:![cc](https://latex.codecogs.com/gif.latex?%3A%20L%20%3D%20D%5E%7B1/2%7D%28D%20-%20W%29D%5E%7B-1/2%7D)
2. Shifted Laplacian	:![dfea](https://latex.codecogs.com/gif.latex?L%20%3D%202I%20-%20L%20%3D%20I%20&plus;%20D%5E%7B-1/2%7DWD%5E%7B-1/2%7D)
3. Random walk		:![vsdv](https://latex.codecogs.com/gif.latex?RW%3DD%5E%7B-1%7DL)
Where D  is degree matrix and W is similarity matrix

# Proposed Method

## Convex Combination of Graph Laplacian

- Let a multimodal data set, consisting of M modalities, be given by X1, . . . ,Xm, . . . ,XM
- Shifted Laplacian for modality ![fbewgfbfb](https://latex.codecogs.com/gif.latex?X_%7Bm%7D%3A%20L_%7Bm%7D%20%3D%20I%20&plus;%20D%5E%7B-1/2%7D_%7Bm%7D%20W_%7Bm%7DD%5E%7B-1/2%7D)
- The rank r eigenspace of shifted Laplacian Lm for modality Xm is defined by a two-tuple:
 ![bfbdfbew](https://latex.codecogs.com/gif.latex?%5CPsi%20%28L%5E%7Br%7D_%7Bm%7D%29%20%3D%20%3CU%5E%7Br%7D_%7Bm%7D%5E%7B%7D%2C%5Csum%5E%7Br%7D_%7Bm%7D%3E)

## Construction of Joint Eigenspace:
* ![fewDVFSf](https://latex.codecogs.com/gif.latex?L%3DZ%5CGamma%20Z%5E%7BT%7D)
    * Joint shifted Laplacian
    * Z consists of the eigenvector of L
    * ![..](https://latex.codecogs.com/gif.latex?%5CGamma)=diag(![..](https://latex.codecogs.com/gif.latex?%5Cgamma%20_%7B1%7D.........%5Cgamma%20_%7Bn%7D))
* ![..vdsfvwv.](https://latex.codecogs.com/gif.latex?%5CPsi%20%28L%5E%7Br%7D%29%20%3D%20%3CZ%5E%7Br%7D%2C%5CGamma%5E%7Br%7D%3E)
    * ![...vf.](https://latex.codecogs.com/gif.latex?%5CPsi%20%28L%29%5E%7Br%7D) represents that eigenspace has rank r
* ![..vv...](https://latex.codecogs.com/gif.latex?L%5E%7Br%5E%7B*%7D%7D%3D%5Csum_%7Bm%3D1%7D%5E%7BM%7D%5Calpha%20_%7Bm%7DL_%7Bm%7D%5E%7B%5E%7Br%7D%7D)
    * ![.a.a.a.](https://latex.codecogs.com/gif.latex?L%5E%7Br%5E%7B*%7D%7D)  represents convex combination of best rank approximation of Laplacians Lm of indivisual modality Xm..

## Proposed CoALa Algorithm
**Input:** Similarity matrices ![d.lalsacad](https://latex.codecogs.com/gif.latex?W_%7B1%7D%2C.......%2CW_%7BM%7D) combination vector= ![.a.s.d.sca](https://latex.codecogs.com/gif.latex?%5B%5Calpha%20_%7B1%7D%2C.......%2C%5Calpha%20_%7BM%7D%5D) number of clusters k, and rank r >= k.

**Output:** Clusters ![dbb.it](https://latex.codecogs.com/gif.latex?%5BA_%7B1%7D%2C.......%2CA_%7Bk%7D%5D)
1. **for** m=1 **to** M **do**
2. Construct degree matrix Dm and shifted normalized Laplacian Lm.
3. Compute the eigen-decomposition of Lm.
4. Store the r largest eigenvalues in r m and corresponding eigenvectors in Urm in the rank r eigenspace of Xm.
5. **end for**
6. Compute initial basis![ffccc](https://latex.codecogs.com/gif.latex?U_%7B1%7D%20%5Cleftarrow%20U_%7Br%7D)
7. **for** m = 1 **to** M - 1 **do**
8. Compute Sm+1, projected component Pm+1, and residual component Qm+1 .
9. ![tfcfc fcsac,c.c.c.ccdss](https://latex.codecogs.com/gif.latex?%5Cgamma%20_%7Bm&plus;1%7D%5Cleftarrow) Gram-Schmidt orthogonalization of Qm+1
10. Update basis ![v](https://latex.codecogs.com/gif.latexU_%7Bm&plus;1%7D%5Cleftarrow%20%5B?U_%7Bm%7D%20%5Cgamma%20_%7Bm&plus;1%7D%5D)
11. **end for**
12. For each modality Xm, compute Hm 
13. Compute the new eigenvalue problem H.
14. Solve the eigen-decomposition of H to get R and H.
15. Compute eigenvectors ![p](https://latex.codecogs.com/gif.latex?V%5Cleftarrow%20U_%7BM%7DR)
16. Compute joint eigenspace 	![salkcndandsnvd](https://latex.codecogs.com/gif.latex?%5CPsi%20L%5E%7Br%5E%7B*%7D%7D%5Cleftarrow%20%3CV%5E%7Br%7D%20%5CPi%20%5E%7Br%7D%3E)
17. Find k largest eigenvectors Vk = [v1 . . . vk].
18. Perform clustering on the rows of Vk using k-means algorithm.
19. **Return** clusters A1, . . . ,Ak from k-means clustering.

# Notes

- Code has been written considering that the raw modality table has nodes in columns and features in rows.
- You don't need to touch anything except `main.py` (unless you're researching or eager to contribute). 
- For getting results, just enlist the absolute paths of each modality in `path_list` list in `main.py`
- You may also like to edit the values of variables- `k` (expected number of clusters) and `rank` (the rank of the laplacian) which have been given default values of 2 and 4 respectively.
- The results will get stored in `.data` directory as `CoALa.csv`.
- In that file, first row is the index of each data-point and each column of second row is assigned a number on the basis of their belonging to the clusters
- If your data files are doubly featured (as in- two dimensional), then you can uncomment the code snippet written at the bottom in `main.py` allowing it to pop up an illustration indicating all the clusters.

# Contribution

Not accepting.

