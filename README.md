# Biostrings Tutorial

## Step 1: Install and Load the Package
Before using the ```Biostrings``` package, you need to install it from the CRAN repository. Make sure to install the correct genome you are using by using the BiocManager function. To find a list of all of the genomes availbe for use and how to enter them in, visit <<<https://bioconductor.org>>> Open R or RStudio and execute the following commands:
```
install.packages("Biostrings")
library(Biostrings)
BiocManager::install("Genome_of_use") #replace with the genome that you wish to use.
```
## Step 2: Create Sequences
Identify a specific gene or sequence of interest from NCBI. Make sure to note down the GI number or accession number associated with the sequences. Using the ```getSeq``` function, retrieve the DNA sequences based on the GI or accession number. Replace ```your_gi_or_accession_number'``` with the actual GI number or accession number. The second portion of this function is where you are pulling the number from, ie. what genome. 
```
sequence <- getSeq("your_gi_or_accession_number", "Where you are getting your number from")
print(sequence)
```
## Step 3: Sequence Manipulation
The ```Biostrings``` package provides various functions for manipulating sequences. Here are a few commonly used operations:

### Length: 
Get the length of a sequence.
```
sequence_length <- nchar(sequence)
print(sequence_length)
```
### Reverse complement
Obtain the reverse complement of a DNA sequence.
```
reverse <- reverseComplement(dna_seq)
print(reverse)
```
### Translation 
Translate a DNA sequences into protein using the appropriate genetic code. Replace ```genetic_code_number``` with the desired genetic code number (eg. 1 for the standard genetic code). Use the ```translate``` function.
```
translated_protein <- translate(sequence, genetic_code = 'genetic_code_number')
print(translated_protein)
```
### Subsetting
If you wish to extract a subsequence from a sequence. Using the ```subseq``` function, call the sequence that you wish to use and identify the start and end of the sequence.
```
subset <- subseq(dna_seq, start = 3, end = 7)
print(subset)
```
## Step 4: Sequence Comparison
The ```Biostrings``` package allows you to compare seqeuces and search for specific patters. Here are a few examples:

### Gather your sequences and perform the alignement
```
#Sequences you wish to use
seq1 <- getSeq("accession number", "Genome_of_use")
seq2 <- getSeq("accession number" , "Genome_of_use")

# Perform alignment
alignment_score <- pairwiseAlignment(sequence1, sequence2, substitutionMatrix = "BLOSUM50", gapOpening = 10, gapExtension = 2)

print(alignment_score)

#Retrieve the aligned sequences and print them
aligned_sequence1 <- alignment_score@subject
aligned_sequence2 <- alignment_score@query

print(aligned_sequence1, aligned_sequence2)
```
### Pattern matching
Find matches of a pattern within a sequence.
```
# Find matches of the pattern "ATG" in a DNA sequence
pattern <- DNAString("ATG")
pattern_positions <- vmatchPattern(pattern, sequence1)
```
## Step 5: Sequence Analysis
You can also analyze sequence properties and characteristics using the Biostrings package. Here are a couple of examples:

GC content: Calculate the GC content of a DNA sequence.
```
# Calculate GC content
gc_content <- letterFrequency(dna_seq, baseOnly = TRUE)["G"] + letterFrequency(dna_seq, baseOnly = TRUE)["C"]
gc_content
```
Molecular weight: Calculate the molecular weight of a protein sequence.
```
# Calculate the molecular weight of the translated protein sequence
install.packages("protr")
library(protr)

molecular_weight <- sum(aaWeights(translated_protein))
```
# Biostrings Additional Example
## Step 1: Install and Load the Package
Make sure you have the `Biostrings` package installed and loaded in R. If not, refer to Step 1 in the previous tutorial for installation instructions.

## Step 2: Retrieve the BLAST Sequence
To retrieve a BLAST sequence from NCBI, we need to know the accession number or GI number of the sequence. Let's assume we have a GI number, and we want to retrieve the corresponding DNA sequence.
```
# Load the necessary library
library(Biostrings)

# Set the GI number
gi_number <- "123456789"  # Replace with the actual GI number

# Retrieve the sequence from NCBI
blast_seq <- getSeq(gi = gi_number, format = "fasta")
```

In the above code, we set the `gi_number` variable to the GI number of the desired sequence. Replace `"123456789"` with the actual GI number you want to retrieve.

The `getSeq` function retrieves the sequence from NCBI's database in FASTA format and stores it in the `blast_seq` variable.

## Step 3: Perform Sequence Manipulations or Analysis
Once you have the BLAST sequence stored in the `blast_seq` variable, you can perform various manipulations or analyses on it using the functions provided by the `Biostrings` package. For example, you can calculate the length of the sequence:

```
# Calculate the length of the sequence
length(blast_seq)
```

You can also perform other operations such as finding matches, extracting subsequences, or analyzing sequence properties as demonstrated in Step 3 and Step 5 of the previous tutorial.

Remember to replace `"123456789"` with the actual GI number of the BLAST sequence you want to retrieve.
