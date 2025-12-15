# MLinvitroTox Tutorial

MLinvitroTox performs high-throughput hazard-based prioritization of high-resolution mass spectrometry data. In this tutorial, we show how to work with the package.

## A. Project description

MLinvitroTox is an open-source Python package developed to provide a fully automated high-throughput pipeline for hazard-driven prioritization of toxicologically relevant signals among tens of thousands of signals commonly detected in complex environmental samples through nontarget high-resolution mass spectrometry (NTS HRMS/MS). It is a machine learning (ML) framework comprising 490 independent XGBoost classifiers trained on molecular fingerprints from chemical structures and target specific endpoints from the ToxCast/Tox21 [invitroDBv4.1 database](https://www.epa.gov/comptox-tools/exploring-toxcast-data). In contrast to the classical approaches for ML-based toxicity prediction, MLinvitroTox predicts a bioactivity fingerprint for each unidentified HRMS feature (a distinct m/z ion) based on the molecular fingerprints derived from MS2 fragmentation spectra, rather than its chemical structure. The 490-bit binary bioactivity fingerprints are used as the basis for prioritizing the HRMS features towards further elucidation and analytical confirmation. This approach adds toxicological relevance to environmental analysis by focusing the time-consuming molecular identification efforts on features most likely to cause adverse effects instead of the most intense ones. MLinvitroTox enhances the interpretability by providing applicability domain, prediction probabilities, model accuracy, and cumulative contribution of endpoints for mechanistic targets, as well as feature importance analysis. In addition to its core functionality of predicting bioactivity from molecular fingerprints derived from MS2 data, the full release of MLinvitroTox will also support:

- standardization of custom molecular structures
- generation of molecular fingerprints for custom molecular structures
- prediction of bioactivity from structures [smiles](https://archive.epa.gov/med/med_archive_03/web/html/smiles.html)
- validatation of SIRIUS' accuracy in predicting molecular fingerprints


## B. Getting started

In its current form, MLinvitroTox can only be used from a terminal as a **command line interface (CLI)**.

Should you be unfamiliar with Python, mamba (conda) and/or git, you can work on a **renku session** (see below). 


### Prerequisites for **CLI**

To work with MLinvitroTox as a CLI, you need:

- Python 3: [installation](https://realpython.com/installing-python/)
- mamba or conda. We recommend to use mamba, a CLI tool to manage conda environments. The conda guide is still helpful for mamba, simply replace `conda` with `mamba` for any command.
    - mamba: [installation](https://mamba.readthedocs.io/en/latest/installation/mamba-installation.html), [guide](https://mamba.readthedocs.io/en/latest/index.html)
    - conda: [installation](https://conda.io/projects/conda/en/latest/user-guide/install/index.html), [guide](https://towardsdatascience.com/a-guide-to-conda-environments-bc6180fc533)
- git with an ssh connection: [installation](https://git-scm.com/downloads), [setup ssh connection](https://docs.gitlab.com/ee/user/ssh.html), [guide](https://www.baeldung.com/ops/git-guide)
- a terminal: [guide](https://medium.com/@grace.m.nolan/terminal-for-beginners-e492ba10902a)

Working with terminals on Windows can be cumbersome. We recommend to use WSL, the Windows Subsystem for Linux [installation](https://learn.microsoft.com/en-us/windows/wsl/install), [installation video](https://youtu.be/WrJVlIrutow?si=rzgd549F635XOncO&t=523). This provides access to a full Ubuntu terminal environment.


### Work from a **renku session**

A renku session allows you to test and work with MLinvitroTox without having to install Python, mamba and git. 

To start a session, go to the [repository page](https://renkulab.io/p/lena.aisch/mlinvitrotox-tutorial) and press the green Launch button on the top right. Starting the session might take a minute. 

It will open a JupyterLab session in your browser. There, you click on the `terminal` button to open a terminal. 

Now, you can directly start with the [Usage section](#usage-cli-and-renku) below and can skip the setup of the tutorial repository.

If you are using the provided renku project with your own data, you must first upload the data as a zip file via the interactive menu on the left. Be sure to download the generated results before closing the session. If you create a free Renku account, you can clone the public tutorial project and use Git to save any changes to the code and data you generate.

If you want to take advantage of additional functionality offered by renku projects, we recommend [creating a free account](https://renkulab.io). This will enable you to clone the publicly available MLinvitroTox project, upload your own data, and save predictions for further use.


### Setup the tutorial repopsitory (needed for CLI)

If you like to work locally, open a terminal and move to the folder in which you would like to work.

```
cd path/to/work/folder
```

Use git to clone the repository
```
git clone git@gitlab.renkulab.io:expectmine/mlinvitrotox-tutorial.git
cd mlinvitrotox-tutorial
```

Create a conda environment. We recommend to use mamba.

```
mamba env create -f ./environment.yml
```

Activate the environment. This needs to be done each time. 
```
mamba activate mlinvitrotox-tutorial
```

## C. Usage (CLI and renku)

After the repository and the environment are setup, or when you are in a renku session, you can get started with analyzing HRMS data.

To show the basic functionality, there is a small sample dataset in the `data` folder, called `sampledata`.


### Extract the SIRIUS data

As the first step, you add the output from [SIRIUS](https://bio.informatik.uni-jena.de/software/sirius/). In addition to the output that is automatically generated by SIRIUS, make sure that you use the option of `export summaries`. 

The SIRIUS data can be provided in two ways:

1. as a folder. Move or copy your SIRIUS output folder to the `data` folder. If you do not have data at the moment, you can skip the moving/copying and extract the sample data.

```
mv /path/to/your/sirius/folder data/
```

or 

```
cp -r /path/to/your/sirius/folder data/
```

You then run the following command to extract it. It is being extracted in the same folder. Here, we use the sample dataset. 

```
itox extract -i data/sampledata
```

If you don't want to move or copy your SIRIUS output, it can also be extracted in a folder outside of the repository. In the load step, you will then need to specify the same path.

```
itox extract -i /path/to/your/sirius/folder
```


2. as a zip folder. We recommend to keep the zip filder outside of the repository's directory structure. To extract, run the following command. The data will then be unzipped and extract to the `data/sampledata` folder.

```
itox extract -i path/to/zipfile/sampledata.zip -o data/
```

Depending on the size of the SIRIUS output, this takes a few seconds up to a several minutes.


### Load the SIRIUS data

Then, the SIRIUS data needs to be loaded, i.e., the predicted fingerprints and other information are collected and stored in the `sirius-pred-fps.csv` file in the specified results folder. 

```
itox load -i data/sampledata -o results/sampledata/sirius-pred-fps.csv
```

Alternatively, true SIRIUS fingerprints can be loaded from SMILES. 

```
itox loadsmiles -i data/predictfromstructure/file-with-smiles.csv -o results/predictfromstructure/true-fps.csv
```

The file needs to contain an ID and a SMILES column. The column names can be specified if they differ from the defaults "ID" and "SMILES".

```
itox loadsmiles -i data/predictfromstructure/file-with-smiles.csv -d "ID" -s "SMILES" -o results/predictfromstructure/true-fps.csv
```


### Run the models

Run the models on your predicted fingerprints. When you use this command for the first time, it is going to download the model from [zenodo](https://zenodo.org/records/13323297). The models are provided as a zipped folder (with the abbreviation `.itox`). Currently, there is one set of models with the name `mlinvitrotox_model.itox`.

```
itox run -m mlinvitrotox_model -i results/sampledata/sirius-pred-fps.csv -o results/sampledata
```

or
```
itox run -m mlinvitrotox_model.itox -i results/sampledata/sirius-pred-fps.csv -o results/sampledata
```

The models can also be run on true fingerprints.
```
itox run -m mlinvitrotox_model.itox -i results/predictfromstructure/true-fps.csv -d "ID" -o results/predictfromstructure
```


### View the results

To view the results, have a look at the output csv file `mlinvitrotox_model_predictions.csv` in `results/sampledata/`.

Column names explanations:

- **aeid** assay endpoint identifier from ToxCast/Tox21 [invitroDBv4.1 database](https://www.epa.gov/comptox-tools/exploring-toxcast-data)
- **chem_id** a feature identifier created as follows sirius-id_mgf-file-name_mzmine-id_formula_adduct
- **prediction** a binary activity (1) or nonactivity (0) prediction generated from column **probability** using a threshold of 0.5
- **probability** the continuous activity probability 
- **similarity** cosine-based similarity of the feature to the training data (based on molecular fingerprints). 
- **MechanisticTarget** the biologically meaningful effect of an endpoint mapped according to [NICEATM](https://ncim.nci.nih.gov/ncimbrowser/)
- **signal_direction** `gain` for agonistic effects, `loss` for antagonistic effects, and `both` for bidirectional endpoints. 
- **endpoint_score** the fraction of active hitcalls divided by the total number of endpoints per mechanistic target (for a feature)
- **endpoint_count** the total number of endpoints associated with a mechanistic target 


### More information

All the commands can also be run with `mlinvitrotox` instead of `itox`. 

MLinvitroTox will work with SIRIUS output up to [v5.8.6](https://github.com/bright-giant/sirius/releases/tag/v5.8.6), but not later releases (work in progress). 



## References
- Arturi, K., Harris, E.J., Gasser, L. et al. MLinvitroTox reloaded for high-throughput hazard-based prioritization of high-resolution mass spectrometry data. J Cheminform 17, 14 (2025). https://doi.org/10.1186/s13321-025-00950-4.

- Arturi, Katarzyna, and Juliane Hollender. "Machine learning-based hazard-driven prioritization of features in nontarget screening of environmental high-resolution mass spectrometry data." Environmental Science & Technology 57, no. 46 (2023): 18067-18079.

- Dührkop, Kai, Markus Fleischauer, Marcus Ludwig, Alexander A. Aksenov, Alexey V. Melnik, Marvin Meusel, Pieter C. Dorrestein, Juho Rousu, and Sebastian Böcker. "SIRIUS 4: a rapid tool for turning tandem mass spectra into metabolite structure information." Nature methods 16, no. 4 (2019): 299-302.

- Abedini, Jaleh, Bethany Cook, Shannon Bell, Xiaoqing Chang, Neepa Choksi, Amber B. Daniel, David Hines et al. "Application of new approach methodologies: ICE tools to support chemical evaluations." Computational Toxicology 20 (2021): 100184.

- Richard, Ann M., Richard S. Judson, Keith A. Houck, Christopher M. Grulke, Patra Volarath, Inthirany Thillainadarajah, Chihae Yang et al. "ToxCast chemical landscape: paving the road to 21st century toxicology." Chemical research in toxicology 29, no. 8 (2016): 1225-1251.

