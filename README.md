# custom_gbk_project_bgcflow
An example of a customized BGCFlow project with genbank inputs

## Requirements
Have bgcflow set up, see [quickstart guide](https://github.com/NBChub/bgcflow/wiki#quick-start)
Make sure to initiate the template `config.yaml` with `bgcflow init`

## Usage
### Running the notebooks
1. Clone the repo
```bash
git clone https://github.com/matinnuhamunada/custom_gbk_project_bgcflow.git
cd custom_gbk_project_bgcflow
```
2. (Optional) Create conda environment
```bash
mamba env create -f env.yaml
```
3. (Optional) Run the notebooks
The notebook will prepare custom genbanks file required for BGCFlow
```
mamba activate bgc_analytics
jupyter execute notebooks/00_data_cleanup.ipynb
jupyter execute notebooks/01_taxonomy.ipynb
```
### Synchronizing with BGCFlow
1. Copying the project
Copy the content of this folder inside BGCFlow's config directory:
```bash
BGCFLOW_DIR="../bgcflow" #change accordingly
PROJECT_DIR="$BGCFLOW_DIR/config/frida"
mkdir -p $PROJECT_DIR
cp custom_taxonomy.tsv project_config.yaml samples.csv input_files $PROJECT_DIR -r
```
2. Add the project PEP to BGCFlow global config file
Open the global config file (`config/config.yaml`) and add the PEP file for this project:
```yaml
# This file should contain everything to configure the workflow on a global scale.

#### PROJECT INFORMATION ####
# This section control your project configuration.
# Each project are separated by "-".
# A project can be defined as (1) a yaml object or (2) a Portable Encapsulated Project (PEP) file.
#   - pep: path to PEP .yaml file. See project example_pep for details.
# PS: the variable pep and name is an alias

projects:
# Project 2 (PEP file)
  #- pep: config/Lactobacillus_delbrueckii/project_config.yaml
  - pep: config/frida/project_config.yaml
bgc_projects:
  - pep: config/lanthipeptide_lactobacillus/project_config.yaml
```
3. Creating custom profiles
Profiles can be used to override Snakefile workflow parameter or resources according to machine availability. An example profile is located [here](profile/config.yaml).
The content of the file looks like this:
```yaml
set-threads:
  antismash: 16
  bigscape: 64
```
This will override the default antismash core usage when the profile is being used.

To copy the example to your bgcflow directory, do:
```bash
cp profile $BGCFLOW_DIR -r
```
4. Checkout to development branch
```bash
cd $BGCFLOW_DIR
git checkout dev-0.8.0-1
```

### Final structure
The final structure should look like this:
```bash
.
├── config
│   ├── config.yaml
│   └── frida
│       ├── custom_taxonomy.tsv
│       ├── input_files
│       │   ├── NW_003318762.1_0_43680_changed_species.gbk
│       │   ├── NW_003318762.1_0_43680_changed_species.txt
│       │   ├── NW_003385442.1_0_46725_changed_species.gbk
│       │   ├── NW_003385442.1_0_46725_changed_species.txt
│       │   ├── NW_019110264.1_0_31676_changed_species.gbk
│       │   ├── NW_019110264.1_0_31676_changed_species.txt
│       │   ├── NZ_KE386655.1_0_25886_changed_species.gbk
│       │   └── NZ_KE386655.1_0_25886_changed_species.txt
│       ├── project_config.yaml
│       └── samples.csv
├── profile
│   └── config.yaml
...
```

### (Optional) Running panoptes to monitor the workflow
Use tmux and run `panoptes`
```bash
tmux new-session -s panoptes
conda activate bgcflow
bgcflow serve --panoptes
```
detach with `ctrl+b` followed by `d`.

### Executing the project
Instead of the BGCFlow CLI, directly use snakemake CLI for flexibility
```bash
snakemake \
  --snakefile workflow/Snakefile \
  --use-conda \
  --keep-going \
  --rerun-incomplete \
  --rerun-triggers mtime \
  -c 64 \
  --wms-monitor http://127.0.0.1:5000 \
  --profile profile \
  --dryrun
```
Remove the `--dryrun` flag to run the job.
