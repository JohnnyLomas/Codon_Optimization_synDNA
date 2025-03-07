# Codon optimization for DNA synthesis
A set of scripts reliant on [BioPython](https://biopython.org/) and [DNAChisel](https://edinburgh-genome-foundry.github.io/DnaChisel/) to perfrom codon optimization with the dual objectives of efficent translation and efficent DNA synthesis. Sequences are optimized towards the fastest translating codons while avoiding common motifs that inhibit DNA synthesis.

Avoided motifs include:
- Repeated K-mers
- Homopolymers
- Hairpins

## Environment
```
conda install -c bioconda dnachisel biopython
```

## Workflow
Start with a fasta file of coding sequences to be optimized and run ```codonOptimizeSynthesis.py```. Adjust the weights and thresholds as necessary to acheive the desired result.

```
usage: codonOptimizeSynthesis.py [options] input_file

positional arguments:
  input_file            Fasta file to be optimized

options:
  -h, --help            show this help message and exit
  -o outfile            Output file name
  -s species            Species codon usage to optimize towards ['A_thaliana', 'G_max']
  --rscu RSCU           JSON file containing RSCU values
  --rscuGenerate CDS.fasta
                        Generate rscu values from CDS fasta file
  --topCodonWeight TOPCODONWEIGHT
                        Adjust weight optimizing towards highest frequency codon (default: 25)
  --rareCodonThreshold RARECODONTHRESHOLD
                        Attempt to avoid codons with frequencies below this number (default: 0.1)
  --rareCodonWeight RARECODONWEIGHT
                        Adjust weight for avoiding rare codons (default: 500)
  --repeatedKmerWeight REPEATEDKMERWEIGHT
                        Adjust weight for avoiding repeated kmers (default: 100)
  --repeatedHomopolymerWeight REPEATEDHOMOPOLYMERWEIGHT
                        Adjust weight for avoiding repeated homopolymers (default: 50)
  --hairpinWeight HAIRPINWEIGHT
                        Adjust weight for avoiding hairpins (default: 1000)
  --uniquifyKmersWeight UNIQUIFYKMERSWEIGHT
                        Adjust weight for avoiding kmer repeats of 8 bases and longer (default: 1000)
```

Next, you can verify your optimized sequences using ```checkCodons.py```. This will print out a report showing the original and optimized protein alignment and the usage of top codons over the coding sequence.

```
checkCodons.py [-h] [-s species] original optimized


positional arguments:
  original    Original fasta file
  optimized   Optimized fasta file

options:
  -h, --help  show this help message and exit
  -s species  Species codon usage to use ['A_thaliana', 'G_max']
```

## Codon Usage Tables

Codon usage tables for *A. thaliana* and *Glycine max* (soybean) are provided in the ```data/``` folder. The values represent fractions among synonymous codons. You can provide your own codon usage table via the ```--rscu``` argument.

**Note:** Many measures of codon translation efficiency exist and there may be a more effective formulation for your use case... These include (CAI, ENC, tAI, expression normalized tAI, etc.)

**Arabidopsis:** 
https://www.kazusa.or.jp/codon/cgi-bin/showcodon.cgi?species=3702&aa=1&style=N

**Soybean** https://www.kazusa.or.jp/codon/cgi-bin/showcodon.cgi?species=3847&aa=1&style=N
