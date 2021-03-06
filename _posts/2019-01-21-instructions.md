---
layout: post
title:  "Getting started at ETH Tier3"
---

This page is intended to help semester/master students in the first steps of their work with ETH. Send comments to micheli@cern.ch

## 1. Get an account at Tier3

The starting point is to **get an account at the T3 @ PSI**. You can follow the instructions [here](https://wiki.chipp.ch/twiki/bin/view/CmsTier3/HowToGetAccount). In general, you can specify that you don't
need a GRID account. Ask your supervisor what to put in "physics group". The suggested login shell is /bin/bash.

Basic instructions on how to login and perform basic operations can be found [here](https://wiki.chipp.ch/twiki/bin/view/CmsTier3/HowToSetupYourAccount). **Please note that for ETH the user interface host name is t3ui02.psi.ch**. 

## 2. Preliminary steps

The first step is to setup the environement for running **ROOT** and **jupyter**. [This page](https://wiki.chipp.ch/twiki/bin/view/CmsTier3/HowToWorkInCmsEnv#The_ROOT_Environment_and_Jupyter) collects the needed informations. 
After that try to start a jupyter notebook on a t3 machine:

```python
jupyter notebook --port 8883 --no-browser 
```

where 8883 has to be changed with the number of the PORT you used for setup.

Open a local browser on your laptop and go to <http://localhost:8883/tree>

## 3. Setting up a web-page

We will need **a lot of plots**. A convenient way is to look at them is to create a web-page for them.

**Instructions**:

0- Access your directory on people.phys.etz.ch following <https://readme.phys.ethz.ch/storage/access/>

1- Cut an paste the lines in  "Creating the homepage - Short description for Linux and Mac command line users” from :  <https://readme.phys.ethz.ch/web/how_to_get_a_personal_homepage/>

2- create in public_html a directory called plots.

3- cd plots

4- git clone https://github.com/musella/php-plots.git 

5- Follow the instructios here <https://github.com/musella/php-plots/blob/master/README.md>

6- It is suggested to get [this php file](https://github.com/michelif/ETH-Students-instuctions/blob/master/index_for_students.php) and overwrite the one you got from git. This php allows a better management for plot display

7- change mdonega —>you_username in index.php
   (it’s the line    $script_path = "/~mdonega/plots”;   )
   
8- send a mail to isg@phys.ethz.ch and ask them to enable php in your directory adding your supervisor in cc (say it’s the same configuration they made for mdonega) 

9- if everything works you should get something like this: 
     people.phys.ethz.ch/~mdonega/plots/uglyPlots_v9/
     
## 4. How to use GPUs on Tier3

1. Get a Tier3 computing account and make sure that you can log in to the /t3home (use pwd to check if you are in the correct home directory). In case you dont have an account yet or only a work directory follow the steps above and/or contact Tier3 admins. The t3home space is limited to 10GB, for bigger files use the /work directory or storage element.

2. Login to the GPU login interface with:

```bash
ssh t3ui02
```

3. Set correct environment in order to use python and all libraries needed:

```bash
export PATH=/work/creissel/anaconda3/bin:$PATH
```

which loads 4 environments which can be shown by using:

```bash
conda env list
```
For general purposes, only the two tensorflow environments are relevant:
- tensorflow_cpu (contains all basic packages for machine learning on a CPU)
- tensorflow_gpu (contains all basic packages for machine learning on a GPU) 

Choose the correct environment. It is important to use the tensorflow_gpu environment, when the jobs are submitted to the gpus. Using tensorflow_cpu will not raise any error, but jobs will run on the CPUs and potentially block computing resources for other users!

```bash
source activate tensorflow_gpu
```

4. In order to run a training, you can do so by submitting a script:

```bash
sbatch submit_gpu.sh
```

submit_gpu.sh has to be in the following form:

```bash
#!/bin/bash

#SBATCH --job-name=example                   

#SBATCH --account=gpu_gres  # to access gpu resources

#SBATCH --partition=gpu

#SBATCH --nodes=1       # request to run job on single node                                       

#SBATCH --ntasks=5     # request 10 CPU's (t3gpu01: balance between CPU and GPU : 5CPU/1GPU)      

#SBATCH --gres=gpu:1    # request 1 GPU's on machine                                         

python example.py

```
In order to run a training on a single gpu, you dont have to modify the script at all, except of changing the last line in order to run your personal code.

Submitting / running submit_gpu.sh will give you a file called slurm-XXX.out containing the terminal output of your job as well as possible errors. Input and the output such as pickle files saving the trained model can only be stored in the /t3home directory or the /work directory by now.

5. The status of the jobs can be checked with 

```bash
squeue -all
```
The current activity on the GPUs can be monitored by running:
```bash
srun --partition gpu /usr/bin/nvidia-smi
```

6. Before you start, please download all files in [this folder](https://github.com/michelif/ETH-Students-instructions/tree/master/example_gpu). Try to run a training with keras and tensorflow as described above using the example code. Make sure that the output in slurm-XXX.out says
```bash
-------------------------------------------
GPU available:  True
Keras backend:  tensorflow
-------------------------------------------
```
While running more heavy trainings, please always check the GPU workload with 
```bash
srun --partition gpu /usr/bin/nvidia-smi
```
in order to make sure that you are really using the GPUs!

6. In case you have questions regarding the GPUs, write an email to christina.reissel@cern.ch or contact any of the T3admins.

