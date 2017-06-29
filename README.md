# multinode_sshwrapper
A singularity container demonstrating multi node MPI via SSH wrapper

This container is tested with Singularity Version 2.3 and above.
You will need to append the following lines to `/etc/singularity/init`

```
SINGULARITYENV_SINGULARITY_IMAGE="$SINGULARITY_IMAGE"
export SINGULARITYENV_SINGULARITY_IMAGE
```

You will need to bind mount `/etc/ssh` in `/etc/singularity/singularity.conf`

```
bind path = /etc/ssh
```

Pull this image onto the login node of a multi host HPC system and run it in single node mode
```
singularity pull shub://katakombi/multinode_sshwrapper
./katakombi_multinode_sshwrapper
```

In order to submit it dual node the nodes `n0720` and `n0726` need to be accessible via SSH keys. Do something like this:

```
singularity exec $PWD/katakombi-multinode_sshwrapper-master.img /.singularity.d/runscript n0726,n0720
```

The container needs to be accessible from each node and each node needs to have the same singularity installation / configuration as the host you are invoking the multi node MPI job on
