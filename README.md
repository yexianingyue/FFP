# FFP
Feature Frequency Profile (FFP); two core programs


## Requirements  
- GCC(g++) version 4.7.1+  
- Google sparse hash library. Look here: https://github.com/sparsehash/sparsehash  
- zlib version 1.2.8+. Look here: http://www.zlib.net/  


## 1. FFP_compress.cpp
Compile: g++ -std=c++11 -o (execute name) (this script) -lz  
Run example: [Program path][options][input file path][output file path]  

### [Arguments]
* -h  
    Show options  
* -s [INT]  
    Feature size (l-mer)  
* -e [INT]  
    Feature size (l-mer) range end. Functional only when measuring vocabulary size using with -V  
* -a  
    Takes 20 amino acids sequences as inputs  
* -c  
    Convert and accept nucleotide bases (A, G, C, T) into RY bases (Purine, Pyrimidine)
* -k [STR]  
    Manual input of alphabet base string. For example input 'HJKL' is ['H', 'J', 'K', 'L'] bases  
* -r  
    Disable reverse complement counting. Any bases set other than AGCT code will disable reverse complement counting  
* -n  
    Output frequency, i.e., feature count / total feature count  
* -u  
    Accept masked (lower confidence) letters which are in lower cases in FASTA format  
* -V  
    Count vocabulary size at given range of feature length (l-mer)  
* -b [LONG LONG]  
    Bottom limit. Remove features that have the count less than [-b]  
    Default = 1
* -t [LONG LONG]  
    Top limit. Remove features that have the count larger than [-t]  
    Default = 0 = maximum  
    

### [Note]

/*
When using option [-a], amino acids input, [-r] turns on automatically that disable reverse complement counting, because peptide sequences have direction (start code -> stop codon). However, when using nucleotide (eg. genome and transcriptome) sequences as an input (default option) the sequence may be  can be either forward or reverse complement of double helix.


Use [-r] option which turn off reverse compliment counting when an input is a single strand nucleotide sequence.

Reverse complement counting pick one 'forward' or 'backward' feature of sequence that is lexically prior than another, because couting number of all forward and backward feature is equal to reverse complement counting x 2.

For example,
In DNA double helix,

AAAATTT -> lexically prior, so pick this one  
TTTTAAA -> even if actual readen feature is this! 
*/


### [Input]
FASTA format peptide or nucleotide sequence files. 


### [Output]
zlib compressed Feature Frequency Profile


## 2. JSD_matrix.cpp
Compile: g++ -std=c++11 -pthread -o (execute name) (this script) -lz  
Run example: [Program path][options][input files path] > [output file path (standard output)]  

### [Arguments]

* -h  
    Show options  
* -c [INT]  
    Specific integer code of single or escape character for delimiter in bewteen a set of [Feature|Value]  
    Default is 13, '\n', linebreak
* -t [INT]  
    Number of threads for multiprocessing  
    Heuristically, an adequate thread number is the number of cpu cores - 1. Default is 5
* -r [PATH]  
    Input previous matrix, and add more items to the matrix without calculating a whole    
* -d  
    Output Jensen-Shannon distance matrix instead of Jensen-Shannon divergence matrix which is default option.  
    Square root(JS divergence) = JS distance  
    

### [Note]
[-r] input low triangular divergence or distance matrix. This requires all pair-wise output of 'FFP_compress'


### [Input]
The output files of 'FFP_compress' which are zlib compressed Feature Frequency Profiles (FFPs)


### [Output]
Standard output of low triangular Jensen-Shannon divergence or distance matrix


## Limitation:
Generally, longer feature lengths (l-mer) consume more memory and time.  
In fungi proteome study the largest proteome has 35,274 proteins containing 10,866,611 amino acids, this program worked for feature length up to 24 amino acids.
