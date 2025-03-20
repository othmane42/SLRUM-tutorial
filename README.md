# SLRUM-tutorial
This small tutorial helps users work with clusters using SLURM and is specifically adapted for CECI cluster users.



# 1- SSH setting
## Linux/WSL/MacOS
Download the ssh key then execute the following commands : 
```
mkdir -p ~/.ssh && mv ~/Downloads/id_rsa.ceci ~/.ssh

chmod 600 ~/.ssh/id_rsa.ceci

dos2unix ~/.ssh/id_rsa.ceci

ssh-keygen -yf ~/.ssh/id_rsa.ceci > ~/.ssh/id_rsa.ceci.pub
```

Create config file : 

- Use the following link to generate the config file https://www.ceci-hpc.be/sshconfig.html
- Copy the generated config using : 
    ```
    nano ~/.ssh/config
    ```
- Make sure the file has correct permissions : 
```
chmod 600 ~/.ssh/config
```
Restart the terminal and try to connect to the cluster :
```
ssh cecicluster
```



## Windows

Todo


# 2- Modules

To load a python module, run the following command :
```
module load devel/python/3.9.13
```
You can check the available modules by running the following command :
```
module avail
```
Some of them come with tensorflow already installed for instance.

# Interactive session
If you want to run job interactively, you can use the following command : 
```
srun -p debug-gpu -A mmfusion -N 1 --gpus=1 --cpus-per-task=16 --mem=100G -t 2:00:00 --pty bash
```
| Parameter              | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| `-p` or `--partition`  | Specifies the partition (queue) to which the job should be submitted.       |
| `-A` or `--account`    | Specifies the project name to be charged for the job.                            |
| `-N` or `--nodes`      | Specifies the number of nodes to be allocated for the job.                  |
| `--gpus`               | Specifies the number of GPUs to be allocated for the job.                   |
| `--cpus-per-task`      | Specifies the number of CPUs to be allocated per task.                      |
| `--mem`                | Specifies the amount of memory to be allocated for the job.                 |
| `-t` or `--time`       | Specifies the maximum runtime for the job. for interactive session, it is allowed  to set max 2 hours wall time.                                  |
| `--pty`                | Allocates a pseudo-terminal for interactive use.                            |
| `--ntasks`             | Specifies the number of tasks to be run.                                    |
| `--ntasks-per-node`    | Specifies the number of tasks to be run per node.                           |



# 3- Tmux
Tmux is a terminal multiplexer; it allows you to create several "pseudo terminals" from a single terminal. it comes in handy when you want to run a long job and you want to keep it running in the background even when you exit the terminal.
To create a new tmux session, run the following command : 
```
tmux new -s session_name
```
To list all tmux sessions, run the following command : 
```
tmux ls
```
To attach to a tmux session, run the following command : 
```
tmux attach -t session_name
```

# 4- Job submission
For longer trainings, you must use job submission using `sbatch` like follows : 
```
sbatch job_submission_config.sh

```
For that you have to prepare a config file which is divided into 2 parts :

**Part 1 : computation ressources allocation**


```
#!/bin/bash
#SBATCH --job-name=gpu_job
#SBATCH --output=outputfilename.out
#SBATCH --partition=gpu  # mendatory to allocate gpu ressources
#SBATCH --nodes=1 # mostly enough for model trainings
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=16  # Set CPU allocation (number of cpu cores)
#SBATCH --ntasks=1
#SBATCH --mem=100G  # Adjust memory as needed
#SBATCH --gpus=1   # Adjust GPU count as needed
#SBATCH --time=6:30:00  # Adjust runtime as needed
#SBATCH --account=PROJECTNAME  # Your project allocation
``` 

**Part 2 : Environment Setup and Job Execution**

```

echo "----------------- Setting Python Environment ------------------"

# Load the needed module
module load devel/python/3.9.13

# Activate Python environment
source /gpfs/home/acad/umons-info/oamel/mm_frameworkEnv/bin/activate


echo "--------------- Running the Code ---------------"
echo -n "This run started on: "
date

srun python /path/to/script.py 

echo -n "This run completed on: "
date
```



# 3- Job monitoring

To check the status of  your job, run the following command :
```
squeue --user=username
```
Example of output :
```
      JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           7306729       gpu  gpu_job    oamel  R      26:39      1 cna020
```
You can also check the status in details of your job(s) by running the following command :
```
sacct --user=username
```
Example of output : 

```
JobID           JobName  Partition    Account  AllocCPUS      State ExitCode
------------ ---------- ---------- ---------- ---------- ---------- --------
7306729         gpu_job        gpu   mmfusion         16    RUNNING      0:0
7306729.bat+      batch              mmfusion         16    RUNNING      0:0
7306729.ext+     extern              mmfusion         16    RUNNING      0:0
7306729.0        hd-bet              mmfusion         16    RUNNING      0:0
```

To kill a job use `scancel` : 
```
scancel JobID 
```
# 4- Available Space

For Lucia cluster, you can check the available space on the cluster by running the following command : 
```
mmlsquota -g PROJECTNAME --block-size g ess
```

# 5- Other useful commands

## Jupyter kernel
- In order to run notebook on the cluster using a specific python virtual environment, you need to follow the following steps : 

-  Create a virtual environment if not done yet : 
```
python -m venv ENVNAME
```
-  Activate your virtualenv : 
```
source ENVNAME/bin/activate/
```
-  Run : 
```
pip install  ipykernel
```
-  Add it to Jupyter Kernels : 
```
python -m ipykernel install --user --name=ENVNAME
```


## Downloading/Uploading files

You can use `scp` to upload or download files between your local machine and cluster
- upload to cluster
```
scp file USERNAME@lucia:path/to/destination
```
- Download to local machine
```
scp USERNAME@lucia:path/to/file path/to/destination/in/localmachine 
```


# Other useful links 

To set VPN UMONS , follow this tutorial : https://alumniumonsac.sharepoint.com/sites/DirectiondesServicesInformatiques/SitePages/Connexion-VPN.aspx?web=1
