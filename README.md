# AmpliconArchitect 
[![https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg](https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg)](https://singularity-hub.org/collections/1640)

## Batch Job Script
```
#!/bin/bash
#PBS -N aa_test
#PBS -S /bin/bash
#PBS -l nodes=1:ppn=1
#PBS -l mem=5gb
#PBS -l walltime=2:00:00
#PBS -j oe

module load use.own
module load AmpliconArchitect

cd $PBS_O_WORKDIR
runAA $AA 
```
1. Copy contents of aa.pbs (example above) to a file in your home directory.

2. Modify the runAA command to suit your job.

3. Open terminal on cluster login node and submit the job script:

```
$ qsub aa.pbs
```

## Web Interface Job Script
```
#!/bin/bash
#PBS -N aa_web_test
#PBS -l nodes=1:ppn=1
#PBS -l mem=5gb
#PBS -l walltime=2:00:00
#PBS -j oe

# get tunneling info
port=$(shuf -i8000-9999 -n1)
node=$(hostname -s)
user=$(whoami)

cd $PBS_O_WORKDIR

# print tunneling instructions 
echo -e "
1. SSH tunnel from your workstation using the following command:

   ssh -L ${port}:${node}:${port} ${user}@$PBS_O_HOST

   and point your web browser to http://localhost:${port}.
"

# load modules
module load use.own
module load AmpliconArchitect

runAA flask run --port=${port}
```
1. Copy contents of aa_web.pbs (example above) to a file in your home directory.

2. Open terminal on cluster login node and submit the job script:

```
$ qsub aa_web.pbs
```

### Connect to Web session
1. Check output file (*jobname*.o*JOBNUM*) for details.

Example output file: aa_web_test.o252668
```
1. SSH tunnel from your workstation using the following command:

   ssh -L 8793:node01:8793 tester@loginnode

   and point your web browser to http://localhost:8793.
```

2. Open second terminal and run tunneling command from the output file:
```
$ ssh -L 8793:node01:8793 tester@loginnode
```
3. Open a browser and enter the URL from the output file:
```
http://localhost:8793
```
You should now be connected to your AmpliconArchitect web session that is running on a cluster compute node. To stop the web session, close the browser window. If you need to reconnect, repeat steps. If you're done with your web session, remember to stop the job with qdel.
