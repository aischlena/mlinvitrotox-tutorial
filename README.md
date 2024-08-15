# MLinvitroTox Tutorial

How to get started with the MLinvitrotox package

## A. Project description



## B. Getting started

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

Create a conda environment. We recommend to use mamba, an optimized version of conda.

If you need to install mamba (and conda), see [here](https://mamba.readthedocs.io/en/latest/installation/mamba-installation.html). Should you be unfamiliar with conda environments, please check: TODO link

```
mamba env create -f ./environment.yml
```


If it was successful, you see an output that ends like this:

```
done
#
# To activate this environment, use
#
#     $ conda activate mlinvitrotox-tutorial
#
# To deactivate an active environment, use
#
#     $ conda deactivate

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

To show the basic functionality, there is a small sample dataset in the `data` folder, called `sampledata`.


### Extract the SIRIUS data

As the first step, you add the output from [SIRIUS](https://bio.informatik.uni-jena.de/software/sirius/). Make sure to create the SIRIUS output with the summaries.

KASIA Could you check whether this description with the summaries make sense?

The SIRIUS data can be provided in two ways:

1. as a folder. Move or copy your SIRIUS output folder to the `data` folder. 

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

