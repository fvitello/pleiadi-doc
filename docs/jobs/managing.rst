Managing jobs
=============

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