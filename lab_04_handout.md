# **Lab 4: Genome Annotation and Functional Analysis**

## **Logging into EDI**

```bash
ssh your_username@lbs.cent.uw.edu.pl  # Good
ssh edi00_jupyterlab  # Better
```

Ensure you update your `.ssh/config` if you change your seat.

## **Working with JupyterLab**

```bash
conda activate jupyter_lab
jupyter lab --no-browser --port=your_port
```

Open `localhost:your_port` in a web browser.

---

# **Structural Annotation**

## **Installing Augustus**

```bash
conda create -n augustus_env -c bioconda -c conda-forge augustus gsl=2.5 boost=1.60
conda activate augustus_env
augustus
```

## **Installing GeneMark-ES**

1. Download the software and key from:
   [GeneMark Website](http://topaz.gatech.edu/GeneMark/license_download.cgi)
2. Move both files to the server, in your home directory.
3. Setup GeneMark-ES:

```bash
gzip -d gm_key_64.gz
mv gm_key_64 .gm_key
tar -xvzf gmes_linux_64_4.tar.gz
echo 'export PATH=$HOME/gmes_linux_64_4:$PATH' >> ~/.bashrc
source ~/.bashrc
```

4. Install GeneMark environment:

```bash
conda env create -f genemark_env.yml
conda activate genemark_env
gmes_petap.pl
```

## **Running Structural Annotation**

Copy the input file:

```bash
cp /home/nfs/teaching/tgt_2025/lab_4/biggest_contig.fasta <desired_location>
```

Run Augustus **(with SLURM, 1 CPU, 5GB RAM)**:

```bash
conda activate augustus_env
augustus --species=rhizopus_oryzae --gff3=on --outfile=biggest_contig.augustus_output.gff3 biggest_contig.fasta
```

Run GeneMark-ES **(with SLURM, 10 CPU, 5GB RAM)**:

```bash
conda activate genemark_env
mkdir genemark_output
gmes_petap.pl --sequence biggest_contig.fasta --ES --fungus --cores 10 --work_dir genemark_output/biggest_contig.genemark
```

## **RNAseq Alignments**

Run the alignment in the genome_assembly_lab1 conda environment.

```bash
conda activate genome_assembly_lab1
conda install -c conda-forge -c bioconda bwa gffread samtools
```

Build BAM alignment file **(with SLURM, 10 CPU, 5GB RAM)**:

```bash
bwa index biggest_contig.fasta
bwa mem -t 8 biggest_contig.fasta /home/nfs/teaching/tgt_2025/lab_4/Umbe_pre_R1_001.fastq.gz /home/nfs/teaching/tgt_2025/lab_4/Umbe_pre_R2_001.fastq.gz > biggest_contig.sam # mapping RNAseq reads agains the biggest contig fasta
samtools view -bS biggest_contig.sam | samtools sort -o biggest_contig.sorted.bam # converting SAM to BAM and sorting
samtools view -b -F 4 biggest_contig.sorted.bam > biggest_contig.sorted.mapped.bam # removing non-mapped reads
samtools index biggest_contig.sorted.mapped.bam # indexing the BAM file
```

## **Evaluating Annotation with IGV**

```bash
samtools faidx biggest_contig.fasta
```

Upload files to [IGV Web App](https://igv.org/app/) and analyze results.

---

# **Functional Annotation**

## **Extracting Protein Sequences**

```bash
gffread -g biggest_contig.fasta -y biggest_contig.protein.fa biggest_contig.augustus_output.gff3
```

Select 10 proteins with RNAseq evidence in IGV and copy them into a new FASTA file.

## **InterProScan Analysis**

Go to [InterProScan](https://www.ebi.ac.uk/interpro/search/sequence/) and submit the protein sequences.

Questions:
- Do you have any hits?
- How many proteins have hits to all databases?
- Explore tabluar format (TSV)

## **EggNOG-mapper Analysis**

Go to [EggNOG-mapper](http://eggnog-mapper.embl.de/) and submit the protein sequences.

Questions:
- Are the results overlapping with InterProScan?
- Do you see information that are unique to each approach?

## **KAAS Metabolic Pathway Mapping**

Submit the full genome protein set at [KAAS](https://www.genome.jp/kaas-bin/kaas_main).

Use the following full genome protein set:

```bash
/home/nfs/teaching/tgt_2025/lab_4/Umbelopsis.proteins.fa
```

Use **Eukaryotic reference gene set**.

[Computed results](https://www.genome.jp/kaas-bin/kaas_main?mode=user&id=1742384212&key=ke6ViRLm
)

Questions:
- Is the Krebs cycle functional?
- Can our fungus assimilate ethanol?


---

# **Contact Information**

- **Dr. Mikołaj Dziurzyński** - m.dziurzynski@uw.edu.pl
- **Dr. Maciej Kotliński** - m.kotlinski@uw.edu.pl
