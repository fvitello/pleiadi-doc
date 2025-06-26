*******************
Cluster structure
*******************

The Pleiadi infrastructures is distributed on the following sites:

Bologna
^^^^^^^^^^^^^^^^^^^^^^

#. **1 frontend node for scheduling only**
#. **48 compute nodes without GPUs**

The below table summarizes the main features of the Bologna Pleiadi cluster:

+------------------------+----------------------------------------------------------+
| Architecture           | Cluster Linux x86_64                                     |
+------------------------+----------------------------------------------------------+
| Nodes interconnection  | Omni-Path HFI Silicon 100 Series, 100 Gbits interconnect |
+------------------------+----------------------------------------------------------+
| Service network        | Ethernet 1 Gbits                                         |
+------------------------+----------------------------------------------------------+
| CPU Model              | Intel(R) Xeon(R) CPU E5-2697 v4 @ 2.30GHz                |
+------------------------+----------------------------------------------------------+
| Number of nodes        | 48                                                       |
+------------------------+----------------------------------------------------------+
| Operating system       | Debian 11                                                |
+------------------------+----------------------------------------------------------+
| Workload manager       | SLURM 20.11.7                                            |
+------------------------+----------------------------------------------------------+
| Storage volume         | 600 TB, Lustre parallel filesystem                       |
+------------------------+----------------------------------------------------------+

Catania
^^^^^^^^^^^^^^^^^^^^^^

#. **1 frontend node**
#. **56 compute nodes without GPUs** (18 with a RAM memory of 256 GB and 38 with a RAM memory of 256 GB)
#. **10 compute nodes with 4 GPU each** (4 of Tesla V100 type, of 16 GB of memory each), with a RAM memory of 128 GB
#. **1 storage volume** of 174 TB with BeeGFS parallel filesystem

The below table summarizes the main features of the Catania Pleiadi cluster:

+------------------------+----------------------------------------------------------+
| Architecture           | Cluster Linux x86_64                                     |
+------------------------+----------------------------------------------------------+
| Nodes interconnection  | Omni-Path HFI Silicon 100 Series, 100 Gbits interconnect |
+------------------------+----------------------------------------------------------+
| Service network        | Ethernet 1 Gbits                                         |
+------------------------+----------------------------------------------------------+
| CPU Model              | Intel(R) Xeon(R) CPU E5-2697 v4 @ 2.30GHz                |
+------------------------+----------------------------------------------------------+
| Number of nodes        | 56                                                       |
+------------------------+----------------------------------------------------------+
| Operating system       | CentOS Linux release 7.9.2009                            |
+------------------------+----------------------------------------------------------+
| Workload manager       | SLURM 21.08.5                                            |
+------------------------+----------------------------------------------------------+
| Storage volume         | 174 TB, BeeGFS parallel filesystem                       |
+------------------------+----------------------------------------------------------+

Pleiadi-GPU cluster:  

+------------------------+---------------------------------------------------------------------+
| Architecture           | Cluster Linux ppc64le                                               |
+------------------------+---------------------------------------------------------------------+
| Nodes interconnection  | InfiniBand Mellanox ConnectX-5 (MT27800), 100 Gbps EDR interconnect |
+------------------------+---------------------------------------------------------------------+
| Service network        | Ethernet 1 Gbits                                                    |
+------------------------+---------------------------------------------------------------------+
| CPU Model              | IBM POWER9, 2.3–3.8 GHz                                             |
+------------------------+---------------------------------------------------------------------+
| Number of nodes        | 10                                                                  |
+------------------------+---------------------------------------------------------------------+
| Operating system       | Almalinux 8.10                                                      |
+------------------------+---------------------------------------------------------------------+
| Workload manager       | SLURM 21.08.5                                                       |
+------------------------+---------------------------------------------------------------------+
| Storage volume         | 28 TB, BeeGFS parallel filesystem  (temporary)                      |
+------------------------+---------------------------------------------------------------------+

Compute nodes main features
"""""""""""""""""""""""""""

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

GPU nodes main features
"""""""""""""""""""""""""""

The below table summarizes the main features of the compute nodes of the Pleiadi cluster, which are useful to set the Slurm options in the submission scripts or for interactive jobs submissions: 

