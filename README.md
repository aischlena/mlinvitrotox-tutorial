# MLinvitroTox Tutorial

How to get started with the MLinvitrotox package


## A. Project description

## B. Getting started

TODO the SIRIUS data and the models still need to be added

In its current form, MLinvitroTox can only be used from a terminal as a command line interface.

Open a terminal and move to the folder in which you would like to work.

```
cd move/to/work/folder
```

Clone the repository
```
git clone git@gitlab.renkulab.io:expectmine/mlinvitrotox-tutorial.git
cd mlinvitrotox-tutorial
```

Create a conda environment. We suggest to use mamba, an optimized version of conda.

TODO or another virtual env?

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


The SIRIUS data is usually provided as a zip folder. Extract the zip folder and specify the data folder as the output folder. Here, we call the zip file sample.zip
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
















## C. Example / usage
