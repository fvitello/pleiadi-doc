Managing files
==============

**Note:** There is no backup of the data stored on the Pleiadi cluster. Any removed file is lost forever. It is the user’s responsibility to keep a copy of the contents of their data in a safe place.

The cluster areas
-------------------

The *home* directory
^^^^^^^^^^^^^^^^^^^^^^

Once logged in on the frontend node, the user will end up in its *home* directory, that has the following absolute path:

``/home/username``

To come back to the *home* directory from another directory, the user can execute the command:

``$ cd $HOME``

or simply

``$ cd``

The *home* directory absolute path can be shown with the command::

 $ echo $HOME
 /home/username

The *home* directory is available on the frontend node and on all the compute nodes of the cluster, since it is shared with the Network Filesystem protocol (``nfs``).

The *home* directory is dedicated to the data necessary to the launching of the task, to the data that have to be saved at the end of each task, to source codes (programs, scripts), to configuration files, and to small datasets (like input files). For each user all the files in this directory cannot exceed the quota of 20 GB (see also Section `Quota`_). 

**Note:** To your main working activity, e.g. with large datasets, do not use the *home* directory but the *work* directory (see Section `The work directory`_). 

The *home* directory is accessible only to the user owner of the *home* directory.

To copy files from your own host on your personal computer to your own *home* directory on the Pleiadi cluster it is possible to use the ``scp`` command (see Section `Copying files (basics)`_) in this way:

``$ scp -r /path/to/local/dir/ username@pleiadi.oact.inaf.it:/home/username``

Viceversa, to copy files from your own *home* directory on the Pleiadi cluster to your own host on your personal computer it is possible to use the ``scp`` command in this way:

``$ scp -r username@pleiadi.oact.inaf.it:/home/username /path/to/local/dir/``


The *work* directory
^^^^^^^^^^^^^^^^^^^^^^

For more memory-demanding computations, the user should not refer to the *home* directory but to the *work* directory. The *work* directory can be accessed with the following command:

``$ cd $WORK``

and it has the following absolute path::
 
 $ echo $WORK
 /mnt/beegfs/username

where ``/mnt/beegfs`` is the storage volume, defined with the high-performance, scalability, flexibility, and robustness BeeGFS parallel filesystem (`BeeGFS <https://www.beegfs.io/c/>`_). Each user will have a personal directory called ``<username>`` on the storage area ``/mnt/beegfs``, which cannot be accessed by other users. For each user all the files in this directory cannot exceed the quota defined in the user's request of the account (see also Section `Quota`_). As the *home* directory, the *work* directory is available both on the frontend and on all the compute nodes of Pleiadi cluster, since it is shared with ``nfs``.


Data archiving
^^^^^^^^^^^^^^^^^^^^^^

If the account is not reactivated within 3 MONTHS after the expiration indicated in the application form, two compressed archives containing all the files present in the *home* and in the *work* areas are created in the respective directories. After further 6 months from the creation of the compressed archives, i.e. after 9 months after the expiration of the account, all the archives are definitely removed from the storage and the account is deleted.


Quota
^^^^^^^^^^^^^^^^^^^^^^

The following storage quota are set for the different areas of the Pleiadi cluster:

#. *home*: each user can use up to 20 GB of disk space.
#. *work*: each user can use up to the quota defined in the request of the account.

The users can request an increase of the quota reserved for them sending us an e-mail at the address board.pleiadi@inaf.it, including the proper motivations for the request that will be successively evaluated by the local Staff. Possible quota extensions have to have an expiration, that must be as short as possible. Anyway, the period required for the quota extension cannot exceed the expiration date expected for the account.

For the accounts that will result in overquota to the monthly control following the expiration of the same, the data will be archived in advance as soon as the normal grace period granted (7 days) has passed. If at the next monthly check the account will still be overquota even after the anticipated creation of compressed archives, we will proceed to the early removal of archive files. 

To monitor the used quota you can use the ``du -h`` command, to be used in the following way:

``$ du -h /home/username``

to monitor the *home* quota or

``$ du -h /mnt/beegfs/username``

