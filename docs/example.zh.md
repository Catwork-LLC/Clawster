## AlphaFold

```bash
module load anaconda/python3.8/2020.07 alphafold
```

```bash
#!/bin/bash 

#SBATCH -J alphafold_example
#SBATCH -p dl
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=24
#SBATCH --gpus-per-node v100:1
#SBATCH --time=04:00:00
#SBATCH -A slurm-account-name

module load anaconda/python3.8/2020.07
module load alphafold

export TF_FORCE_UNIFIED_MEMORY='1'
export XLA_PYTHON_CLIENT_MEM_FRACTION='4.0'

python /N/soft/rhel7/alphafold/alphafold/run_alphafold.py \
  --fasta_paths=T1050.fasta \
  --output_dir= /N/slate/$USER \
  --model_names=model_1,model_2,model_3,model_4,model_5 \
  --max_template_date=2020-05-14 \
  --preset=full_dbs \
  --benchmark=False \
  --logtostderr \
  --flagfile=alphafold_flags
```
