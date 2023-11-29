# custom_gbk_project_bgcflow
An example of a customized BGCFlow project with genbank inputs

## Requirements
Have bgcflow set up

## Usage
### Running the notebooks
1. Clone the repo
```bash
git clone https://github.com/matinnuhamunada/custom_gbk_project_bgcflow.git
cd custom_gbk_project_bgcflow
```
2. (Optional) Create conda environment
`mamba env create -f env.yaml`
3. (Optional) Run the notebooks
The notebook will prepare custom genbanks file required for BGCFlow
```
mamba activate bgc_analytics
jupyter lab
```
### Synchronizing with BGCFlow
1. Copying the project
Copy the content of this folder inside BGCFlow's config directory:
```bash
BGCFLOW_DIR="../bgcflow" #change accordingly
PROJECT_DIR="$BGCFLOW_DIR/config/frida"
mkdir $PROJECT_DIR
cp * $PROJECT_DIR/.
```
2. Add the project PEP to BGCFlow global config file
3. Checkout to development branch
```bash
cd $BGCFLOW_DIR
git checkout dev-0.8.0-1
```
### Creating custom profiles
Profiles can be used to override Snakefile workflow parameter or resources according to machine availability
### Running panoptes to monitor the workflow
Use tmux and run `bgcflow serve --panoptes`
### Executing the project
Instead of the BGCFlow CLI, directly use snakemake CLI for flexibility
```bash
snakemake --use-conda -c 64 --snakefile workflow/Snakefile --profile config/frida/profile
```