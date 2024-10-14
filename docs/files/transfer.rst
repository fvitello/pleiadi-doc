
Transferring files to and from the clusters
===========================================

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
