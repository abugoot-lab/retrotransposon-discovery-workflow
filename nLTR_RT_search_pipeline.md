# nLTR Retrotransposon Discovery Pipeline

## ORF calling

Predict ORFs from eukaryotic assemblies.

```
orfm -m 1500 -c <codon_table> -s <FASTA>
```

## RT and RLE model construction

```
hmmbuild -n <name> <HMM> <MSA>
```

## RVT and RLE domain search

Search for RVT and RLE domains in ORF translations.

```
hmmsearch --domtblout <hits_table> -E 0.001 -Z 61295632 <HMM> <FASTA>
```

## ORF filtering

Discard ORFs meeting the following criteria:

- independent E-value greater than 0.0001
- bias >= 50% of the bit score
- length shorter than 800 amino acids or longer than 3000 amino acids
- containing an unknown amino acid (X)
- not having exactly one RT domain and one RLE domain C-terminal to the RT

## Annotating ORFs

### NCBI CDD

```
rpsblast -query <FASTA> -db Cdd -evalue 0.01 -outfmt 11 -out <output> \
  -seq no -comp_based_stats 1 -dbsize 5000000
rpsbproc -i <input> -o <output> -e 0.01 -m rep -d <CDD_data_dir>
```

### Pfam

```
hmmscan -o /dev/null --domtblout <output> --cut_ga -Z 61295632 <Pfam_DB> \
  <FASTA>
```

### Rfam

Search up to 2000 bases upstream and downstream of ORF.

```
cmscan -o /dev/null --nohmmonly --tblout <output> --fmt 2 \
  --clanin <Rfam_clans> -E 0.0001 <Rfam_DB> <FASTA>
```

### PROSITE

Search PROSITE zinc finger and myb profiles.

```
perl ps_scan.pl -o gff -d <profiles> -l '-1' -v <FASTA>
```

### TRF

```
trf <FASTA> 2 7 7 80 10 50 500 -d -h
```

## Phylogenetic tree

1. Extract ORF from start of RT domain to C-terminal end
2. Cluster sequences with permissive parameters
3. Align permissive cluster representatives
4. Manually refine alignment to create seed alignment
5. Cluster sequences with strict parameters
6. Add strict cluster representatives to seed alignment
7. Trim alignment
8. Construct tree

### Permissive clustering

```
mmseqs easy-cluster <FASTA> <output> <tmpdir> -c 0.8 --min-seq-id 0.4
```

### Seed alignment

```
muscle -super5 <FASTA> -output <output>
```

### Strict clustering

```
mmseqs easy-cluster <FASTA> <output> <tmpdir> -c 0.8 --min-seq-id 0.7
```

### Align strict cluster representatives

```
mafft --auto --add <FASTA> <MSA>
```

### Trim alignment

```
clipkit --mode kpic-smart-gap <MSA>
```

### Construct tree

```
FastTreeMP -lg -gamma -spr 4 -mlacc 2 -slownni <MSA>
```

## Computational prediction of ncRNA

https://github.com/abugoot-lab/ncrna_conservation

## Construction of representative set of ORFs

```
mmseqs easy-cluster <FASTA> <output> <tmpdir> -c 0.8 --min-seq-id 0.9 -s 5
```

## Reference

Fell, C.W., Villier, L., Lim, J. *et al* Reprogramming site-specific
retrotransposon activity to new DNA sites. *Nature* (2025).
