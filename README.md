# custom_gbk_project_bgcflow
An example of a customized BGCFlow project with genbank inputs

## Usage
### Running the notebooks
1. Clone the repo
2. Create conda environment
3. Run the notebooks
The notebook will prepare custom genbanks file required for BGCFlow
### Synchronizing with BGCFlow
1. Copying the project
Copy the content of this folder inside BGCFlow's config directory:
```bash
BGCFLOW_DIR="../bgcflow" #change accordingly
PROJECT_DIR="$BGCFLOW_DIR/frida"
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
### Executing the project
Instead of the BGCFlow CLI, directly use snakemake CLI for flexibility
