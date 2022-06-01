# flank as a percentage


```
a=(0.50 0.60 0.70);
s=(0.90 0.95 0.98);
flank=(0.00 0.05 0.10);

for i in "${a[@]}"
    do
        aa=$i
        for j in "${s[@]}"
            do
                ss=$j
                for k in "${flank[@]}"
                    do
                        fflank=$k
                        echo "Processing a:" ${aa} "s:" ${ss} "flank:" ${fflank}
                        liftoff \
                        -g ../input/QUERY_genes.gff3 \
                        -o ../output/output_subdir/MAPPINGS_a-${aa}_s-${ss}_flank-${fflank}.gff3 \
                        -u ../output/output_subdir/MAPPINGS_a-${aa}_s-${ss}_flank-${fflank}_unmapped_features.txt \
                        -dir ../output/output_subdir/MAPPINGS_input_files \
                        -a $aa -s $ss -flank $fflank \
                        -p 24 \
                        -chroms ../input/chroms.txt \
                        -unplaced ../input/unplaced_SCAFFOLDS.txt \
                        ../input/REFERENCE.fasta \
                        ../input/QUERY.fasta > ../output/output_subdir/MAPPINGS_a-${aa}_s-${ss}_flank-${fflank}_log.txt 2>&1;
                        rm -r ../output/output_subdir/MAPPINGS_input_files;
                    done
            done
    done;
```

see

[htps://github.com/NIB-SI/Liftoff/blob/master/liftoff/run_liftoff.py](https://github.com/NIB-SI/Liftoff/blob/master/liftoff/run_liftoff.py)

```
   aligngrp.add_argument(
        '-flank', default=0, metavar='F', type=float, help="amount of flanking sequence to align as a "
                                                           "fraction [0.0-1.0] of gene length. This can improve gene "
                                                           "alignment where gene structure  differs between "
                                                           "target and "
                                                           "reference; by default F=0.0")
```
[https://github.com/NIB-SI/Liftoff/blob/master/liftoff/extract_features.py](https://github.com/NIB-SI/Liftoff/blob/master/liftoff/extract_features.py)

```
def write_gene_sequences_to_file(chrom_name, reference_fasta_name, reference_fasta_idx, parents, fasta_out, args):
...
            parent.start = round(max(1, parent.start - args.flank * gene_length))
            parent.end = round(min(parent.end + args.flank * gene_length, len(chrom_seq)))
            parent_seq = chrom_seq[parent.start - 1: parent.end]
...
```
```
def extract_features_to_lift():
    ...
    get_gene_sequences()
    ...

def get_gene_sequences():
    ...
    write_gene_sequences_to_file
    ...
```
[https://github.com/NIB-SI/Liftoff/blob/master/liftoff/liftover_types.py](https://github.com/NIB-SI/Liftoff/blob/master/liftoff/liftover_types.py)
```
from liftoff import fix_overlapping_features, lift_features, liftoff_utils, align_features, extract_features
    ...
    extract_features.extract_features_to_lift()
    ...
```
