# Andrographis paniculata
Transcriptome analysis of Andrographis paniculata

### Downloading the sequence files
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

### Quality Control

