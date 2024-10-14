Monitoring of the used CPU hours (CPU hours saldo)
=================================================

To monitor the utilization of the CPU hours assigned when the account was activated by the board members, according to the user's request, you can use the SLURM report command ``sreport``, for example with the follwing options: 

``$ sreport -t Hour cluster AccountUtilizationByUser user=<username> start=M1/DD1/YY1 end=M2/DD2/YY2``

For example, the command::

  $ sreport -t Hour cluster AccountUtilizationByUser user=pippo start=6/10/22 end=6/13/22
    
    --------------------------------------------------------------------------------
    Cluster/Account/User Utilization 2022-06-10T00:00:00 - 2022-06-12T23:59:59 (259200 secs)
    Usage reported in CPU Hours
    --------------------------------------------------------------------------------
     Cluster         Account     Login     Proper Name     Used   Energy 
    --------- --------------- --------- --------------- -------- -------- 
    pleiadi-+           pippo    pippo+                      300        0
    
provides the CPU Hours used by the user ``pippo`` from 00:00:00 of 10/06/2022 to 23:59:59 of 12/06/2022.