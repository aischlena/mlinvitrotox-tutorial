# MLinvitroTox Tutorial

How to get started with the MLinvitrotox package

## A. Project description
MLinvitroTox is an open-source Python package designed to provide a fully automated high-throughput pipeline for a hazard-driven prioritization of toxicologically relevant signals among tens of thousands of HRMS signals commonly found in complex environmental samples via nontarget (NTS) screening. MLinvitroTox is a machine learning (ML) framework consisting of 490 independent XGBoost supervised classifiers trained on molecular fingerprints calculated from structures [source](https://www.epa.gov/comptox-tools/distributed-structure-searchable-toxicity-dsstox-database) and target specific endpoints from the ToxCast/Tox21 invitroDBv4.1 database [link](https://www.epa.gov/comptox-tools/exploring-toxcast-data) to predict a 490 bit binary bioactivity fingerpint for each undientified HRMS feature (distinct m/z ions) based its MS2 fragmentation [spectra](https://en.wikipedia.org/wiki/Tandem_mass_spectrometry#:~:text=Tandem%20mass%20spectrometry%2C%20also%20known,abilities%20to%20analyse%20chemical%20samples.) via [SIRIUS](https://bio.informatik.uni-jena.de/software/sirius/). This fingerprints, along with supporting inforamtion, is used as the basis for prioritization of HRMS features towards further elucidation and analytical confirmation, thus adding toxicological relevance to environmental analysis by focusing the time-consuming molecular identification efforts on the features most likely to cause adverse effects. In addition to its core functionality of predicting bioactivity from molecular fingerprints derived from MS2, MLinvitroTox can:

- standardize molecular structures
- generate molecular fingerprints
- predict nioactivity from structures [smiles](https://archive.epa.gov/med/med_archive_03/web/html/smiles.html)
- validate SIRIUS' accuracy in predicting molecular fingerprints
- extract SIRIUS output


## B. Getting started

In its current form, MLinvitroTox can only be used from a terminal as a command line interface (CLI).

Should you be unfamiliar with Python, conda/mamba and/or git, you can work on a renku session (see below). 


### Prerequisites

To work with MLinvitroTox in CLI, you need:

- Python 3: [installation guide](https://realpython.com/installing-python/)
- conda: [installation](https://conda.io/projects/conda/en/latest/user-guide/install/index.html), [guide](https://towardsdatascience.com/a-guide-to-conda-environments-bc6180fc533)
- mamba: [installation](https://mamba.readthedocs.io/en/latest/installation/mamba-installation.html), [guide](https://mamba.readthedocs.io/en/latest/index.html)
- git: [installation](https://git-scm.com/downloads), [guide](https://www.baeldung.com/ops/git-guide)
- a terminal: [guide]()

Working with terminals on Windows can be cumbersome. We recommend to use WSL, the Windows Subsystem for Linux [installation](https://youtu.be/WrJVlIrutow?si=rzgd549F635XOncO&t=523). This gives you access to a full Ubuntu terminal environment.

For users with limited coding skills or lacking the necessary installations, we created a Renku repository for MLinvitroTox. To be able to use it you need:

- A browser
- An internet connection.

If you want to take advantage of additional funcrtionality of Renku proejcts, we reccomend to [create free account](https://renkulab.io), whic hwill enable you to clone the publicly availbale MLinvitroTox, upload your own data, and save the predictions for further use. 


### Work from a renku session

A renku session allows you to test and work with MLinvitroTox without having to install Python, mamba and git. 

To start a session, go to the [repository page](https://renkulab.io/projects/expectmine/mlinvitrotox-tutorial/) and press the green Start button on the top right. Starting the session might take a minute. 

It will open a JupyterLab session in your browser. There, you click on the `terminal` button to open a terminal. 

Now, you can directly start with the [Usage section](#usage) below and can skip the setup of the tutorial repository.

If using the provided Renku project with your own data, the data has to uploaded as a zip file first via the interactive menu on the left. The generated results have to then be downloaded before closing the session. If you start a free Renku account, you can clone the public tutorial project and then use git to save the changes to the code and the data that you generate.

### Setup the tutorial repopsitory

If you like to work locally, open a terminal and move to the folder in which you would like to work.

```
cd path/to/work/folder
```

Use git to clone the repository
```
git clone git@gitlab.renkulab.io:expectmine/mlinvitrotox-tutorial.git
cd mlinvitrotox-tutorial
```

Create a conda environment. We recommend to use mamba, an optimized version of conda.

```
mamba env create -f ./environment.yml
```


## C. Usage

After the repository and the environment are setup, you can get started with analyzing HRMS data.

Activate the environment. This needs to be done each time you work in this repository. 
```
conda activate mlinvitrotox-tutorial
```

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

If you don't want to move or copy your SIRIUS output, it can also be extracted. You will then need to specify the same path in the load step. 
```
itox extract -i /path/to/your/sirius/folder
```


2. as a zip folder. We recommend to keep the zip filder outside of the repository's directory structure. To extract, run the following command. The data will then be unzipped and extract to the `data/sample` folder.

```
itox extract -i path/to/zipfile/sampledata.zip -o data/
```

Depending on the size of the SIRIUS output, this takes a few seconds up to a several minutes.


### Load the SIRIUS data

Then, the SIRIUS data needs to be loaded, i.e., the predicted fingerprints and other information are collected and stored in the `sirius-pred-fps.csv` file in the specified results folder.

```
itox load -i data/sampledata -o results/sampledata/sirius-pred-fps.csv
```


### Run the models

Run the models on your predicted fingerprints. When you use this command for the first time, it is going to download the model from [zenodo](https://zenodo.org/records/13323297). The models are provided as a zipped folder (with the abbreviation `.itox`). 

```
itox run -m mlinvitrotox_model -i results/sampledata/sirius-pred-fps.csv -o results/sampledata
```

or
```
itox run -m mlinvitrotox_model.itox -i results/sampledata/sirius-pred-fps.csv -o results/sampledata
```


### View the results

To view the results, have a look at the output csv files.

TODO explain columns



### More information

All the commands can also be run with `mlinvitrotox` instead `itox`. 

