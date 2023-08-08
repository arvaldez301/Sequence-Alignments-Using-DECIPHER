# DECIPHER Tutorial
In this tutorial, you will learn how to perform sequence alignment using the DECIPHER package in R. Sequence alignment is a fundamental step in bioinformatics and is used to identify similarities and differences between biological sequences, such as DNA or protein sequences. DECIPHER is a powerful R package that provides functions for sequence alignment and analysis.

Additional Resource: <https://bioconductor.github.io/BiocWorkshops/working-with-genomic-data-in-r-with-the-decipher-package.html>

## Step 1: Install and Load the Required Libraries
Before you begin, make sure you have R and the DECIPHER package installed. You can install the DECIPHER package using the following code:
```
if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("DECIPHER")
```
To start, we need to load the DECIPHER package into our R session. Use the following code to load the package:
```
library(DECIPHER)
```
## Step 2: Reading the DNA Sequences
In this step, we will read the DNA sequences from a FASTA file. You can either use your own FASTA file or use the provided example file from the DECIPHER package. If you have your own FASTA file, specify the path to the file in the ```fas``` variable. Otherwise, you can use the provided example file from the DECIPHER package:
```
# Upload your own sequences and enter in the path to your FASTA file
fas <- "<<path to FASTA file>>"
```
If you do not have your own you can use those provided in the package. To view what files are in the package use the following code:
```
package_dir <- system.file("extdata", package = "DECIPHER")
files <- list.files(package_dir)
print(files)

#Upload an existing file
fas <- system.file("extdata", "<<<file name>>>", package = "DECIPHER")
```
Finally, whatever data file you decide to use whether it is your own or an existing, proceed to the following code:
```
dna <- readDNAStringSet(fas)
```
The ```readDNAStringSet()``` function reads the sequences from the FASTA file and stores them in the ```dna``` variable. You can display the unaligned sequences by typing dna in the R console.
## Step 3: Aligning the DNA Sequences
DECIPHER provides multiple methods for sequence alignment. We will cover two methods: aligning the translation and aligning the sequences themselves.

### Method 1: Aligning the Translation
To align the translation of the DNA sequences into amino acids, use the ```AlignTranslation()``` function:
```
AA <- AlignTranslation(dna, type = "AAStringSet")
```
The AlignTranslation() function aligns the DNA sequences and translates them into amino acids. The aligned sequences are stored in the AA variable. ```type = "AAStringSet"``` indicates that the sequences that are being aligned are amino acids
### Method 2: Aligning the Sequences
To align the DNA sequences without translation, use the AlignSeqs() function:
```
DNA <- AlignSeqs(dna)
```
The AlignSeqs() function directly aligns the DNA sequences without translating them. The aligned sequences are stored in the DNA variable.

#If an error appears regarding Gap Characters preform the follwing command before the alignments:
```
dna <-RemoveGaps(dna)
```
### Method 3: Aligning the Translation and then the Reverse Translation
```
DNA <- AlignTranslation(dna)
```

## Step 4: Visualizing the Alignment
DECIPHER provides a function called BrowseSeqs() that allows you to visualize the alignment. You can highlight specific sequences or regions of interest. To browse the alignment, use the following code:
```
BrowseSeqs(AA, highlight = 1)
```
## Step 5: Correcting Alignment Errors
If there are frameshifts or errors in the alignment, you can correct them using the ```CorrectFrameshifts()``` function. First, apply the function to the nucleotide sequences, and then re-align them using either ```AlignTranslation()``` or ```AlignSeqs()```.

## Step 6: Saving the Aligned Sequences
To save the aligned sequences to a new FASTA file, you can use the ```writeXStringSet()``` function. Specify the aligned sequences and the output file path and the name of your new file:
```
writeXStringSet(DNA, file = "<<path to output file>>")
```
Make sure prior to using the ```writeXStringSet``` function you convert your new aligned data set back to a String Set using either ```DNAStringSet()```, ```RNAStringSet()```, of ```AAStringSet()``` depending on what your aligned data was.
## For Non-Coding RNA Sequences
In these step, we will work with non-coding RNA sequences. Specifically, we will use a database containing 16S ribosomal RNA sequences and align them.

