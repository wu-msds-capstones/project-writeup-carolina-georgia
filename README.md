# Menstrual Health Legislation and K-12 Chronic Absenteeism
 
Capstone project examining whether statewide menstrual health and hygiene (MHH) legislation is associated with reduced female-specific chronic absenteeism in US public schools.
 
**Authors:** Carolina Pinillos, Georgia Kirkpatrick

**For reviewers:** the live rendered site is available at [wu-msds-capstones.github.io/project-writeup-carolina-georgia](https://wu-msds-capstones.github.io/project-writeup-carolina-georgia/). To run the code yourself, `capstone.qmd` is the file to render — it includes all seven section files (`_01_intro.qmd` through `_07_references.qmd`) in order and controls the shared execution environment; the individual section files are not meant to be rendered on their own.
 
## Setup
 
This project uses [Quarto](https://quarto.org/) with a Python/Jupyter execution engine and a conda environment. Follow these steps to get it running on a new machine.
 
### 1. Install prerequisites
 
- [Miniconda](https://docs.conda.io/en/latest/miniconda.html) (or Anaconda)
- [Quarto CLI](https://quarto.org/docs/get-started/)

### 2. Clone the repo and create the environment
 
```bash
git clone https://github.com/wu-msds-capstones/project-writeup-carolina-georgia.git
cd project-writeup-carolina-georgia
conda env create -f environment.yml
```
 
### 3. Activate the environment and register it as a Jupyter kernel
 
Quarto renders this project's code cells using a kernel named `capstone` (set in `capstone.qmd`'s YAML header). Creating the conda environment alone does **not** register it with Jupyter — this step does:
 
```bash
conda activate capstone
python -m ipykernel install --user --name capstone --display-name "capstone"
```
 
Confirm it registered correctly:
 
```bash
jupyter kernelspec list
```
 
You should see `capstone` in the list.
 
### 4. Render the project
 
```bash
quarto render capstone.qmd
```
 
This produces the rendered output in `_output/`.
 
### 5. Publish to GitHub Pages
 
```bash
quarto publish gh-pages
```
 
This renders the project fresh and pushes the output to the `gh-pages` branch, which is what serves the live site.
 
## Project structure
 
- `capstone.qmd` — top-level manuscript file; sets project-wide YAML config and includes each section below in order
- `_01_intro.qmd` through `_07_references.qmd` — individual sections (Introduction, Background, Data Engineering, Analysis, Results, Conclusions, References)
- `references.bib` — bibliography; all citation entries live here (not in `_07_references.qmd`)
- `capstone_traditional.db` — SQLite database used throughout the analysis
- `menstrual_health_legislation_50states.csv` — state-level MHH legislation dataset
- `environment.yml` — conda environment spec; use this to recreate the `capstone` environment

## Troubleshooting
 
**`ERROR: Jupyter kernel 'capstone' not found`**
The conda environment exists locally but hasn't been registered with Jupyter yet on this machine. Run step 3 above.
 
**`ModuleNotFoundError` for a package (e.g. `geopandas`, `statsmodels`)**
The active environment is missing a package. Confirm you're in the `capstone` environment (`conda activate capstone`) and that `environment.yml` is up to date. If a package is missing from `environment.yml` itself, install it manually and re-export:
 
```bash
conda install -c conda-forge <package-name>
conda env export --from-history > environment.yml
```
 
**`CRSError: Invalid projection` or PROJ-related errors**
Usually caused by a stale `PROJ_DATA` or `PROJ_LIB` environment variable pointing at a different conda environment's PROJ install. Check and unset:
 
```bash
echo $PROJ_LIB
echo $PROJ_DATA
unset PROJ_LIB
unset PROJ_DATA
```
 
**`quarto publish gh-pages` says the folder is "not a website, manuscript or book project"**
`_quarto.yml` is missing from the project root, or you're not running the command from the project root. Confirm with `ls _quarto.yml` — if it's missing, restore it with `git restore _quarto.yml` (if deleted locally) or `git pull` (if it's missing from your local branch entirely).