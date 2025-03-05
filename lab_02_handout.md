
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

## 6. Additional Notes

- Refer to the official [BUSCO documentation](https://busco.ezlab.org/) for detailed command options and lineage datasets.