+-----------------------------+------------------------------------------+
| Number of CPUs              | 128 (including hyperthreading)           |
+-----------------------------+------------------------------------------+
| Number of sockets           | 2                                        |
+-----------------------------+------------------------------------------+
| Number of cores per socket  | 18                                       |
+-----------------------------+------------------------------------------+
| Number of threads per core  | 4                                        |
+-----------------------------+------------------------------------------+
| RAM memory                  | 256 GB                                   |
+-----------------------------+------------------------------------------+



Palermo
^^^^^^^^^^^^^^^^^^^^^^

#. **1 frontend node**
#. **4 compute nodes with 4 GPU each** (4 of Tesla V100 type, of 16 GB of memory each), with a RAM memory of 128 GB (in the future will be 8)
#. **1 storage volume** of 7,8 TB with OrangeFs parallel filesystem (temporary)

Palermo-GPU cluster:  

+------------------------+---------------------------------------------------------------------+
| Architecture           | Cluster Linux ppc64le                                               |
+------------------------+---------------------------------------------------------------------+
| Nodes interconnection  | InfiniBand Mellanox ConnectX-5 (MT27800), 100 Gbps EDR interconnect |
+------------------------+---------------------------------------------------------------------+
| Service network        | Ethernet 1 Gbits                                                    |
+------------------------+---------------------------------------------------------------------+
| CPU Model              | IBM POWER9, 2.3–3.8 GHz                                             |
+------------------------+---------------------------------------------------------------------+
| Number of nodes        | 4 (8 in the future)                                                 |
+------------------------+---------------------------------------------------------------------+
| Operating system       | Almalinux 8.10                                                      |
+------------------------+---------------------------------------------------------------------+
| Workload manager       | SLURM 21.08.5                                                       |
+------------------------+---------------------------------------------------------------------+
| Storage volume         | 7,8 TB with OrangeFs  (temporary)                                   |
+------------------------+---------------------------------------------------------------------+


GPU nodes main features
"""""""""""""""""""""""""""

The below table summarizes the main features of the compute nodes of the Pleiadi cluster, which are useful to set the Slurm options in the submission scripts or for interactive jobs submissions: 

+-----------------------------+------------------------------------------+
| Number of CPUs              | 128 (including hyperthreading)           |
+-----------------------------+------------------------------------------+
| Number of sockets           | 2                                        |
+-----------------------------+------------------------------------------+
| Number of cores per socket  | 18                                       |
+-----------------------------+------------------------------------------+
| Number of threads per core  | 4                                        |
+-----------------------------+------------------------------------------+
| RAM memory                  | 256 GB                                   |
+-----------------------------+------------------------------------------+





Trieste
^^^^^^^^^^^^^^^^^^^^^^

#. **1 frontend node**
#. **60 compute nodes without GPUs** (all with 256 GB of RAM)
#. **6 compute nodes with 1 GPU each** (Tesla K80 and 128 GB of RAM)
#. **1 storage volume** of 480 TB with BeeGFS parallel filesystem

The below table summarizes the main features of the Trieste Pleiadi cluster:

+------------------------+----------------------------------------------------------+
| Architecture           | Cluster Linux x86_64                                     |
+------------------------+----------------------------------------------------------+
| Nodes interconnection  | Omni-Path HFI Silicon 100 Series, 100 Gbits interconnect |
+------------------------+----------------------------------------------------------+
| Service network        | Ethernet 1 Gbits                                         |
+------------------------+----------------------------------------------------------+
| CPU Model              | Intel(R) Xeon(R) CPU E5-2697 v4 @ 2.30GHz                |
+------------------------+----------------------------------------------------------+
| Number of nodes        | 66                                                       |
+------------------------+----------------------------------------------------------+
| Operating system       | CentOS Linux release 7.9.2009                            |
+------------------------+----------------------------------------------------------+
| Workload manager       | SLURM 19.05.0                                            |
+------------------------+----------------------------------------------------------+
| Storage volume         | 480 TB, BeeGFS parallel filesystem                       |
+------------------------+----------------------------------------------------------+
