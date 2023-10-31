*******************
Cluster structure
*******************

The Pleiadi cluster has the following components:

Catania
^^^^^^^^^^^^^^^^^^^^^^

#. **1 frontend node**
#. **72 compute nodes without GPUs** (12 with a RAM memory of 256 GB and 60 with a RAM memory of 128 GB)
#. **6 compute nodes with 1 GPU each** (4 of Tesla K40m type, of 12 GB of memory each, and 2 of Tesla V100 PCIe type, of 16 GB of memory each), with a RAM memory of 128 GB
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
| Number of nodes        | 78                                                       |
+------------------------+----------------------------------------------------------+
| Operating system       | CentOS Linux release 7.9.2009                            |
+------------------------+----------------------------------------------------------+
| Workload manager       | SLURM 21.08.5                                            |
+------------------------+----------------------------------------------------------+
| Storage volume         | 174 TB, BeeGFS parallel filesystem                       |
+------------------------+----------------------------------------------------------+


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
