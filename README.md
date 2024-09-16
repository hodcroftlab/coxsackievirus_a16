# Coxsackievirus A16 Nextstrain Analysis

This repository provides a comprehensive Nextstrain analysis of Coxsackievirus A16. You can choose to perform either a **VP1 run (>=600 base pairs)** or a **whole genome run (>=6400 base pairs)**.

For those unfamiliar with Nextstrain or needing installation guidance, please refer to the [Nextstrain documentation](https://docs.nextstrain.org/en/latest/).

### Enhancing the Analysis
Most of the data for this analysis can be obtained from [NCBI Virus](https://www.ncbi.nlm.nih.gov/labs/virus/vssi/#/). Instructions for downloading sequences are provided at the end of this README under [Sequences](#sequences). 

## Repository Organization
This repository includes the following directories and files:

- `ingest`: Contains Python scripts and the `snakefile` for automatic downloading of CVA16 sequences and metadata.
- `scripts`: Custom Python scripts called by the `snakefile`.
- `snakefile`: The entire computational pipeline, managed using Snakemake. Snakemake documentation can be found [here](https://snakemake.readthedocs.io/en/stable/).
- `vp1`: Sequences and configuration files for the **VP1 run**.
- `whole_genome`: Sequences and configuration files for the **whole genome run**.

### Configuration Files
The `config`, `vp1/config`, and `whole_genome/config` directories contain necessary configuration files:
- `colors.tsv`: Color scheme
- `geo_regions.tsv`: Geographical locations
- `lat_longs.tsv`: Latitude data
- `dropped_strains.txt`: Dropped strains
- `clades_genome.tsv`: Virus clade assignments
- `reference_sequence.gb`: Reference sequence
- `auspice_config.json`: Auspice configuration file

The reference sequence used is [G-10, accession number U05876](https://www.ncbi.nlm.nih.gov/nuccore/U05876).

## Quickstart

### Setup

#### Nextstrain Environment
Install the Nextstrain environment by following [these instructions](https://docs.nextstrain.org/en/latest/guides/install/local-installation.html).

### Running a Build

Activate the Nextstrain environment:
```
conda activate nextstrain
```

To perform a build, run:
```
snakemake --cores 1
```

For specific builds:
- VP1 build:
    ```
    snakemake auspice/cv_a16_vp1.json --cores 1
    ```
- Whole genome build:
    ```
    snakemake auspice/cv_a16_whole_genome.json --cores 1
    ```

### First steps
To run the ingest, you will need some specific reference files, such as a `reference.fasta` or `annotation.gff3` file.
1. In the `config` file: check that the taxid is correct
2. To get these files you have to run the script [generate_from_genbank.py](ingest/bin/generate_from_genbank.py) manually. 
    ```
    python3 ingest/generate_from_genbank.py --reference "U05876.1" --output-dir "whole_genome/config/"
    ```
    - You need to specify a few things: [0];[product];[2].
    - It will create the files in the subdirectory `data/references`. 
    - These files will be used by the `ingest` snakefile.
3. Check that the `attributes` in `data/references/pathogen.json` are up to date.
4. Run the `ingest' snakefile (either manually or using the main snakefile).
    - Depending on your system you may need to run `chmod +x ./vendored/*; chmod +x ./bin/*` first.
5. Run the main snakefile.

### Visualizing the Build
To visualize the build, use Auspice:
```
auspice view --datasetDir auspice
```
To run two visualizations simultaneously, you may need to set the port:
```
export PORT=4001
```

### Sequences
Sequences can be downloaded manually or automatically.

1. **Manual Download**: Visit [NCBI Virus](https://www.ncbi.nlm.nih.gov/labs/virus/vssi/#/), search for `CVA16` or Taxid `31704`, and download the sequences.
2. **Automated Download**: The `ingest` functionality, included in the main `snakefile`, handles automatic downloading.

The ingest pipeline is based on the Nextstrain [RSV ingest workflow](https://github.com/nextstrain/rsv.git). Running the **ingest** pipeline produces `data/metadata.tsv` and `data/sequences.fasta`.

### Updating Vendored Scripts
This repository uses [`git subrepo`](https://github.com/ingydotnet/git-subrepo) to manage copies of ingest scripts in `ingest/vendored`. To pull new changes from the central ingest repository, first install `git subrepo` and then follow the instructions in [ingest/vendored/README.md](./ingest/vendored/README.md#vendoring).

## Feedback
For questions or comments, contact me via GitHub or [nadia.neuner-jehle@swisstph.ch](mailto:nadia.neuner-jehle@swisstph.ch).