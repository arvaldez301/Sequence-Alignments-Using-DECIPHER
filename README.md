# Sequence Alignment in BLAST
## Step 1: Install Required Packages
To get started, make sure you have the necessary packages installed in RStudio. You'll need the "rentrez" package for accessing the Entrez database and the "dplyr" package for data manipulation. If you don't have these packages installed, you can install them using the following code:

```
install.packages("rentrez")
install.packages("dplyr")
```

## Step 2: Load Required Packages
Once you have the packages installed, load them into your RStudio session using the following code:

```
library(rentrez)
library(dplyr)
```

## Step 3: Search for Data in Entrez
To search for data in Entrez, you'll need to specify the database you want to search and the query you want to perform. Here's an example of how to search for articles related to a specific keyword in the PubMed database:

```
query <- "cancer"  # Specify your query term
db <- "pubmed"  # Specify the Entrez database

# Search for data
search_results <- entrez_search(db, term = query)
```

## Step 4: Retrieve Data from Entrez
Once you have performed the search, you can retrieve the data using the following code:

```
# Get the list of IDs from the search results
id_list <- search_results$ids

# Fetch the data based on the IDs
data <- entrez_fetch(db, id = id_list, rettype = "xml", parsed = TRUE)
```

## Step 5: Extract Relevant Information
The data retrieved from Entrez is typically in XML format. You can extract the relevant information using the functions provided by the "rentrez" and "dplyr" packages. Here's an example of how to extract the title and abstract from the retrieved data:

```
# Extract title and abstract
title <- data$PubmedArticle$MedlineCitation$Article$ArticleTitle
abstract <- data$PubmedArticle$MedlineCitation$Article$Abstract$AbstractText

# Combine the results into a data frame
results <- data.frame(Title = title, Abstract = abstract)
```

## Step 6: Analyze and Visualize the Data
Now that you have the relevant information in a data frame, you can perform any analysis or visualization you require. Here's an example of how to count the number of articles for each year:

```
# Convert the publication date to year
results$Year <- substr(results$PubDate, start = 1, stop = 4)

# Count the number of articles per year
article_counts <- results %>% count(Year)

# Plot the results
plot(article_counts$Year, article_counts$n, type = "b", xlab = "Year", ylab = "Number of Articles")
```

# Biostrings Tutorial

## Step 1: Install and Load the Package
Before using the ```Biostrings``` package, you need to install it from the CRAN repository. Open R or RStudio and execute the following commands:
```
install.packages("Biostrings")
library(Biostrings)
```
## Step 2: Create Sequences
To begin, we need to create sequences to work with. You can create DNA, RNA, or protein sequences using the ```DNAString()```, ```RNAString()```, and ```AAString()``` functions, respectively. Here are a few examples:
```
# DNA sequence
dna_seq <- DNAString("ATCGGCTAATCG")

# RNA sequence
rna_seq <- RNAString("AUCGGCUAAUCG")

# Protein sequence
protein_seq <- AAString("MSPAMLEWR")
```
## Step 3: Sequence Manipulation
The ```Biostrings``` package provides various functions for manipulating sequences. Here are a few commonly used operations:

Length: Get the length of a sequence.
```
length(dna_seq)
```
Reverse complement: Obtain the reverse complement of a DNA sequence.
```
reverseComplement(dna_seq)
```
Translation: Translate a DNA or RNA sequence to protein.
```
translate(dna_seq)
translate(rna_seq)
```
Subsetting: Extract a subsequence from a sequence.
```
subseq(dna_seq, start = 3, end = 7)
```
## Step 4: Sequence Comparison
The ```Biostrings``` package allows you to compare seqeuces and search for specific patters. Here are a few examples:

Alignment: Perform pairwise sequence alignment
```
# Create sequences
seq1 <- DNAString("ATCG")
seq2 <- DNAString("ATCGA")

# Perform alignment
pairwiseAlignment(pattern = seq1, subject = seq2)
```
Pattern matching: Find matches of a pattern within a sequence.
```
# Find matches of the pattern "AT" in a DNA sequence
pattern <- DNAString("AT")
matchPattern(pattern, dna_seq)
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
# Calculate molecular weight
molecular_weight(protein_seq)
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