to monitor the *work* quota. A periodic control of the quota limits is performed by the staff members.


Transferring files to and from the clusters
-------------------

**Note**: All the commands listed below have to be executed on your pc.

The two main commands to copy the data from your pc to the cluster and viceversa are the ``scp`` and the ``rsync`` commands, whose usage is already explained in Section `Copying files (basics)`_.

We now detail other situations that is important to know when tranferring data between your pc and the cluster.

Transfer a large number of small files
^^^^^^^^^^^^^^^^^^^^^^

Transferring a lot of small files will take a very long time with the ``scp`` command because of the overhead of copying every file individually. In such case, using the ``tar`` command will reduce the transfer time significantly. You can first create a ``tar`` (compress) archive, then ``scp`` it as a single file and then ``untar`` the file. But the most efficient way is to do all three operations in one go, without creating an intermediate file. You have to execute the following commands:
 
#. To copy the local directory ``My_Directory``, that contains several small files, on the Pleiadi cluster, at the path ``/path/to/my/destination``: ``$ tar cz ./My_Directory | ssh pippo@pleiadi.oact.inaf.it 'tar xvz -C /path/to/my/destination'``. With this command, the files in the local directory ``My_Directory`` are automatically compressed, transferred to the Pleiadi cluster, and again uncompressed in the ``/path/to/my/destination/My_Directory`` directory.
#. To copy the remote directory ``My_Directory_1``, at path ``/path/to/my/source``, that contains several small files, locally on your pc: ``ssh pippo@pleiadi.oact.inaf.it 'tar -C /path/to/my/source -cz My_Directory_1' | tar -xz``.
   
The ``-C`` option of the ``tar`` command compresses the files.

Transfer large files
^^^^^^^^^^^^^^^^^^^^^^
 
When transferring large files, it is better to copy them with the ``-C`` option of the ``scp`` command: in this way, the file is firstly compressed and then decompressed. You have to execute the following commands:
 
#. To copy a large file from your laptop to Pleiadi: ``$ scp -C /path-on-your-laptop/my_file.txt pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/``
#. To copy a large file from Pleiadi to your laptop: ``$ scp -C pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/my_file.txt /path-on-your-laptop/``

Resume interrupted transfers
^^^^^^^^^^^^^^^^^^^^^^
   
If, for any reason, a transfer is interrupted, instead of restarting it from scratch you can recover it, with the ``rsync`` command. The ``rsync`` command will compare the source and the destination directories and only transfer what needs to be transferred, e.g. missing files and modified files. For this purpose, the ``rsync`` command is used in this way: 

#. ``$ rsync -va /path-on-your-laptop/my_dir pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi``
#. ``$ rsync -va pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/my_dir /path-on-your-laptop`` 
   
It is important not to put the ending slash in the destination path ``/path-on-Pleiadi`` (in case of copies from your pc to the cluster) or ``/path-on-your-laptop`` (in case of copies from the cluster to your pc), as you might end up with a full copy of the directory inside the existing, partial, one. To check what will happen before you run the commands above, execute the ``rsync`` command with the ``-n`` option (dry-run), that performs a trial run with no changes made:
   
#. ``$ rsync -n /path-on-your-laptop/my_dir pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi``
#. ``$ rsync -n pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/my_dir /path-on-your-laptop`` 
   
If one large file is left half-transferred, you can resume it using the ``--partial`` option:
   
#. ``$ rsync --partial /path-on-your-laptop/my_large_file.txt pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/``
#. ``$ rsync --partial pippo@pleiadi.oact.inaf.it:/path-on-Pleiadi/my_large_file.txt /path-on-your-laptop/``







Copying files (basics)
-------------------

**Note**: All the commands listed below have to be executed on your pc.

#. **With the** ``scp`` **command**: the ``scp`` command works as the ``cp`` command except for the fact that it works across the network to copy files from one computer to another. You have to execute the following commands:

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

To edit a file, the most convenient way is to use a terminal-based editor. Some example are Vi, Vim, Emacs, Nano, mcedit, ne, slap, micro, pico, Joe or mped. **Vi, Vim, Emacs, and Nano are already installed on Pleiadi**.
