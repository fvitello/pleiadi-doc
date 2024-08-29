*******************
Userguide for Pleiadi cluster
*******************

Note: along the entire document the dollar $ sign indicates the shell prompt.

The current guide illustrates the usage of the HPC@PLEIADI system (Pleiadi cluster) of the INAF-Osservatorio Astrofisico di Catania (OACT) institution based on the SLURM workload manager system.


Gentlemen’s agreement
====================

#. Using this cluster for research purposes, the user automatically autorizes the Pleiadi's Staff to publish her/his personal data (e.g name, surname, and research group) and the data associated to the research on the website `Pleiadi <http://www.pleiadi.inaf.it>`_, in all the other paper publications broadcast by HPC@PLEIADI (e.g. annual reports and presentations), and on any other support system.
#. Using a HPC@PLEIADI system for research purposes, the user accepts to cite the PLEIADI services on all her/his scientific publications in papers, conference proceedings, books, and any other support type. We suggest the following citation "This work made use of PLEIADI, a computing infrastructure installed and managed by INAF-USCVIII, under the proposal [here the reference of the proposal]".


Rules for the Pleiadi cluster usage
====================

#. Calculations, simulations, etc. have NEVER to be launched directly from command line on the frontend node, in order not to risk to make unusable the frontend node or other shared resources (penalty of account blocking and task interruption). ALWAYS pass through the execution queues of the SLURM scheduler (also in case of compilation).
#. It is NOT allowed to employ the resources for usage different from the research and/or teaching purposes.
#. Every user is responsable for every activity either performed by or attributable to herself/himself. Therefore, the account sharing is discouraged and it is only allowed if communicated in time to the HPC Staff. In general, it is NOT allowed to share sensitive data (e.g. username and password) that the user undertakes to properly store.


TITLE-TOD
====================

.. toctree::
   :maxdepth: 1

   first_step/index.rst
   SLURM/index.rst
   managingfiles/index.rst
   modules/index.rst
   libraries/index.rst



   





   
   






    





