Creating and submitting a job
=============================
The typical way of creating a job is to write a submission script. A submission script is a shell script, e.g. a Bash script, whose comments ``#``, if they are prefixed with ``SBATCH``, are understood by Slurm as parameters describing resource requests and other submissions options. You can get the complete list of parameters from the sbatch manpage ``man sbatch``.

The first line of a submission script must be the shebang, e.g.:

``#!/bin/bash``

which must be followed by the ``#SBATCH`` directives.

A possible heading of a submission script, which might be called ``run_ppl.cmd``, can be::

 #!/bin/bash
 #SBATCH --job-name=my_job_1
 #SBATCH --time=00:10:00
 #SBATCH --ntasks=1
 #SBATCH --mem-per-cpu=100
 #SBATCH --error=job.%j.err
 #SBATCH --output=job.%j.out

In this submission script a job called ``my_job_1`` requests 1 CPU (task) for 10 minutes of time, with 100 MB of RAM memory per CPU, in the default partition (since the partition is not specified). It is possible to specify a particular partition with the  ``--partition`` option. Possible error messages due to the job and the output produced by the job will be saved in the ``job.<JOBID>.err`` and ``job.<JOBID>.out`` files. Instead of requiring the amount of RAM memory per CPU with the ``--mem-per-cpu`` option, it is possible to request the amount of RAM memory per node with the ``--mem`` option.

After the heading, the submission script ``run_ppl.cmd`` might continue as::

 cd /path/to/my_working_directory
 
 srun ./my_executable
 srun hostname
 srun sleep 60
 
 exit 0
 
that is, a working directory, ``/path/to/my_working_directory``, is set and the job will run the first job step ``srun ./my_executable``, namely it will execute the program executable ``my_executable`` in the working directory (``./``) on the node where the resource requested by the heading part of the job script is allocated. Then, a second job step, ``srun sleep 60``, will execute the ``sleep 60`` command on the requested resource. The command ``srun`` in front of the ``./my_executable``, the ``hostname``, and the ``sleep 60`` commands is optional.

After writing the submission script, it has to be submitted to Slurm through the ``sbatch`` command::

 $ sbatch run_ppl.cmd
   sbatch: Submitted batch job 83
   
The job then enters the queue in the PENDING state. Once resources become available and the job has highest priority, an allocation is created for it and it goes to the RUNNING state. If the job completes correctly, it goes to the COMPLETED state, otherwise, it is set to the FAILED state.

To obtain near-realtime information about the running job (e.g. memory consumption) you can execute the ``sstat`` command:

``$ sstat -j <JOBID>``

Specifically, you can select which information you want the command ``sstat`` to show with the ``--format`` option (see the manpage ``man sstat`` for more information on this command).