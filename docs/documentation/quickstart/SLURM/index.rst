SLURM quick start tutorial
==========================

As workload manager to schedule jobs, Pleiadi employs SLURM. SLURM schedules jobs on a certain amount of allocated resources (number of nodes, number of CPUs per node, time limit, memory amount, etc.) according to the specifications of the user.

Compute nodes main features
^^^^^^^^^^^^^^^^^^^^^^

The below table summarizes the main features of the compute nodes of the Pleiadi cluster, which are useful to set the Slurm options in the submission scripts or for interactive jobs submissions: 

+-----------------------------+----------------+
| Number of CPUs              | 36             |
+-----------------------------+----------------+
| Number of sockets           | 2              |
+-----------------------------+----------------+
| Number of cores per socket  | 18             |
+-----------------------------+----------------+
| Number of threads per core  | 1              |
+-----------------------------+----------------+
| RAM memory                  | 128 or 256 GB  |
+-----------------------------+----------------+


Gathering information
^^^^^^^^^^^^^^^^^^^^^^

To obtain information about SLURM on your cluster the ``sinfo`` and the ``squeue`` commands are quite useful.

The ``sinfo`` command provides information about:

#. **PARTITION**: The partitions available on the cluster, where a partition is a set of compute nodes logically grouped and dedicated to different tasks (e.g. batch processing, debugging, post processing, or visualization). The default partition is marked with an asterisk. The partitions present on Pleiadi are ``128g``, ``256g``, ``gpu``, and ``v100`` where ``128g`` is the default partition;
#. **AVAIL**: The state of the partitions;
#. **TIMELIMIT**: The maximum time limit for a job launched by any user in days-hours:minutes:seconds. If the parameter is “infinite” it means no time limit is set for that partition;
#. **NODES**: The number of nodes with a particular configuration in each partition;
#. **STATE**: The state of each group of nodes;
#. **NODELIST**: The list of nodes in each group.

Example of output of the ``sinfo`` command on Pleiadi::

    $ sinfo
      PARTITION  AVAIL  TIMELIMIT  NODES   STATE  NODELIST
          debug     up   infinite      1   inval  r35c1s10
          debug     up   infinite      5   down*  r35c1s11,r35c3s[11-12],r35c5s10,r35c6s11
          debug     up   infinite      2     mix  r35c1s09,r35c6s12
          debug     up   infinite      8   alloc  r35c1s[01-08]
          debug     up   infinite     56    idle  r35c1s12,r35c2s[01-12],r35c3s[01-10],r35c4s[01-12],r35c5s[01-09,11-12],r35c6s[01-10]
          gpu       up   infinite      6    idle  r33c2s[01-06]
          v100      up   infinite      2    idle  r33c2s[01-02]
          256g*     up   infinite      1   inval  r35c1s10
          256g*     up   infinite      1   down*  r35c1s11
          256g*     up   infinite      1     mix  r35c1s09
          256g*     up   infinite      8   alloc  r35c1s[01-08]
          256g*     up   infinite      1    idle  r35c1s12
          
           
If ``sinfo`` is launched with the ``-N`` option, it shows the output in a node-oriented fashion.

Example of output of the ``sinfo -N`` command on Pleiadi::

     $ sinfo -N
       r33c2s01    1     gpu* idle 
       r33c2s02    1     gpu* idle 
       r33c2s03    1     gpu* idle 
       r33c2s04    1     gpu* idle 
       r33c2s05    1     gpu* idle 
       r33c2s06    1     gpu* idle 
       r35c1s01    1     debug alloc
       r35c1s02    1     debug alloc
       r35c1s03    1     debug idle 
       r35c1s04    1     debug idle 
       r35c1s05    1     debug idle 
       r35c1s06    1     debug idle 
       r35c1s07    1     debug idle 
       r35c1s08    1     debug idle 
       r35c1s09    1     debug idle 
       r35c1s10    1     debug idle 
       r35c1s11    1     debug down*
       ...

When also the ``-l`` option is added, more information about the nodes is shown, such as the number of CPUs, the memory, the temporary disk space (also called scratch space), the node weight (an internal parameter specifying preferences in nodes for allocations when there are multiple possibilities), the features of the nodes (such as processor type for instance), and the reason, if applicable, for which a node is down.
 
