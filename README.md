# writing

CREATING AN ANACONDA VIRTUAL ENVIRONMENT
Here’s an example of how to create a new Conda environment using the conda create command:

Open your terminal or command prompt
Enter the following command to create a new Conda environment named myenv:
 
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

HOW TO USE PYTHON VEND AND CONDA ENV IN SLURM SCRIPT
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

# Load any necessary modules or dependencies
module load some_module

# Activate the virtual environment
source /path/to/venv/bin/activate

# Run your Python script
python myscript.py

# Deactivate the virtual environment
deactivate
Replace /path/to/venv with the path to your virtual environment directory, and myscript.py with the name of your Python script.

Using a virtual environment created with conda:

#!/bin/bash
#SBATCH --job-name=myjob
#SBATCH --output=myjob.out
#SBATCH --error=myjob.err
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --time=1:00:00
#SBATCH --partition=your_partition

# Load any necessary modules or dependencies
module load conda
module load some_module

# Activate the virtual environment
conda activate /path/to/env

# Run your Python script
python myscript.py

# Deactivate the virtual environment
conda deactivate
Replace /path/to/env with the path to your virtual environment directory, and myscript.py with the name of your Python script.

Additionally, make sure to adjust the module load commands for any other modules or dependencies your Python script requires.

original source : https://www.arch.jhu.edu/python-virtual-environments/ 
