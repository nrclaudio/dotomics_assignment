# Dotomics Assignment

Notebook workflows to slice a reference chromosome into fixed windows, analyze GC content, export the windows for downstream use, and embed each window with the Agro Nucleotide Transformer (1B). A helper script prints model config details from Hugging Face.

## Setup
- Clone the repo, then create and activate a conda environment:
  ```bash
  conda create -y -n dotomics python=3.10
  conda activate dotomics
  ```
- Install pinned, hash-checked dependencies:
  ```bash
  pip install --require-hashes -r requirements.txt
  ```

- If you ever need to regenerate the lockfile, use `requirements.in` with pip-tools:
  ```bash
  python -m pip install pip-tools
  pip-compile --generate-hashes --output-file requirements.txt requirements.in
  ```


## How to run
- **Windowing / GC analysis:** open `notebooks/fastq_exploration.ipynb` and run all cells. It loads `data/Arabidopsis_thaliana.TAIR10.dna.chromosome.1.fa.gz`, slices 1 kb windows (full coverage by default), visualizes GC, and exports:
  - `outputs/windows.fasta`
  - `outputs/windows.tsv`
- **Embedding extraction:** open `notebooks/extract_embeddings_agro_transformer.ipynb`. Set `windows_fasta_path`/`windows_tsv_path` to your exported files in `outputs/`, then run all cells. It saves `outputs/window_embeddings_agro_transformer.npz` (`ids`, `embeddings`). Optional cells merge GC metadata and compute a quick correlation.
- **Model config helper:** run `python scripts/print_model_config.py` (or `--model your/model-id`) to print architecture fields.

## Project structure
- `data/Arabidopsis_thaliana.TAIR10.dna.chromosome.1.fa.gz`: input chromosome FASTA.
- `notebooks/fastq_exploration.ipynb`: windowing, GC/N stats, plots, export to FASTA/TSV.
- `notebooks/extract_embeddings_agro_transformer.ipynb`: load exported windows, embed with Agro Nucleotide Transformer, save NPZ, GC join/correlation.
- `outputs/`: window exports and embedding artifacts.
- `requirements.txt`: Python dependencies.

## Input data
The repo does not bundle the reference FASTA to keep size small. Download chromosome 1 from Ensembl Plants and place it in `data/`:
```bash
mkdir -p data
curl -L -o data/Arabidopsis_thaliana.TAIR10.dna.chromosome.1.fa.gz \\
  https://ftp.ensemblgenomes.ebi.ac.uk/pub/plants/release-62/fasta/arabidopsis_thaliana/dna/Arabidopsis_thaliana.TAIR10.dna.chromosome.1.fa.gz
```
Then run the notebooks as described above.
