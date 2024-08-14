# MLinvitroTox Tutorial

How to get started with the MLinvitrotox package

## A. Project description



## B. Getting started

TODO the SIRIUS data and the models still need to be added

### MLinvitroTox is a CLI

In its current form, MLinvitroTox can only be used from a terminal as a command line interface (CLI). 

Should you be unfamiliar with Python, please check: TODO link

Should you be unfamiliar with the terminal and CLIs, please check: TODO link 

Working with terminals on Windows can be cumbersome. If you are working on Windows, we recommend to use WSL, the Windows Subsystem for Linux. This gives you access to a full Ubuntu terminal environment. TODO link

Also, we recommend to use git to download the repository. For more information about git and how to get started with it, please check: TODO link


### Setup the tutorial repopsitory

Open a terminal and move to the folder in which you would like to work.

```
cd path/to/work/folder
```

Use git to clone the repository
```
git clone git@gitlab.renkulab.io:expectmine/mlinvitrotox-tutorial.git
cd mlinvitrotox-tutorial
```

Create a conda environment. We suggest to use mamba, an optimized version of conda.

If you need to install mamba (and conda), see [here](https://mamba.readthedocs.io/en/latest/installation/mamba-installation.html). Should you be unfamiliar with conda environments, please check: TODO link

```
mamba env create -f ./environment.yml
```


If it was successful, you see an output like that.

```
TODO mamba env create output
```


Then, activate the environment. This needs to be done each time you work in this repository. 
```
conda activate mlinvitrotox-tutorial
```


Install the mlinvitrotox package

TODO This step can be skipped, when mlinvitrotox is on pypi (it needs to be added to environment.yml)
```
pip install -i https://test.pypi.org/simple/ mlinvitrotox
```


## C. Example / usage

After the repository and the environment are setup, you can get started with analyzing HRMS data.

To show the basic functionality, there is a small `sample` dataset in the `data` folder.


### Extract the SIRIUS data

As the first step, you add the output from SIRIUS. Make sure to create the SIRIUS output with the summaries.

TODO link SIRIUS

KASIA Could you check whether this description with the summaries make sense?

The SIRIUS data can be provided in two ways:

1. as a folder. Move or copy your SIRIUS output folder to the `data` folder. You then run the following command to extract it. It is being extracted in the same folder. For this tutorial, we use the `sample` dataset. 

```
itox extract -i data/sample
```

2. as a zip folder. We recommend to keep the zip filder outside of the repository's directory structure. To extract, run the following command. The data will then be unzipped and extract to the `data/sample` folder.

```
itox extract -i path/to/zipfile/sample.zip -o data/
```

Depending on the size of the SIRIUS output, this takes a few seconds up to a several minutes.


### Load the SIRIUS data

Then, the SIRIUS data needs to be loaded, i.e., the predicted fingerprints and other information are collected and stored in the `sirius-pred-fps.csv` file in the specified output folder.
```
itox load -i data/sample -o results/sample/sirius-pred-fps.csv
```

Run the models on your predicted fingerprints
```
itox run -m models/2024-08-09_20-08-13 -i results/sample/sirius-pred-fps.csv -o results/sample

```



View the results

TODO how





## D. More information

All the commands can also be run with `mlinvitrotox` instead `itox`. 














