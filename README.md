# Summary

The Recursive Binary Divisor recursiverly splits clusters in two until it cannot annymore. For this, the  script uses the mnhn-tree-tools (Haschka T. 2021) in a specific way. It first operates the count of kmers, the applies a pca on these counts. The projection is used as a support for dbscan. The epsilon paramter of dbscan is increased until a binary division occures. Then it adds the resulting clusters to a list. The tool visits and expands the list at the same time.  
# Installation

* Dependencies:

Make sure you have a GCC compiler version >= 4.9.2

For MNHN-Tree-Tools:

```
sudo apt-get install git build-essential libpng-dev libsdl2-dev liblapack-dev libopenmpi-dev libpocl-dev ocl-icd-opencl-dev pocl-opencl-icd

git clone https://github.com/Louis-MG/MNHN-Tree-Tools.git

cd MNHN-Tree-Tools
mkdir bin
make all
cd bin

# make MNHN-Tree-Tools available from any folder ( this is temporary, you
# may modify your .bashrc and similar files to make this permanent
export PATH=$PATH:$PWD
```

For python 3.4.2:

```
#if you dont have pip installed already (UNIX):

curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py 

#in case of installation troubles, refer to https://pip.pypa.io/en/stable/installing/
#python packages required:

pip install numpy
pip install regex
pip install shutils
pip install subprocess
pip install argparse

```
# Usage

e.g. :
``` 
python3 script.py -f test.fasta -e 0.5 -d 0.01 -o run_test
```

# Results

This program outputs 3 tabulated files and several folders. Each folder corresponds to a cluster; it contains: the fasta file 
containing the sequences, the kmer counts of the fasta, the eigen values file and the pca file obtained from the count file. 

* The first tabulated text file is named cluster_param.txt, where param is the chain of carachters of the main arguments (kmer length, 
epsilon, delta epsilon, minpoints, dimpca). Its header and content is the following:

|cluster_name|epsilon|father_size|son1_size|son2_size|
|:----------:|:-----:|:---------:|:-------:|:-------:|
|cluster|1.2|1000|400|500|
|cluster.1|1.5|400|200|100|	
|cluster.1.1|-1|200|NONE|NONE|
|cluster.1.2|0.9|100|30|20|

Epsilon  is either a positive float (the real epsilon) or a negative integer that indicates an error code: -1 signifies no or just one sub-cluster was found.
 
* The second tabulated file is sequence_parameters, where parameters is written as described above. It contains the names of the 
sequences and the respective name of the last cluster it belonged to. Exemple given:

|sequences_names|cluster_name|
|:-------------:|:-----:|
|sequenceXXXXXXX|cluster|
|sequenceYYYYYYY|cluster.1.2.1|
|sequenceZZZZZZZ|cluster.2.1|
|sequenceAAAAAAA|cluster.1|

* The third file is a summary of sequence_parameters, conveniently named sequence_summary.txt . It contains the number of sequences 
remaining in a cluster. In the case of a leaf, the number corresponds to the number of sequence in the leaf. In case of a branch, the 
number corresponds to the sequences fromn the custer that were not assigned to a sub-cluster; these are orphans.  

|Sequences left|cluster_name|
|:-:|:--------:|
|240|cluster|
|30|cluster.1.2|
|12|cluster.2|


###louis-mael.gueguen@etu.univ-lyon1.fr
