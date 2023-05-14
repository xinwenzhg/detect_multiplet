# Identify multipletes in single cell experiements

### **1. Script usage** 

 The script takes the count matrix file and the gene annotation file as inputs and returns summary plots as a pdf file and a csv report for the number of droplets with 1 or 2+ cells identified.

* Script file: `summarize_droplets.py`

Testing environment:
- Windows 10
- Python 3.8.8

Dependencies:
- `argparse`: Command line argument parser.
- `numpy`: Convenient vector operations.
- `pandas`: Dataframe operations.
- `matplotlib`: Result plotting.

Print the help message as follows:

```sh    
python .\summarize_droplets.py --help
```

```
usage: summarize_droplets.py [-h] -c F1 -g F2 -p F3 -s F4

Process droplets count matrix to summarize multiplex in
given droplets

optional arguments:
  -h, --help  show this help message and exit
  -c F1       the count matrix file path, each row should
              be a gene, each column should be a droplet
  -g F2       the gene_name file path
  -p F3       the output plot file path
  -s F4       the output summary file path
```

Exmaple useage:

```sh
python .\summarize_droplets.py \
        -c .\data\count_matrix.csv \
        -g .\data\peak_names_out.csv \
        -p summary.pdf \
        -s summary.csv
```

The script outputs `summary.csv` and `summary.pdf` as results.

`summary.pdf` shows how the droplets are seperated by total counts of chosen human and mouse genes. If the cut-off points don't look reasonable in fig 1 & 2, try to increase the number of genes used to calculate total counts. 
Current results used 200 human genes and 200 mouse genes.  

To change that number, check `summarize_droplets.py` line 61-62 to change cut point.
```python
hg_gene_cp = sorted(hg_dropCvg,reverse=True)[200] # cp: cut point
mm_gene_cp = sorted(mm_dropCvg,reverse=True)[200]
```

### **2. Validations and common user errors**

- File-level validation: The script checks if (number of genes in `count_matrix.csv`) equals (number of genes in `peak_names_out.csv`). If not, it raises an error and terminates.

- Line-level validation: The script checks every line in count_matrix:
    1. If number of counts doesn't match with number of droplots (i.e., if the number of values in each row matches the length of the header).
    2. If there are non-numeric value in counts.

    In the case of line-level validation failure, it prints out an error message with the line number, and skips the line to continue with the rest of the file. 
