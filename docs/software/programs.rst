Programs and libraries installed on the Pleiadi cluster
======================================================

The following programs and libraries are installed on the Pleiadi cluster.

Programs
-------------------

Python
^^^^^^^^^^^^^^^^^^^^^^
Both Python2 (version 2.7.5) and Python3 (version 3.6.8) are installed on the Pleiadi cluster. The ``pip3`` command for the management of the installation of the Python packages is installed on Pleiadi, with the following version::

 $ pip3 --version
   pip 9.0.3 from /usr/lib/python3.6/site-packages (python 3.6)
   
It is possible to define Python virtual environments for a cutomized installation of the needed Python packages with the ``pyvenv`` and the ``pyvenv-3.6`` commands.

X11
^^^^^^^^^^^^^^^^^^^^^^
The graphic interface manager X11 is installed on the frontend node of the Pleiadi cluster.

**N.B.: X11 is not installed on the compute nodes of the Pleiadi cluster**.

To use the X11 tool you have to log in to the Pleiadi cluster with the ``-Y`` option, in this way:

``$ ssh -Y <username>@pleiadi.oact.inaf.it``

The X11 program allows to launch some graphic interfaces, such as the one of the Firefox browser with the following command:

``$ firefox``

Libraries
-------------------

zlib
^^^^^^^^^^^^^^^^^^^^^^

Other examples for SLURM submission scripts
-------------------------------------------

The ``--array`` option
-------------------

(`array <https://slurm.schedmd.com/job_array.html>`_)