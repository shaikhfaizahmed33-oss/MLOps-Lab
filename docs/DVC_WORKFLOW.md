 
# DVC Workflow

## Remote Configuration

DVC was initialized in the project and a local DVC remote was configured:

    dvc remote add -d myremote ~/dvc-remote-storage

The DVC remote stores dataset objects separately from the Git repository.

## DVC Data Versioning Workflow

For every dataset change, the following workflow was followed:

    dvc add data/raw/iris_v1.csv
    git add data/raw/iris_v1.csv.dvc
    git commit -m "data: describe dataset change"
    dvc push

DVC tracks the actual dataset using content hashes, while Git tracks the `.dvc` metafile.

## Dataset Versions

Version 1 contained 150 rows.

Version 2 was created by augmenting the dataset with 20 synthetic rows, resulting in 170 rows.

The two versions have different DVC hashes:

- Version 1: `21d441a28bce4417276097df955afc50`
- Version 2: `674c8c36bb7c4ba8d851dee9e6ee67af`

## Comparing Dataset Versions

The command:

    dvc diff c54ddcd

was used to compare the current dataset with the Version 1 commit.

It reported that:

    data/raw/iris_v1.csv

was modified.

## Restoring Dataset Versions

To reproduce Version 1, the historical `.dvc` metafile was restored:

    git checkout c54ddcd -- data/raw/iris_v1.csv.dvc

Then the corresponding dataset was restored using:

    dvc checkout data/raw/iris_v1.csv.dvc

This restored 150 rows (151 lines including the header).

The latest `.dvc` metafile was then restored:

    git checkout HEAD -- data/raw/iris_v1.csv.dvc

and the latest dataset was restored using:

    dvc checkout data/raw/iris_v1.csv.dvc

This restored 170 rows (171 lines including the header).

## Code and Data Reproducibility

Git versions the source code and DVC metafiles, while DVC versions the actual datasets. Together, Git and DVC allow historical code and dataset states to be reproduced for experiments.
