# MLinvitroTox Tutorial

How to get started with the MLinvitrotox package

## A. Project description



## B. Getting started

TODO the SIRIUS data and the models still need to be added

In its current form, MLinvitroTox can only be used from a terminal as a command line interface (CLI). Also, we recommend to use git to download the repository.

TODO provide a link on how to get started with a terminal

Open a terminal and move to the folder in which you would like to work.

```
cd move/to/work/folder
```

Use git to clone the repository
```
git clone git@gitlab.renkulab.io:expectmine/mlinvitrotox-tutorial.git
cd mlinvitrotox-tutorial
```

Create a conda environment. We suggest to use mamba, an optimized version of conda.

If you need to install mamba (and conda), see [here](https://mamba.readthedocs.io/en/latest/installation/mamba-installation.html)
```
mamba env create -f ./environment.yml
```

Activate the environment
```
conda activate mlinvitrotox-tutorial
```

Install the mlinvitrotox package

TODO This step can be skipped, when mlinvitrotox is on pypi (it needs to be added to environment.yml)
```
pip install -i https://test.pypi.org/simple/ mlinvitrotox
```

## C. Example / usage

After we have setup the repository and the environment, we get started with adding the output from SIRIUS. Make sure to create the SIRIUS output with the summaries.

KASIA Could you check whether this description with the summaries make sense?

The SIRIUS data can be provided in two ways. 

1. as a folder. Move or copy the folder of SIRIUS output to the `data` folder. You then run the following command to extract it. It is being extract in the same folder. For this tutorial, we call the SIRIUS folder `sample`.

```
itox extract -i data/sample
```

2. as a zip folder. We recommend to keep the zip filder outside of the repository's directory structure. To extract, run the following command. The data will then be unzipped and extract to the `data/sample` folder.

```
itox extract -i path/to/zipfile/sample.zip -o data/
```

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














