### Steps to start running RNA-seq on O2 cluster 
1. Login to O2 cluster with your HMS username & password using Putty or .... 
2. Begin an interactive session by running:<br>
	`srun --pty -p interactive -t 0-2:0:0 --mem 150G -c 15 /bin/bash`<br>
	You can request extra memory or multiple cores (up to 20). More information is found [`here`](https://wiki.rc.hms.harvard.edu/display/O2/Using+Slurm+Basic).
3. Install the required modules by running:<br> 
	`module load conda2/4.2.13`<br>
	`module load rcbio/1.0`<br>
	`module load cellranger/2.2.0`<br>
	`module load bcl2fastq/2.20.0.422`<br>
	`module load gcc/6.2.0`<br>
	`module load star/2.5.4a`<br>
	`module load samtools/1.9`<br>
	`module load python/2.7.12`<br>
	`module load htseq/0.9.1`<br>
4. Create a new directory to put all your files and name it **RNA-seq**.<br>
	`mkdir RNA-seq`
5. Generating a genome index using STAR should be done only once. 
     - Go to **RNA-seq** folder you created in *step 4*, generate a new folder and name it **Index** as such:<br>
	 `ls RNA-seq`<br><br>
   	 `mkdir Index`<br><br>
     - Go to **Index** folder you just created and generate a new genome index as such:<br>
   	 `ls Index`<br><br>
	 `STAR --runMode genomeGenerate --genomeDir /home/kb246/immdiv-bioinfo/karni/mouse_genome/ --genomeFastaFiles /home/kb246/immdiv-bioinfo/karni/mouse_genome/Mus_musculus.GRCm38.dna.primary_assembly.fa --sjdbGTFfile /home/kb246/immdiv-bioinfo/karni/mouse_genome/Mus_musculus.GRCm38.97.gtf --sjdbOverhang 50`<br>
	You can replace the directory in the above (`/home/kb246/immdiv-bioinfo/karni/mouse_genome/...`) with the one your fasta and gft files are found.<br>
	This step can be ignored if you an index from a shared directory, which has the lastest version of mouse genome. Otherwise you will have to create a new one as shown in the above.<br>
6. Go inside the folder you generated in the previous step and create a new folder, name it **fastqFiles** as such:<br>
	`ls RNA-seq`<br>
	`mkdir fastqFiles`<br>
	Copy all your fastq files into this folder created using WinSCP (for Windows) & (for Linux).<br>
7. Download **pipeline.sh** from Github and copy it into **RNA-seq** folder using (WinSCP).<br>
8. Run the commands in the file **pipeline.sh** by running:<br>
	`runAsPipeline pipeline.sh "sbatch -p short -t 20:0 -n 1" noTmp run`