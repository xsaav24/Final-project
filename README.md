# Final-project (saavedra)
The question that this project sought to solve is determining some of the spots where dingo and domesticated dog chromosomal information deviates. As trying to comb through the entire chromosomal sets of both dogs (_Canis lupus familiaris_) and dingoes (_Canis lupus dingo_) would be too tedious to handle, in addition to being highly taxing on the cluster and GitHub, it was better to comb through only one matching chromosome from each species. The initial chromosome of choice was the first one, but attempts to push data on it into GitHub failed due to how large its associated genomic size for both species was, which made the resulting files too large for comfort. Ultimately, the thirty-eighth chromosome was selected for its small size and ease of transferring data into the repository.

The first step was to create a dedicated directory for a new Git repository on the Opuntia cluster:

```
mkdir final_project
cd final_project
```
This project utilized genomic data from the NCBI database. To get the data onto the cluster, the following commands were used

```
wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/003/254/725/GCF_003254725.2_ASM325472v2/GCF_003254725.2_ASM325472v2_genomic.fna.gz # dingo
wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/011/100/685/GCF_011100685.1_UU_Cfam_GSD_1.0/GCF_011100685.1_UU_Cfam_GSD_1.0_genomic.fna.gz # dog
```
Proof of download:

![image](https://github.com/user-attachments/assets/8cc6d255-dfb5-4239-bf7b-ceb98e0312d1)

Unzipping files:

```
gzip -d GCF_003254725.2_ASM325472v2_genomic.fna.gz
gzip -d GCF_011100685.1_UU_Cfam_GSD_1.0_genomic.fna.gz
```

Resulting unzipped files:

```
GCF_003254725.2_ASM325472v2_genomic.fna # dingo unzip
GCF_011100685.1_UU_Cfam_GSD_1.0_genomic.fna # dog unzip
```

It was uncertain whether the unzipped files were wrapped--that is, whether the sequences were on multiple lines instead of just one. As a precaution, the following commands were utilized for the unzipped files: 

```
awk '{if(NR==1) {print $0} else {if($0 ~ /^>/) {print "\n"$0} else {printf $0}}}' GCF_003254725.2_ASM325472v2_genomic.fna > dingo.unwrap.fna # dingo unwrap
awk '{if(NR==1) {print $0} else {if($0 ~ /^>/) {print "\n"$0} else {printf $0}}}' GCF_011100685.1_UU_Cfam_GSD_1.0_genomic.fna > dog.unwrap.fna # dog unwrap
```

Confirming unwrapped files:

![image](https://github.com/user-attachments/assets/426e4f31-544d-4cd5-9886-54be5276efe4)

The unwrapped files contained the entire genomes of both canid species, but the goal was to compare only the thirty-eighth chromosomes of the dingo and dog to each other. The "grep" function was helpful in finding only the necessary part of the genomes:

```
grep "chromosome 38" dingo.unwrap.fna --after-context=1 > dingo.chromosome38.fna
grep "chromosome 38" dog.unwrap.fna --after-context=1 > dog.chromosome38.fna
```

_Note: The "--after-context=1" addition to the "grep" command was necessary to include the sequence into the output files. Without it, the "grep" search would have only included the header that precedes the sequence itself._

After creating the initial branch and README.md file, the following list of commands was utilized to commit the new files to the repo:

```
git init
git add dingo.chromosome38.fna
git add dog.chromosome38.fna
git commit -m "raw chromosome 38 data commit"
git remote add origin https://github.com/xsaav24/Final-project.git
git push -u origin master
```
Keeping in mind the purpose behind the ninth lab assignment of this semester, it was necessary to download MAFFT:

```
singularity pull --dir $(pwd) mafft.sif https://depot.galaxyproject.org/singularity/mafft:7.525--h031d066_1
```
