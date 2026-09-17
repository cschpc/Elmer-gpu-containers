# Roihu GPU container for Elmer 

The container installs
1. MMG, ParMmg
2. hypre 2.33.0
3. AMGX on commit 3188dce5bdc22e96407d609e398925bed6aa010d (newer commit wasn't tested)
4. Elmer branch devel


### Building the container
Example build script that builds `container.sif`.

```
#!/bin/bash
#SBATCH --job-name=elmer_container_build
#SBATCH --account=project_XXXXXXX
#SBATCH --partition=gpumedium
#SBATCH --time=00:40:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1 --cpus-per-task=72  # The product should be 72 if requesting 1 GPU per node
#SBATCH --gres=gpu:gh200:1  # Corresponds to 1 GPU per node

srun apptainer build --fakeroot container.sif Elmer_roihu.def
```

### Running Elmer with the container
```
srun --partition gpumedium --account project_XXXXXXX --time=00:30:00 --nodes 1 --ntasks 1 --cpus-per-task=72 --gres=gpu:gh200:1 apptainer run --nv --bind="$(csc-common-bind)" container.sif ElmerSolver
```

### CSC container documentation
https://docs.csc.fi/computing/containers/examples/