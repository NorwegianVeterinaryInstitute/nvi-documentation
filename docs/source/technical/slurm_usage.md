# How to use slurm

On the SAGA cluster all compute resources are managed by a system called [Slurm](https://slurm.schedmd.com/documentation.html). If we want to analyze data on Saga we need to request resources and if those are accepted a job is started. We do this because we can not run large analysis on the login nodes. If you try to run a large job anyway, it will be killed automatically after 30 minutes. 

For a very detailed overview of using the slurm queue system on saga you can check out the pages linked here: [Sigma2 - running jobs](https://documentation.sigma2.no/jobs/overview.html)

There are two ways of running analysis on SAGA using the slurm system
1. using a slurm script
2. using a interactive command.


