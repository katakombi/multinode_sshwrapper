# multinode_sshwrapper

## Description
This is a singularity container demonstrating multi node MPI via SSH wrapper.
The specialty is that the MPI installation is inside the container, and that no MPI on the host system is being used.
This is accomplished by packaging OpenMPI and the Infiniband/IPath drivers inside the container.

To demonstrate how it works you will run a short gromacs job using 
* 4 local MPI threads on localhost
* 2x2 MPI threads on two host specified by you

Both tasks should run about the same time.

## Limitations

* I have tested this using QDR InfiniBand HCA (Intel TrueScale) on our bwForCluster JUSTUS. I did not try to perform this via standard ethernet although it should be doable. I guess some parameters or environment variables would be needed to try this out. If you have other hardware you might need to install additional driver packages inside the container.
* The container ignores your RM and/or queueing system. Further changes would be needed to integrate that well into whatever it is you are using.

## Prerequisites


* This container is tested with Singularity Version 2.3 and above.
* You will need to append the following lines to `/etc/singularity/init`

```
SINGULARITYENV_SINGULARITY_IMAGE="$SINGULARITY_IMAGE"
export SINGULARITYENV_SINGULARITY_IMAGE
```

* You will need to bind mount `/etc/ssh` in `/etc/singularity/singularity.conf`

```
bind path = /etc/ssh
```

* In order to properly submit a multi node job the nodes must be accessible via SSH keys (password-less)

* The container needs to reside on a shared location which is accessible from each node.

* Furthermore, each node needs to have the same singularity installation / configuration as the host you are invoking the multi node MPI job on.

## Usage

Pull this image onto the login node of a multi host HPC system and run it in single node mode
```
singularity pull shub://katakombi/multinode_sshwrapper
./katakombi_multinode_sshwrapper.img
```
Use `top` to ensure that there are four local cores being occupied.

In order to submit it multi node (here `n0720` and `n0726`, you need to use the hostnames or IPs of your nodes, respectively) execute:

```
singularity exec $PWD/katakombi-multinode_sshwrapper-master.img /.singularity.d/runscript n0726,n0720
```

During the short run, use `top` on the compute nodes to ensure that on each of them two cores are being occupied.

## Credits

Credit to whom it is due: DOCKER_MPIWRAPPER_LINK_COMING!
