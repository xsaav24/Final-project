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
