

**Bash Commands for Lab 1: Genomics and Transcriptomics Techniques**

### **Introduction**

- link to questionnaire: [https://forms.gle/7eYuNTWWHRWNAtcX9](https://forms.gle/7eYuNTWWHRWNAtcX9)
- link to EDI cluster docs: [https://labstructbioinf.github.io/edi/](https://labstructbioinf.github.io/edi/)

### **1. Logging into the Server**
To access the `edi00` server, follow these steps:

- **Connect to the gateway first:**  
  ```bash
  ssh your_username@lbs.cent.uw.edu.pl
  ```
  _Logs into the jumphost server._

- **Login into edi00:**  
  ```bash
  ssh edi00
  ```
  _Logs into the edi00 server after connecting to the gateway._

- **Change your password:**  
  ```bash
  passwd your_username
  ```
  _Updates your password (write it down or take a picture)._ 

**If you forget your password you won’t be able to participate in the classes!**

---

### **2. Installing Conda**
**Step 1: Download and install Miniconda**
  ```bash
  mkdir -p ~/miniconda3
  wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
  bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
  rm ~/miniconda3/miniconda.sh
  ```
_Creates a directory, downloads Miniconda, installs it, and removes the installer._

**Step 2: Restart terminal and log in again.**


**Step 3: Initialize Conda**
  ```bash
  source ~/miniconda3/bin/activate
  conda init --all
  ```
_Activates Conda and initializes it for all shells._

**Step 4: Set up environment**
  ```bash
  conda create -n genome_assembly_lab1 -c bioconda -c conda-forge flye spades quast megahit sra-tools fastp fastqc nanoplot jupyterlab
  ```
_Creates a Conda environment with necessary genomics tools._

---

### **3. Working with JupyterLab**
JupyterLab requires **port forwarding**.

**Step 1: Open SSH configuration file on your PC**
  ```bash
  nano ~/.ssh/config
  ```
_Edit the SSH configuration file._

**Step 2: Add the following configuration**
  ```
  host edi_jumphost
    HostName lbs.cent.uw.edu.pl
    User your_username
    LocalForward your_port 127.0.0.1:your_port
  
  host edi00_jupyterlab
    HostName 10.10.61.23
    ProxyJump edi_jumphost
    User your_username
    LocalForward your_port 127.0.0.1:your_port
  
  Host *
    ServerAliveInterval 40
  ```
_Sets up port forwarding for JupyterLab._

**Step 3: SSH into the server in a new terminal**
  ```bash
  ssh edi00_jupyterlab
  ```
_Connects to edi00 through the configured SSH setup._

**Step 4: Activate Conda and start JupyterLab**
  ```bash
  conda activate genome_assembly_lab1
  jupyter lab --no-browser --port=your_port
  ```
_Activates the environment and starts JupyterLab._

**Step 5: Open JupyterLab in a browser fe. Firefox**
  ```
  localhost:your_port
  ```
_Access JupyterLab from your local machine._

---

### **4. Genome Assembly Workflow**
#### **Step 1: Getting Reads**
Reads are available in **GenBank Sequence Read Archive (SRA):**
- Illumina: `SRR26874108`
- Nanopore: `SRR26874107`

**The files are aleady downloaded and are available in this folder:**
  ```bash
  /home/nfs/teaching/tgt_2025/lab_1/data
  ```

**To download the reads run the following commands in your conda _genome_assembly_lab1_**:
  ```bash
  fastq-dump -v --split-files SRR26874108
  fastq-dump -v SRR26874107
  ```

**Create symbolic links in your home directory**
  ```bash
  ln -s <file/folder_path> <link>
  ```
_Creates a shortcut to the read files in your home directory._

#### **Step 2: Quality Control**


**Run NanoPlot for Nanopore data:**
  ```bash
  NanoPlot --verbose --fastq nanopore_data --outdir nanoplot_output -t 4
  ```
_Prepares a report to assess Nanopore data quality._

**Run fastp for Illumina data:**
  ```bash
  fastp \
    -i illumina_R1_input \
    -I illumina_R2_input \
    -o illumina_R1_output.gz \
    -O illumina_R2_output.gz \
    -q 20 \
    --detect_adapter_for_pe \
    --correction \
    -w 5
  ```
  
_Performs quality control and trimming._

#### **Step 3: Genome Assembly**
- **MEGAHIT (short-read assembly):**
  ```bash
  megahit --min-contig 500 -t 8 -1 input_illumina_R1 -2 input_illumina_R2 -o megahit_illumina
  ```

- **SPAdes (short-read assembly):**
  ```bash
  spades.py -t 10 -m 100 --only-assembler -1 input_illumina_R1 -2 input_illumina_R2 -k 21,33,55,77,99,121 -o spades_illumina
  ```

- **SPAdes (hybrid assembly):**
  ```bash
  spades.py --only-assembler -1 input_illumina_R1 -2 input_illumina_R2 --nanopore nanopore_data -o spades_hybrid_output -k 21,33,55,77,99,121 -t 10 -m 100
  ```

- **Flye (long-read assembly):**
  ```bash
  flye --threads 10 --nano-raw nanopore_data -o flye_nanopore
  ```

#### **Step 4: Assembly Quality Analysis**
- **QUAST:**
  ```bash
  quast -o quast_output -t 8 {path_to_assembly_fasta_1} {path_to_assembly_fasta_2} etc.
  ```
  _Prepares a comprehensive report that compares assemblies._

Open report.html in quast_output and compare the assemblies.
---


_Prepared by Mikołaj Dziurzyński, 26.02.2025, University of Warsaw_


