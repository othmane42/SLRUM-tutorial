# SLRUM-tutorial
This small tutorial helps users work with clusters using SLURM and is specifically adapted for CECI cluster users.



# SSH setting
## Linux/WSL/MacOS
download the ssh key then execute the following commands : 
```
mkdir -p ~/.ssh && mv ~/Downloads/id_rsa.ceci ~/.ssh

chmod 600 ~/.ssh/id_rsa.ceci

dos2unix ~/.ssh/id_rsa.ceci

ssh-keygen -yf ~/.ssh/id_rsa.ceci > ~/.ssh/id_rsa.ceci.pub
```

create config file : 

- use the following link to generate the config file https://www.ceci-hpc.be/sshconfig.html
- copy the generated config using : 
    ```
    nano ~/.ssh/config
    ```
- make sure the file has correct permissions : 
```
chmod 600 ~/.ssh/config
```
restart the terminal and try to connect to the cluster :
```
ssh cecicluster
```



## Windows

Todo


# Modules

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

Example usage:


# Tmux
Tmux is a terminal multiplexer; it allows you to create several "pseudo terminals" from a single terminal. it comes in handy when you want to run a long job and you want to keep it running in background even when you exit the terminal.
to create a new tmux session, run the following command : 
```
tmux new -s session_name
```
to list all tmux sessions, run the following command : 
```
tmux ls
```
to attach to a tmux session, run the following command : 
```
tmux attach -t session_name
```

# Job submission

# Job monitoring

to check the status of  your job, run the following command :
```
squeue --user=username
```
you can also check the status in details of your job(s) by running the following command :
```
sacct --user=username
```


# Available Space

for Lucia cluster, you can check the available space on the cluster by running the following command : 
```
mmlsquota -g PROJECTNAME --block-size g ess
```

# Other useful commands
In order to run notebook on the cluster using a specific python virtual environment, you need to follow the following steps : 

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

If you want to send a file to the cluster, use this command:
```
scp  /path/to/local/file username@lucia:/path/to/remote/directory
```

# Other useful links 

to set VPN UMONS , follow this tutorial : https://alumniumonsac.sharepoint.com/sites/DirectiondesServicesInformatiques/SitePages/Connexion-VPN.aspx?web=1
