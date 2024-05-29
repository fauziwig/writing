# WRITING 

# Daftar Isi
1. [Setup and Use Git](#setup-and-use-git)
2. [Creating an Anaconda Virtual Environment](#creating-an-anaconda-virtual-environment)

## Pendahuluan
Bagian ini menjelaskan tentang pendahuluan dari dokumen ini.

## Latar Belakang
Bagian ini berisi latar belakang yang relevan dengan topik.


## SETUP AND USE GIT 
SETUP
Configuring user information used across all local repositories : 

1. set a name that is identifiable for credit when review version history

        git config --global user.name “[firstname lastname]”

2. set an email address that will be associated with each history marker

        git config --global user.email “[valid-email]”

SETUP & INIT
Configuring user information, initializing and cloning repositories : 

3. initialize an existing directory as a Git repository

        git init

retrieve an entire repository from a hosted location via URL **(OPTIONAL)**

        git clone [url]

STAGE & SNAPSHOT
Working with snapshots and the Git staging area :

4. show modified files in working directory, staged for your next commit

        git status

5. add a file as it looks now to your next commit (stage)

        git add [file]

unstage a file while retaining the changes in working directory

        git reset [file]

diff of what is changed but not staged

        git diff

diff of what is staged but not yet commited

        git diff --staged

6. commit your staged content as a new commit snapshot

        git commit -m “[descriptive message]”

SHARE & UPDATE
Retrieving updates from another repository and updating local repos : 

7. add a git URL as an alias

        git remote add [alias] [url]

fetch down all the branches from that Git remote

        git fetch [alias]

merge a remote branch into your current branch to bring it up to date

        git merge [alias]/[branch]

8. Transmit local branch commits to the remote repository branch

        git push [alias] [branch]

fetch and merge any commits from the tracking remote branch

        git pull

Thanks to, 
Original source : https://education.github.com/git-cheat-sheet-education.pdf

<hr>

## CREATING AN ANACONDA VIRTUAL ENVIRONMENT
- Before use virtual environment, we should download and install Anaconda in https://www.anaconda.com/download 

Here’s an example of how to create a new Conda environment using the conda create command:

1. Open your terminal or command prompt (after you installed success Anaconda, recommended use : Anaconda Prompt)
2. Enter the following command to create a new Conda environment named `myenv`:

           module load anaconda
        conda create --name myenv

You can also specify which version of Python you want to use by including the version number after the environment name. For example, to create a new environment named myenv with Python 3.9, you would enter:

        conda create --name myenv python=3.9
        
3. Activate the new environment

        conda activate myenv

4.  Once the environment is activated, you can install any Python packages you need using conda or pip. For example, to install the numpy package using conda, you would run:

        conda install numpy

Alternatively, you can use pip to install packages

        pip install numpy

When you’re done working in the environment, you can deactivate it by running the following command:

        conda deactivate

That’s it! You’ve now created a new Conda environment and installed a package inside it.

## HOW TO USE PYTHON VEND AND CONDA ENV IN SLURM SCRIPT
To use a virtual environment created with either venv or conda in a Slurm script, you need to activate the environment before running your Python script.

Here’s how to do that:

Using a virtual environment created with venv:

      #!/bin/bash
      #SBATCH --job-name=myjob
      #SBATCH --output=myjob.out
      #SBATCH --error=myjob.err
      #SBATCH --ntasks=1
      #SBATCH --cpus-per-task=1
      #SBATCH --time=1:00:00
      #SBATCH --partition=your_partition

## Load any necessary modules or dependencies
      module load some_module

## Activate the virtual environment
      source /path/to/venv/bin/activate

## Run your Python script
      python myscript.py

## Deactivate the virtual environment
      deactivate

Replace /path/to/venv with the path to your virtual environment directory, and myscript.py with the name of your Python script.

Additionally, make sure to adjust the module load commands for any other modules or dependencies your Python script requires.

# Adding Note from me :
## Get a list of all conda virtual environments
        conda env list


Thanks to, 

Original Source : https://www.arch.jhu.edu/python-virtual-environments/ 
