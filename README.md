# Andrographis paniculata
Transcriptome analysis of Andrographis paniculata

### 1. Downloading the sequence files
dl.sh
```bash
#!/bin/bash

#sets the target bioproject ID
bioproject=PRJNA326960

#makes a directory based on the target bioproject
mkdir $bioproject


for line in $(cat ${bioproject}.txt); do                    #opens a txt file containing SRA name for each line
        
        fasterq-dump $line -e 6 -p -S -O ./$bioproject      #runs fasterq-dump on each line to download &
                                                            #saves them in the directory of the bioproject
done
```

### 2. Quality Control
fastp.sh
```bash
#!/bin/bash

#sets the target bioproject ID
bioproject=PRJNA326960

#sets the target directory output. Named according to the nth time/batch it was run.
time=first

#makes a directory based on the target bioproject
mkdir ../$time/res_fastp/$bioproject

#goes to the directory and sets it on the top of the stack; for easy navigation
pushd ../../seq_files

for line in $(cat ${bioproject}.txt); do
        echo Running on $line ......

        fastp \
        --in1 ./$bioproject/${line}_1.fastq \
        --out1 ../qual_ctrl/$time/res_fastp/$bioproject/trimmed.${line}_1.fastq \
        --unpaired1 ../qual_ctrl/$time/res_fastp/$bioproject/unpaired.${line}_1.fastq \
        --in2 ./$bioproject/${line}_2.fastq \
        --out2 ../qual_ctrl/$time/res_fastp/$bioproject/trimmed.${line}_2.fastq \
        --unpaired2 ../qual_ctrl/$time/res_fastp/$bioproject/unpaired.${line}_2.fastq \
        --merge \
        --merged_out ../qual_ctrl/$time/res_fastp/$bioproject/merged.${line}.fastq \
        --detect_adapter_for_pe \
        --dedup \
        --trim_poly_g \
        --cut_front \
        --cut_tail \
        --cut_mean_quality 20 \
        --length_required 50 \
        --thread 10 \
        --json ../qual_ctrl/$time/res_fastp/$bioproject/${line}.json \
        --html ../qual_ctrl/$time/res_fastp/$bioproject/${line}.html \
        &> ../qual_ctrl/$time/res_fastp/$bioproject/${line}.log #redirects the stderror and stdout and
                                                                #writes on the log file; for process assessment 

done

#goes back to the previous directory before 'pushd'
popd
```
