# Using a graphical window in SAGA
It is possible to use graphical interface on Saga.
Login to saga using the `-Y` option

<div style="background-color:silver">

_**PRACTICAL EXERCISE:**_

```bash
ssh -Y <username>@saga.sigma2.no
```

Do a simple test that to check that it is working

```bash
xeyes
```

type `ctrl+C` to kill xeyes

</div>

You can read more on [SAGA manual: X11 forwarding](https://documentation.sigma2.no/getting_started/create_ssh_keys.html#x11-forwarding)


## Concrete example
Bellow is an example that we using IGV, and is required for the tutorial: [Visualizing assembly and associated reads pileup with IGV](../tutorials/Visualisation_assembly_reads_pileup_IGV.md)

Ask for ressources on Saga to use IGV (do not forget `--x11`)

```bash
# Example of how to use the ressources interactively
srun --account=nn9305k --mem-per-cpu=16G --cpus-per-task=1 --qos=devel \
--time=0:30:00 --x11 --pty bash -i
conda activate igv
# launching IGV
igv
```

A window with IVG should open.

You will see several error messages that it tries to load a genome file, ignore those (it is because there are some server data that try to be loaded through internet, but we have no internet access from the compute nodes. Anyway we do not need those data to work with.)


</div>
</br>
