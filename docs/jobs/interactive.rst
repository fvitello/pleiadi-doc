Interactive jobs
================

Slurm jobs are normally batch jobs in the sense that they are run unattended. If you want to have a direct view on your job, for tests or debugging, you have two options.
If you need simply to have an interactive Bash session on a compute node, with the same environment set as the batch jobs, run the following command:

``$ srun --pty bash``

Doing that, you are submitting a 1-CPU, default memory, default duration job that will return a Bash prompt when it starts.

If you need more flexibility, you will need to use the `salloc <https://slurm.schedmd.com/salloc.html>`_ command. The ``salloc`` command accepts the same parameters as ``sbatch`` as far as resource requirement is concerned. Once the allocation is granted, you can use ``srun`` in the same way you would do in a submission script.

