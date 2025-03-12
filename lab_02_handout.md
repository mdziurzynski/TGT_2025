
**Bash Commands for Lab 2: Genomics and Transcriptomics Techniques**

### **Important links**


- link to EDI cluster docs: [https://labstructbioinf.github.io/edi/](https://labstructbioinf.github.io/edi/)


## 1. Logging in to EDI

To connect to the EDI cluster:

- **Preferred method**:
  ```bash
  ssh edi00_jupyterlab
  ```
- **Alternative method**:
  ```bash
  ssh your_username@lbs.cent.uw.edu.pl
  ```
- **Important**:
  - Ensure your `~/.ssh/config` is updated if you’ve changed your seat.
  - If you forget your password, you will not be able to participate in the classes.

---

## 2. Working with JupyterLab

We will use **JupyterLab** to:

- Run basic bash commands
- View command results and reports

To start JupyterLab from the command line:

1. **Activate** your environment (example):
   ```bash
   conda activate
   ```
2. **Launch** JupyterLab:
   ```bash
   jupyter lab --no-browser --port=<your_port>
   ```
3. Open the Jupyter interface by pointing your web browser to:
   ```
   localhost:<your_port>
   ```
   (e.g., `localhost:8888`)

---

## 3. Assembly Completeness and Contamination Assessment

### Why Assess Genome Quality?
- **Contamination** can distort downstream analyses. Tools such as *BlobTools* help identify contamination.
- **Completeness** ensures that the majority of expected genomic content is present.

### Relevant Tools
- **BlobTools**: to detect contamination
- **BUSCO**: for contamination and completeness metrics (e.g., eukaryotic assemblies)
- **CheckM2**: for completeness/contamination of _prokaryotic_ assemblies

---

## 4. About BUSCO

- **BUSCO** (*Benchmarking Universal Single-Copy Orthologs*):
  - Evaluates genome completeness by searching for highly conserved genes (universal single-copy orthologs).
  - Genes are conserved across specific phylogenetic lineages (e.g., *fungi*, *mammalia*, etc.).
  - Results help compare genome quality between different assemblies or species.

---

## 5. Installing and Running BUSCO

### Installation Options

1. **Inside an existing environment** (example: `genome_assembly_lab1`):
   ```bash
   conda install -c conda-forge -c bioconda busco=5.8.2
   ```

2. **Create a dedicated environment**:
   ```bash
   conda create -n busco_env -c conda-forge -c bioconda busco=5.8.2
   conda activate busco_env
   ```

### Running BUSCO

- **Task**:
  1. Run BUSCO on both the **best** and **worst** assemblies from lab 01.
  2. Compare the results.

**Questions to consider**:
- Which lineage dataset did you use, and why?
- What differences in BUSCO metrics do you observe between assemblies?

---

## 6. SLURM

1. **Basic SLURM commands**:

```
sinfo                          # Show information about SLURM partitions
squeue                         # Show current jobs queue
scontrol show job <job_id>     # Get status information of my job
sbatch <your_script_file>.sh   # Run a job
```

2. **SLURM batch template**:
```
#!/bin/bash
#SBATCH --job-name=<your_job_name>         # Job name
#SBATCH --cpus-per-task=10                 # Number of CPU cores
#SBATCH --mem=15G                          # How much RAM is required
#SBATCH -p cpu                             # CPU partition
#SBATCH --output=<std_out_file>.out        # Standard output stream
#SBATCH --error=<std_err_file>.out         # Standard error stream
#SBATCH --nodelist=edi0


# Load Conda environment
export PATH=<your_miniconda3_path>/miniconda3/bin:$PATH          # Adjust to your Conda installation path
source <your_miniconda3_path>/miniconda3/etc/profile.d/conda.sh  # Ensure Conda is initialized

# Activate your environment
conda activate genome_assembly_lab1

# Run your script
<your command(s) to run>
```

---

## 7. Installing and Running BlobTools



1. **Install BlobTools following the official documentation:**
   - [BlobTools GitHub](https://github.com/DRL/blobtools)

2. **Generate BlobTools databases and BlobPlots for the following datasets**:
    - dataset_1

        _/home/nfs/teaching/tgt_2025/lab_2/blobtools/blobtools_dataset_1_
    
    - dataset_2
    
        _/home/nfs/teaching/tgt_2025/lab_2/blobtools/blobtools_dataset_2_


- **Task**:
  1. Adjust commands listed in this tutorial: [My First BlobPlot](https://blobtools.readme.io/docs/my-first-blobplot).
  2. You do not need to run any aligners (blastn, minimap etc.) - all input data is available in the above listed folders!
  3. Compare the results.

**Questions to consider**:
- Is there any contamination in our assemblies?
- How can we identify which contigs are contaminants?

---

## 7. Additional Notes

- Refer to the official [BUSCO documentation](https://busco.ezlab.org/) for detailed command options and lineage datasets.

