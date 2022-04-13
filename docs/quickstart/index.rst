*******************
Userguide for Pleiadi cluster
*******************

.. contents:: Table of Contents

Note: along the entire document the dollar $ sign indicates the shell prompt.

Quick start – First steps 
====================

Creating an account
-------------------

Connecting to the cluster
-------------------

To connect to the Pleiadi cluster frontend as a user with a certain username, execute:

``$ ssh <username>@pleiadi.oact.inaf.it``

For example, for the user “pippo” the login command is:

``$ ssh pippo@pleiadi.oact.inaf.it``

After executing the ``ssh`` command, the login can be completed inserting a password or without the need of inserting a password, if a public key created on your laptop is provided to the cluster.

#. **With the password**: insert the password provided by the cluster administrator when you user was created on the cluster 
#. **With key exchange**:

   #. Create on your laptop a public/private couple of keys, with the command ``$ ssh-keygen``, confirming the default values during the procedure. With this command, a public (``id_rsa.pub``) and a private (``id_rsa``) key are created in the ``.ssh`` directory of your laptop.
   #. Login to Pleiadi inserting the password provided by the administrator.
   #. Append the content of the ``id_rsa.pub`` key created on your laptop in the ``/home/pippo/.ssh/authorized_keys`` file.
   #. From the next login, you will be able to execute ``$ ssh pippo@pleiadi.oact.inaf.it`` without password insertion.
   
Copying files
-------------------

#. **With the** ``scp`` **command**: the ``scp`` command works as the ``cp`` command except for the fact that it works across the network to copy files from one computer to another.
   #. To copy a file from your laptop to Pleiadi: ``$ scp /path-on-your-laptop/my_file.txt pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/``
   #. To copy a file from Pleiadi to your laptop: ``$ scp pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/my_file.txt /path-on-your-laptop/``
   #. To copy a directory from your laptop to Pleiadi: ``$ scp -r /path-on-your-laptop/my_dir pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/``
   #. To copy a directory from Pleiadi to your laptop: ``$ scp -r pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/my_dir /path-on-your-laptop/``
#. **With the** ``rsync`` **command**: As syntax, ``rsync`` works as the ``scp`` command. The main difference is that, differently from ``scp``, when copying the content of one directory in another directory, it copies only the files that are different from the two directories, which saves time. Moreover, with respect to the ``scp`` protocol, ``rsync`` guarantees (1) more security (it allows encryption of data using ``ssh`` protocol during transfer), (2) less bandwidth (it employs compression and decompression of data blocks during the transfers), and (3) the absence of special privileges to install and execute it. The correspondent commands listed above with ``rsync`` are:

   #. ``$ rsync /path-on-your-laptop/my_file.txt pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/``
   #. ``$ rsync pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/my_file.txt /path-on-your-laptop/``
   #. ``$ rsync -r /path-on-your-laptop/my_dir pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/``
   #. ``$ rsync  -r pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/my_dir /path-on-your-laptop/``

Editing files
-------------------

To edit a file, the most convenient way is to use a terminal-based editor. Some example are Vi, Vim, Emacs, Nano, mcedit, ne, slap, micro, pico, Joe or mped. Vi, Vim, and nano are preinstalled on Pleiadi.

SLURM quick start tutorial
-------------------

As workload manager to schedule jobs, Pleiadi employs SLURM. SLURM schedules jobs on a certain amount of allocated resources (number of nodes, number of CPUs per node, time limit, memory amount, etc.) according to the specifications of the user.

Gathering information
^^^^^^^^^^^^^^^^^^^^^^

To obtain information about SLURM on your cluster the ``sinfo`` and the ``squeue`` commands are quite useful.

The ``sinfo`` command provides information about:

#. **PARTITION**: The partitions available on the cluster, where a partition is a set of compute nodes logically grouped and dedicated to different tasks (e.g. batch processing, debugging, post processing, or visualization). The default partition is marked with an asterisk;
#. **AVAIL**: The state of the partitions;
#. **TIMELIMIT**: The maximum time limit for a job launched by any user in days-hours:minutes:seconds. If the parameter is “infinite” it means no time limit is set for that partition;
#. **NODES**: The number of nodes with a particular configuration in each partition;
#. **STATE**: The state of each group of nodes;
#. **NODELIST**: The list of nodes in each group.

Example of output of the sinfo command on Pleiadi::

    $ sinfo
      PARTITION  AVAIL  TIMELIMIT  NODES   STATE  NODELIST
          debug     up   infinite      5   down*  r35c1s11,r35c3s[11-12],r35c5s10,r35c6s11
          debug     up   infinite      2   alloc  r35c1s[01-02]
          debug     up   infinite     65    idle  r35c1s[03-10,12],r35c2s[01-12],r35c3s[01-10],r35c4s[01-12],r35c5s[01-09,11-12],r35c6s[01-10,12]
           gpu*     up   infinite      6    idle  r33c2s[01-06]
           
If ``sinfo`` is launched with the ``-N`` option, it shows the output in a node-oriented fashion.
Example of output of the ``sinfo -N`` command on Pleiadi::






 



