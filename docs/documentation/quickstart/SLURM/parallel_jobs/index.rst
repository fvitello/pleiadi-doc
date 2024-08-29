Going parallel
==============




Parallel job with MPI or other multi-process paradigms
""""""""""""""""""""""""""""""""""""""""""""""""""""""

A job parallelized with MPI is a multi-process program. Since in the Slurm context, a task is identified with a process, a multi-process program is made of several tasks. The tasks are requested with the ``-–ntasks`` option, that we have already seen in the `Creating and submitting a job`_ section. The tasks can be in a single node or spread across more nodes. The number of nodes is specified with the ``--nodes`` option and the number of tasks per node is specified with the ``--ntasks-per-node`` option. If the cores in each node of the cluster are distributed in more sockets, also the ``--ntasks-per-socket`` option, that specifies the number of tasks per socket in a node, can be set. The Pleiadi cluster is a dual socket platform, namely it has two sockets per node, and for optimizing job performances, it is recommended to set this option to the half of the value set for the ``--ntasks-per-node`` option. In this way, the tasks in each node are equally distributed between the two sockets. If the options ``--nodes`` and ``--ntasks-per-node`` are set, the option ``-–ntasks`` is automatically determined as ``-–ntasks = --nodes x --ntasks-per-node`` and it can be omitted in the Slurm submission script.

We show below an example of submission script for a job parallelized with MPI::

 #!/bin/bash
 #SBATCH --job-name=my_job_2   
 #SBATCH --time=10:00:00
 #SBATCH --nodes=2
 #SBATCH --ntasks-per-node=36
 #SBATCH --ntasks-per-socket=18
 #SBATCH --mem=125000
 #SBATCH --error=job.%j.err
 #SBATCH --output=job.%j.out
 #SBATCH --account=my_account
 #SBATCH --partition=debug

 cd /path/to/my_working_directory
 module load gcc-11.2.0 openmpi-4.1.2/gcc-11.2.0
 
 mpirun -np 72 ./my_MPI_executable
 
 exit 0

In this submission script, a job called ``my_job_2`` requests 2 nodes with 36 CPUs per node, and in each node the 36 CPUs are equally distributed between the two sockets (18 CPUs per socket). It also requests 125 GB of RAM per node, and it will run on the debug partition. The error and output messages of the job will be saved in the ``job.<JOBID>.err`` and ``job.<JOBID>.out`` files. Since Pleiadi has 36 cores per node and it is a dual-socket platform, the ``--ntasks-per-node`` and ``--ntasks-per-socket`` options are set to the maximum allowed values. This job will run on a total number of tasks equal to ``-–ntasks = --nodes x --ntasks-per-node = 72``. A new option, ``--account``, is added to the submission script, which means that the computational hours for the job execution will be taken from the ``my_account`` account.
Once the job is submitted, the command ``mpirun -np 72`` will create 72 instances of the executable ``my_MPI_executable`` on the requested resources allocated by Slurm. As in the example in the `Creating and submitting a job`_ section, the working directory is set to ``/path/to/my_working_directory``. The ``module load gcc-11.2.0 openmpi-4.1.2/gcc-11.2.0`` command is used to load in sequence the ``gcc-11.2.0`` and ``openmpi-4.1.2/gcc-11.2.0`` modules. The modules istruct the shell to modify a user’s environment: in this way a user can customize its own environment according to her/his specific needs. In this example, we load the ``gcc-11.2.0`` and the ``openmpi-4.1.2/gcc-11.2.0`` modules, to use (Open MPI) 4.1.2 to compile and run the application ``my_MPI.cpp``, which can be compiled prior to the submission of the Slurm script, with:

``$ mpicc my_MPI.c -o my_MPI_executable``

Loading this module we have the following compiler and MPI versions::

 $ mpicc --version
 gcc (GCC) 11.2.0
 Copyright (C) 2021 Free Software Foundation, Inc.
 This is free software; see the source for copying conditions.  There is NO
 warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

 $ mpirun --version
 mpirun (Open MPI) 4.1.2

 Report bugs to http://www.open-mpi.org/community/help/
 
Other possible modules to use to compile and execute a MPI program are those of the ``/opt/Modules/compilers/intel`` group. For more information about modules consult the website `Modules <https://modules.readthedocs.io/en/latest/>`_, and to see all the modules available on Pleiadi cluster see the section `Available modules`_.


Parallel job with a shared memory paradigm (e.g. OpenMP)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""

A job parallelized with a shared memory paradigm (e.g. OpenMP) is a multithreaded program. A multithreaded program is made of only one task that employs more CPUs. These CPUs are requested/created with the ``--cpus-per-task`` option. Whereas with the ``--ntasks`` option, the tasks can be scheduled among several nodes, as specified in the Slurm submission script, with the ``--cpus-per-task`` option, the CPUs are assigned to a single task in one node.

