BootStrap: debootstrap
OSVersion: stable
MirrorURL: http://ftp.us.debian.org/debian/

%runscript
source /.singularity.d/environment
cd /tmp
if [[ $# -eq 0 ]]; then
  echo "Single node run on 4 cores, should take ~40 secs real, ~2mins30 user..."
  time mpirun -n 4 mdrun_mpi.openmpi -s /data/ion_channel.tpr -maxh 0.50 -noconfout -nsteps 500 -g logfile -v
else
  echo "Multi node run on 2 nodes, 2 cores should take ~40 secs real, ~2mins30 user..."
  time mpirun --oversubscribe -H "$1" -np 4  mdrun_mpi.openmpi -s /data/ion_channel.tpr -maxh 0.50 -noconfout -nsteps 500 -g logfile -v
fi

%environment
echo "### Setting up shell environment ..."
echo 
unset LANG; export LC_ALL="C"; export MKL_NUM_THREADS=1; export OMP_NUM_THREADS=1

%post
echo "Hello from inside the container"
apt-get update
apt-get -y --force-yes install vim gromacs gromacs-openmpi libipathverbs1 ssh
mv /usr/bin/ssh /usr/bin/ssh_orig
mkdir -p /data
ln -sf bash /bin/sh

%files
ssh_wrapper.sh /usr/bin/ssh
ion_channel.tpr /data