### Step 1: Loading the Files
Set the path to the database file containing the 16S ribosomal RNA sequences:
```
db <- system.file("extdata", "Bacteria_175seqs.sqlite", package = "DECIPHER")
```
Use the SearchDB() function to retrieve the non-coding RNA sequences from the database:
```
rna <- SearchDB(db, remove = "all", type = "RNAStringSet")
```
The ```SearchDB()``` function retrieves the RNA sequences from the specified database file and stores them in the ```rna``` variable.

Alternatively, if you have DNA sequences and want to convert them to RNA, use the ```RNAStringSet()``` function:
```
rna <- RNAStringSet(dna)
```
This function converts the DNA sequences in the ```dna``` variable to RNA sequences. 

Alternatively, if you have RNA sequences in a FASTA file, you can import them directly using the ```readRNAStringSet()``` function:
```
rna <- readRNAStringSet("<<path to FASTA file>>")
#Replace ```<<path to FASTA file>>``` with the actual path to your FASTA file.
```
### Step 2: Aligning Non-Coding RNA Sequences
Next, we will align the non-coding RNA sequences. DECIPHER provides the ```AlignSeqs()``` function, which can align RNA sequences and take into account RNA secondary structure.

Align the RNA sequences using the ```AlignSeqs()``` function:
```
alignedRNA <- AlignSeqs(rna)
```
The AlignSeqs() function aligns the RNA sequences, considering RNA secondary structure, and stores the aligned sequences in the ```alignedRNA``` variable.
### Step 3: Aligning Two Aligned Sequence Sets
In this step, we will align two sets of already aligned sequences.

Determine the index of the halfway point of the DNA sequences:
```
half <- floor(length(dna) / 2)

#The half variable store the index of the halfway point
```
Set the first half of your DNA sequences
```
dna1 <- dna[1:half]

#The dna1 variable contains the first half of the DNA sequences, from index 1 to half.
```
Set the second half of your DNA sequences:
```
dna2 <- dna[(half + 1):length(dna)]

#The dna2 variable contains the second half of the DNA sequences, from index half + 1 to the end of the sequences.
```
Align the translation of the first half of DNA sequences:
```
AA1 <- AlignTranslation(dna1, type = "AAStringSet")

#The AlignTranslation() function aligns the translation of the DNA sequences and stores the aligned sequences in the AA1 variable.
```
Align the translation of the second half of DNA sequences:
```
AA2 <- AlignTranslation(dna2, type = "AAStringSet")

#The AlignTranslation() function aligns the translation of the DNA sequences and stores the aligned sequences in the AA2 variable.
```
Align the two alignments using the AlignProfiles() function:
```
AA <- AlignProfiles(AA1, AA2)

#The AlignProfiles() function aligns the two sets of aligned sequences (AA1 and `AA
```
## Additional Notes
Other string sets that you are able to use when using the ```AlignTranslation()``` function are:

```"DNAStringSet"```: This type is used to indicate that the sequences being processed are DNA sequences. Functions like AlignSeqs() and SearchDB() accept this type to handle DNA sequence alignment and database searching, respectively.

```"RNAStringSet"```: This type is used for RNA sequences. Similar to DNA sequences, functions like AlignSeqs() and SearchDB() can accept this type when working with RNA sequence alignment or searching in RNA sequence databases.

```"NNStringSet"```: This type is used for nucleotide sequences that can contain both DNA and RNA. Functions like AlignSeqs() and SearchDB() can handle this type to accommodate sequences with mixed DNA and RNA components.

```"MSA"```: This type is used for multiple sequence alignments. It is typically used when working with pre-aligned sequences. Functions like AlignProfiles() accept this type to perform alignment on pre-aligned sets of sequences.

```"AAString"```: This type is used for individual amino acid sequences. It is commonly used when working with single sequences rather than sets of sequences.
