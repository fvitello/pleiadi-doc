The cluster areas
==============

**Note:** There is no backup of the data stored on the Pleiadi cluster. Any removed file is lost forever. It is the user’s responsibility to keep a copy of the contents of their data in a safe place.
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