Example of output of the ``sinfo -Nl`` command on Pleiadi::
 
     $ sinfo -Nl
       Wed Apr 13 16:13:16 2022
       NODELIST   NODES PARTITION       STATE CPUS    S:C:T MEMORY TMP_DISK WEIGHT AVAIL_FE REASON              
       r33c2s01       1     v100*        idle 36     2:18:1 128236        0      1   (null) none                
       r33c2s01       1       gpu        idle 36     2:18:1 128236        0      1   (null) none                
       r33c2s02       1       gpu        idle 36     2:18:1 128236        0      1   (null) none                
       r33c2s02       1     v100*        idle 36     2:18:1 128236        0      1   (null) none                
       r33c2s03       1       gpu        idle 36     2:18:1 128236        0      1   (null) none                
       r33c2s04       1       gpu        idle 36     2:18:1 128236        0      1   (null) none                
       r33c2s05       1       gpu        idle 36     2:18:1 128236        0      1   (null) none                
       r33c2s06       1       gpu        idle 36     2:18:1 128236        0      1   (null) none                
       r35c1s01       1     debug   allocated 36     2:18:1 128236        0      1   (null) none                
       r35c1s02       1     debug   allocated 36     2:18:1 128236        0      1   (null) none                
       r35c1s03       1     debug        idle 36     2:18:1 128236        0      1   (null) none                
       r35c1s04       1     debug        idle 36     2:18:1 128236        0      1   (null) none                
       r35c1s05       1     debug        idle 36     2:18:1 128236        0      1   (null) none                
       r35c1s06       1     debug        idle 36     2:18:1 128236        0      1   (null) none                
       r35c1s07       1     debug        idle 36     2:18:1 128236        0      1   (null) none                
       r35c1s08       1     debug        idle 36     2:18:1 128236        0      1   (null) none                
       r35c1s09       1     debug        idle 36     2:18:1 128236        0      1   (null) none                
       r35c1s10       1     debug        idle 36     2:18:1 128236        0      1   (null) none                
       r35c1s11       1     debug       down* 36     2:18:1 128236        0      1   (null) Not responding
       ...
       
The ``squeue`` command provides information about the jobs that are currently running (**ST** ``R``) or pending, namely waiting for resources (**ST** ``PD``). Specifically it provides information about:

#. **JOBID**: The ID of the job;
#. **PARTITION**: The partition to which the job is assigned;
#. **NAME**: The name of the job;
#. **USER**: The user that launched the job;
#. **ST**: The status of the job (it can be e.g. ``R`` or ``PD``)
#. **TIME**: It shows how long the job has been running;
#. **NODES**: The number of nodes on which the job is running;
#. **NODELIST(REASON)**: The nodes on which the job is running, for running jobs, or the reason why the job is pending, for pending jobs. Two possible reasons are ``(Resources)``, which means that the job has not started because the requested resources are not available in sufficient amount and ``(Priority)``, which means that this job will not run until another pending job with higher priority will run.

Example of output of the ``squeue`` command on Pleiadi::

    $ squeue
      JOBID PARTITION     NAME        USER  ST       TIME  NODES NODELIST(REASON)
         83     debug my_job_1       pippo   R    6:20:48      2 r35c1s[01-02]
         84       gpu my_job_2       pluto  PD    0.00         6 (Resources)
         85     debug my_job_3    topolino  PD    0.00        16 (Priority)

To interrupt the execution of a job we use the ``scancel`` command in this way:

``$ scancel <JOBID>``

(e.g. ``scancel 83``).


Creating and submitting a job
^^^^^^^^^^^^^^^^^^^^^^
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



Interactive jobs
^^^^^^^^^^^^^^^^^^^^^^

Slurm jobs are normally batch jobs in the sense that they are run unattended. If you want to have a direct view on your job, for tests or debugging, you have two options.
If you need simply to have an interactive Bash session on a compute node, with the same environment set as the batch jobs, run the following command:

``$ srun --pty bash``

Doing that, you are submitting a 1-CPU, default memory, default duration job that will return a Bash prompt when it starts.

If you need more flexibility, you will need to use the `salloc <https://slurm.schedmd.com/salloc.html>`_ command. The ``salloc`` command accepts the same parameters as ``sbatch`` as far as resource requirement is concerned. Once the allocation is granted, you can use ``srun`` in the same way you would do in a submission script.


Parallel jobs
^^^^^^^^^^^^^^
The example in the previous section illustrates a *serial* job which runs a single CPU on a single node, and that does not exploit the resources available in the multiple nodes in the cluster.

We illustrate below some examples of *parallel* jobs. 
`CLICK HERE <parallel_jobs/index.rst>`_.