We show below an example of submission script for a job parallelized with OpenMP::

 #!/bin/bash
 #SBATCH --job-name=my_job_3   
 #SBATCH --time=10:00:00
 #SBATCH --ntasks=1
 #SBATCH --cpus-per-task=4
 #SBATCH --mem=125000
 #SBATCH --error=job.%j.err
 #SBATCH --output=job.%j.out
 #SBATCH --account=my_account
 #SBATCH --partition=debug
 
 cd /path/to/my_working_directory
 module load gcc-11.2.0
 
 export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
 ./my_OpenMP_executable
 
 exit 0
 
In this submission script, a job called ``my_job_3`` will run on 4 CPUs on the same task, reserved in a single compute node. The number of OpenMP threads on which the job will run can be set with the ``OMP_NUM_THREADS`` environment variable, that in this case is set to the value given to the ``--cpus-per-task`` option. The value assigned to the ``--cpus-per-task`` option is extracted with the ``$SLURM_CPUS_PER_TASK`` command. 
In this example, we load the module ``gcc-11.2.0``, where the version of the compiler is::

 $ gcc --version
 gcc (GCC) 11.2.0
 Copyright (C) 2021 Free Software Foundation, Inc.
 This is free software; see the source for copying conditions.  There is NO
 warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

and the program that uses OpenMP launched by this job can be compiled with
 
``$ gcc -fopenmp my_OpenMP.c -o my_OpenMP_executable``
The ``-fopenmp`` option might be omitted.


Hybrid MPI+OpenMP parallel job
"""""""""""""""""""""""""""""""

Some programs exploit both multi-process and shared memory paradigms. These kind of programs can be executed, for example, with a submission script like this::

 #!/bin/bash
 #SBATCH --job-name=my_job_4   
 #SBATCH --time=10:00:00
 #SBATCH --nodes=2
 #SBATCH --ntasks-per-node=18
 #SBATCH --ntasks-per-socket=9
 #SBATCH --cpus-per-task=2
 #SBATCH --mem=125000
 #SBATCH --error=job.%j.err
 #SBATCH --output=job.%j.out
 #SBATCH --account=my_account
 #SBATCH --partition=debug
 
 cd /path/to/my_working_directory
 module load gcc-11.2.0 openmpi-4.1.2/gcc-11.2.0
 
 export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
 mpirun -np 36 ./my_MPI_OpenMP_executable
 
 exit 0

In this case, Slurm will allocate ``--ntasks = --nodes x --ntasks-per-node = 2 x 18 = 36`` tasks on 2 nodes, and 2 CPUs for each task. In this way, the computation assigned to each MPI process will be further parallelized on 2 OpenMP threads.


GPU parallel job
""""""""""""""""""

On Pleiadi cluster, 6 out of 72 nodes have one GPU. If you want to run a job parallelized on a GPU environment (e.g. with OpenACC or CUDA) you have to specify the GRES (Generic Resource Scheduling) option in the submission script:

``#SBATCH --gres=gpu:1``

It is important to note that the nodes having a GPU belong only to particular partitions. On the cluster Pleiadi the ``sinfo`` command, already seen in the `Gathering information`_ section, provides as output::

 $ sinfo
   PARTITION  AVAIL  TIMELIMIT  NODES   STATE  NODELIST
       ...
       gpu       up   infinite      6    idle  r33c2s[01-06]
       v100      up   infinite      2    idle  r33c2s[01-02]
       ...
        
which indicates that the 6 nodes that have a GPU belong to the ``gpu`` partition and that, in particular, the two nodes that have a GPU of the V100 type further belong to the v100 partition. Therefore, in the submission script the

``#SBATCH --partition=gpu``

or the 

``#SBATCH --partition=v100``

option has to be set.

An example of submission script for a job running on one GPU node is::

 #!/bin/bash
 #SBATCH --job-name=my_job_4   
 #SBATCH --time=10:00:00
 #SBATCH --ntasks=1
 #SBATCH --gres=gpu:1
 #SBATCH --mem=125000
 #SBATCH --error=job.%j.err
 #SBATCH --output=job.%j.out
 #SBATCH --account=my_account
 #SBATCH --partition=gpu
 
 cd /path/to/my_working_directory
 module load nvhpc_2022_221/gcc-11.2.0
 
 srun ./my_MPI_GPU_executable
 
 exit 0

In this example the task allocated on the requested node will also run on the GPU of the node. For this job, we loaded the ``nvhpc_2022_221/gcc-11.2.0`` module, that contains NVIDIA compilers, suitable for the compilation of GPU-based applications, and the ``gcc`` compiler of the ``11.2.0`` version. The ``nvhpc_2022_221/gcc-9.4.0`` and ``nvhpc_2022_221/gcc-10.3.0`` are also available, which contain the ``gcc`` compiler with the ``9.4.0`` and ``10.3.0`` versions, respectively.
