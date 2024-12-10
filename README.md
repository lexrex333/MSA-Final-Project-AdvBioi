# MSA-Final-Project-AdvBioi
Welcome! This is a walkthrough of benchmarking 4 different multiple sequence alignment (MSA) tools using an Ebola dataset. These 4 MSA software tools include: MAFFT, Halign3, MUSCLE, and ClustalOmega. 

5 Ebola species were used in this benchmarking project: [Bundibugyo](https://www.ncbi.nlm.nih.gov/nuccore/NC_014373?report=genbank), [Reston](https://www.ncbi.nlm.nih.gov/nuccore/NC_004161?report=genbank), [Sudan](https://www.ncbi.nlm.nih.gov/nuccore/NC_006432?report=genbank), [Tai](https://www.ncbi.nlm.nih.gov/nuccore/NC_014372?report=genbank), and [Zaire](https://www.ncbi.nlm.nih.gov/nuccore/NC_002549?report=genbank). The DNA sequences can be downloaded from NCBI Genbank as FASTA files. The Ebola dataset was recieved from: [Ebola Paper](https://www.sciencedirect.com/science/article/pii/S2352914818300029). 

This was all done using VSCode and a remote explorer. 

## Step 1: Preparing the FASTA Files
We need to make sure that our directory has the FASTA files we want to use. You will have to make a directory, and then insert each of the 5 Ebola FASTA files in this directory. I did this manually by opening my file explorer and dragging the downloaded FASTA files into my VSCode. It should look like this:

![1](https://github.com/user-attachments/assets/dcb683fa-1090-4a6b-9e04-5a7eac901779)

Now, using the terminal, enter the directory with all of the FASTA files using: 
```bash
cd Ebola_Virus*
```
Then we will concatenate all the sequences into 1 FASTA file, called Ebola.fasta, to use in each of our software tools:
```bash
cat Bundibugyo.fasta Reston.fasta Sudan.fasta Tai_Forest.fasta Zaire.fasta > Ebola.fasta
```
This should create a FASTA file:
![2](https://github.com/user-attachments/assets/e7e17e9b-5747-41f9-bf33-9a43502c5170)

## Step 2: Using the MSA Software Tools
Now that we have our FASTA file ready to be used, we can create MSAs using the software tools.

### 1. MAFFT
MAFFT's paper can be found here: [Paper](https://academic.oup.com/nar/article/30/14/3059/2904316).

MAFFT's github can be found here: [Github](https://github.com/GSLBiotech/mafft).
#### a) Installation
1) You will need to download the MAFFT version 7 from their [website](https://mafft.cbrc.jp/alignment/software/linuxportable.html). It should look like this:
![3](https://github.com/user-attachments/assets/dad2b0b2-aba1-4dfd-b6e9-8aa70454bdf6)

2) Then you can go to your terminal in VSCode, and go into the directory that has your FASTA file:
```bash
cd Ebola_Virus*
```
3) Drag the downloaded TGZ file into your Ebola_Virus* directory so VSCode can use it. 
4) Untar the TGZ file:
```bash
tar xfvz mafft-7.526-linux.tgz
```
It will then give you a directory that looks like this: 
![Screenshot 2024-12-09 172031](https://github.com/user-attachments/assets/22f4045c-cf25-4395-9ab9-96fc505fd835)

MAFFT is now ready to be used!

#### b) Running MAFFT
There are many options you can choose from – depending on what works best for your data: 

1. FFT-NS-2 method: Fast, but less accurate 

2. FFT-NS-i: Slower, refinement for higher accuracy (add --maxiterate #)

3. G-INS-1: For sequences with global homology (add --globalpair)

4. G-INS-i: For globally homologous sequences (add --globalpair + --maxiterate #)

5. L-INS-1: For sequences with local homology (add --localpair)

6. L-INS-i: For locally homologous sequences (add --localpair + --maxiterate #)

##### Time and MSA Output
(FYI: This step is optional - the next step (memory) will do time as well - this is for result purposes on what I exactly did.)

Then to run MAFFT you use this command (I used global because it relates the most for Ebola virus sequences):
```bash
(time mafft --globalpair --maxiterate 1000 Ebola.fasta) > Ebola_align_Mafft_global.aln 2> time_Mafft_global.log
```
I added time to the command so that it would track how long it took to run and had it put an additional file (time_Mafft_global.log) with the time in it. Maxiterate was also added to add more refinements to the alignment-the higher the number, the more refinement iterations (max number of 1000 refinements in this command).

This will output a MSA (Ebola_align_Mafft_global.aln) and a time log (time_Mafft_global.log).

The MSA file should look like this:

![Screenshot 2024-12-09 180422](https://github.com/user-attachments/assets/e3df7cbb-25cb-4115-954d-eff452dcd9c2)

The time log will give file output like this:

![Screenshot 2024-12-09 180658](https://github.com/user-attachments/assets/11642bf7-59ea-42ef-a529-0980826e9b01)

##### Memory and MSA Output
I had completely forgot about memory usage, so I ran the command again, to get the memory usage (this does contain the amount of time it took to run as well, so you can skip the last command and just do this command):
```bash
/usr/bin/time -v mafft --globalpair --maxiterate 1000 /home/aavalos4/Ebola_Virus_Bioi_500/Ebola.fasta > Ebola_align_Mafft_global_1000.aln 2> /home/aavalos4/Ebola_Virus_Bioi_500/MAFFT_output/memory_Mafft_global_1000.log
```
This will output the same MSA alignment file, and another file that contains the memory usage and time: 

![Screenshot 2024-12-09 181102](https://github.com/user-attachments/assets/03c9a3ed-4ca3-4cc9-aaaa-881d3e1c3cb3)

The maximum resident set size is the maximum amount of memory that the run uses. 

In this case, 5335280 kbytes = 5208.87 MB ≈ 5.21 GB. 
MAFFT used 5.21 GB of memory to run and create a MSA.

There you go! On to the next software tool :).

### 2. Halign3
Halign3's paper can be found here: [Paper](https://academic.oup.com/mbe/article/39/8/msac166/6653123)

Halign3's github can be found here: [Github](https://github.com/malabz/HAlign-3)
#### a) Installation
To install Halign3, follow these steps in your terminal on VSCode:

1) Activate your conda environment:
```bash
conda activate base
```
2) Add channels to conda:
```bash
conda config --add channels conda-forge 

conda config --add channels bioconda 

conda config --add channels malab
```
3) Install the required package "openjdk=11" for running Halign3:
```bash
conda install -c conda-forge openjdk=11
```
4) Install Halign3:
```bash
conda install -c malab halign
```
5) Test Halign3 to make sure it's working:
```bash
halign -h
```
#### b) Running Halign3
Since we already have our FASTA file (Ebola.fasta), we don't need to download anything else.

##### Time and MSA Output
(Just like MAFFT, you can skip this step - as the next step will cover this as well.)

The bald command (the command with no pathways or additional usage) looks like this:
```bash
Halign –o Ebola_Halign3.fasta.aln -t 5 –c 6 Ebola.fasta
```
However, if you did want to do this time step, this is the command I used:
```bash
(time /home/aavalos4/miniconda3/bin/halign -o /home/aavalos4/Ebola_Virus_Bioi_500/Ebola_Halign3.fasta.aln -t 5 -c 6 /home/aavalos4/Ebola_Virus_Bioi_500/Ebola.fasta) 2> time_Halign3.log
```
Which will then give you an output like this:

LOG file:

![4](https://github.com/user-attachments/assets/0df7f188-b43d-49d3-a601-9ae18efe34d5)

MSA file: 

![Screenshot 2024-12-10 054257](https://github.com/user-attachments/assets/7f64da6d-cdb9-42ab-ac1e-74798a8067c2)

##### Memory and MSA Output
To check the memory, time, and get the MSA file use the command:
```bash
/usr/bin/time -v /home/aavalos4/miniconda3/bin/halign -o /home/aavalos4/Ebola_Virus_Bioi_500/Ebola_Halign3.fasta.aln -t 5 -c 6 /home/aavalos4/Ebola_Virus_Bioi_500/Ebola.fasta 2>/home/aavalos4/Ebola_Virus_Bioi_500/Halign3_output/memory_Halign3.log
```
This gave me:

![Screenshot 2024-12-10 050018](https://github.com/user-attachments/assets/63fd5a54-258f-42de-ba0d-fb7bd354967b)

So the total memory used by Halign3 to create a similar MSA was 19428156 kbytes = 18972.81 MB ≈ 18.53 GB. 
Halign3 used a maximum of 18.53 GB of memory to run and do this MSA for the Ebola Dataset.
#### c) Alignment Check
Halign3 also gives the option to check your alignment result to make sure that it worked correctly.

To do this, you would first need to add this command to manipulate the FASTA file:
```bash
conda install seqkit
```
Then do this command:
```bash
seqkit stat /home/aavalos4/Ebola_Virus_Bioi_500/Ebola.fasta /home/aavalos4/Ebola_Virus_Bioi_500/Halign3_output/Ebola_Halign3.fasta.aln
```
This will give you an output that is similar to this!:

![Screenshot 2024-12-10 050612](https://github.com/user-attachments/assets/429b4918-007c-4cd2-9af9-5920bba8df35)

Now we have completed using Halign3, and will move on to the next software tool!

### 3. MUSCLE
MUSCLE's paper can be found here: [Paper](https://www.nature.com/articles/s41467-022-34630-w)

MUSCLE's github can be found here: [Github](https://github.com/rcedgar/muscle)
#### a) Installation
To install MUSCLE5:

1) Download the tar.gz file from this [website](https://github.com/rcedgar/muscle/releases).

![Screenshot 2024-12-10 052531](https://github.com/user-attachments/assets/9ac493f4-ea55-4a9a-b48c-d88f04c55071)

2) Manually move it from file explorer into your VSCode.
3) You will have to extract the file:
```bash
tar -xvzf muscle-5.3.tar.gz
```
4) Go into the new muscle-5.3 directory:
```bash
cd muscle-5.3
```
5) Lastly, make sure MUSCLE is installed by seeing the version number:
```bash
muscle -version
```
It should give you output in the terminal like this:

![Screenshot 2024-12-10 053652](https://github.com/user-attachments/assets/7417d9ac-3eec-4b0d-9eed-8f6cbb911a9b)

#### b) Running MUSCLE
To run MUSCLE, here is the bald command:
```bash
muscle –align Ebola.fasta -output Ebola_align_MUSCLE.fasta
```
##### Time and MSA Output
(Can skip this step.)
```bash
(time muscle –align /home/aavalos4/Ebola_Virus_Bioi_500/Ebola.fasta - output /home/aavalos4/Ebola_Virus_Bioi_500/Ebola_align_MUSCLE.fasta) 2> time_MUSCLE.log
```
Output would look like this:

LOG file: 

![Screenshot 2024-12-10 054026](https://github.com/user-attachments/assets/f4c0f615-f844-4a06-a636-1ceefb900530)

MSA file:

![image](https://github.com/user-attachments/assets/d1fc3f54-57c6-4a8b-9900-8b251e8322ea)
##### Memory and MSA Output
(Can go straight to this step instead of last step.)

Put in this command: 
```bash
/usr/bin/time -v muscle --align /home/aavalos4/Ebola_Virus_Bioi_500/Ebola.fasta --output /home/aavalos4/Ebola_Virus_Bioi_500/Ebola_align_MUSCLE.fasta 2>/home/aavalos4/Ebola_Virus_Bioi_500/MUSCLE_output/memory_MUSCLE.log
```
The output will give you:

![Screenshot 2024-12-10 054544](https://github.com/user-attachments/assets/5d4db212-98b6-4c2f-8a6a-f1792ec51709)

The memory used for MUSCLE to make a MSA: 148642164 = 145140.96 MB ≈ 141.74 GB at its peak.
MUSCLE used about 141.74 GB to run and make a MSA.

#### c) Alignment Checking
MUSCLE allows you to check the alignment it just created. You can do this by adding "stratified" to the command - this will give you an ensemble FASTA with 16 alignments:
```bash
(time muscle -align /home/aavalos4/Ebola_Virus_Bioi_500/Ebola.fasta -stratified -output /home/aavalos4/Ebola_Virus_Bioi_500/Ebola.efa) 2> time_MUSCLE_error.log
```
(I had added time, just to see how long it would take to run.)

The time LOG file should look similar to this:

![Screenshot 2024-12-10 055403](https://github.com/user-attachments/assets/1baa1769-c4de-4f2a-93c6-7769afc5081d)

With the time at the bottom of it:

![image](https://github.com/user-attachments/assets/398b45e2-0374-499c-8d7c-c7741bb200d7)

Be aware: It will take time to run, so I suggest making a screen before running it.

Then you will need to run this command to measure the dispersion:
```bash
(time muscle -disperse /home/aavalos4/Ebola_Virus_Bioi_500/MUSCLE_output/Ebola.efa) 2> time_MUSCLE_disperse.log
```
It will give an output like this:

![Screenshot 2024-12-10 060050](https://github.com/user-attachments/assets/54d71dfc-866f-4099-93a7-c9a596ad9917)

In my analyses, D_LP = 0.1223 and D_Cols = 0.2141. This means that the dispersion of local pairwise alignments and dispersion across alignment columns has a bit of variation. The closer to 0, the more similar the 16 MSAs are to each other. In this case, it seems to have alignment errors, or variation in their biological sequences, just by being over 0.05. In this benchmarking project, it won't affect results when comparing software tools, but it might be useful to check further into this when trying to make trees or when using these MSAs to do other analyses. 

Now, we move on to the last software tool.

### 4. ClustalOmega
ClustalOmega's paper can be found here: [Paper](https://pubmed.ncbi.nlm.nih.gov/21988835/).

ClustalOmega's github can be found here: [Github](https://github.com/GSLBiotech/clustal-omega).
#### a) Installation
To install ClustalOmega:

1) Download from the [website](http://www.clustal.org/omega/).
   
![Screenshot 2024-12-10 061948](https://github.com/user-attachments/assets/8b4d9bc9-cb11-46f7-97f7-fa63c51e7011)

2) Make sure it is in your downloads:

![Screenshot 2024-12-10 062049](https://github.com/user-attachments/assets/edeea864-1028-4df9-ae68-881a6459d784)

3) Insert into your VSCode:

![Screenshot 2024-12-10 062126](https://github.com/user-attachments/assets/2bb49a16-083b-498c-9544-294c19735890)

4) Go to the directory where it is located and then make the permissions executable using this command:
```bash
chmod +x clustalo-1.2.4-Ubuntu-x86_64
```

5) Make sure clustalo is working - it should have "rwxr" and clustalo in green:
```bash
ls -l clustalo-1.2.4-Ubuntu-x86_64
```

![Screenshot 2024-12-10 062401](https://github.com/user-attachments/assets/95f3579a-04ef-47e9-a7e5-2b6bc9993ab6)

6) Check to see that it is working:
```bash
./clustalo-1.2.4-Ubuntu-x86_64 --help
```
It should give similar output to this:

![Screenshot 2024-12-10 062527](https://github.com/user-attachments/assets/623975ec-560e-4a91-bf3d-6406790f948a)

#### b) Running ClustalOmega
To run ClustalOmega, here is the bald command:
```bash
./clustalo-1.2.4-Ubuntu-x86_64 -i Ebola.fasta -o Ebola_align_Clustalo.aln
```
Tip: Make sure to stay in the directory that you have clustalo in for it to run or add the pathway.
##### Time and MSA Output
(Can skip this step.)
```bash
(time ./clustalo-1.2.4-Ubuntu-x86_64 -i /home/aavalos4/Ebola_Virus_Bioi_500/Ebola.fasta -o /home/aavalos4/Ebola_Virus_Bioi_500/Ebola_align_Clustalo.aln) 2> /home/aavalos4/Ebola_Virus_Bioi_500/time_Clustalo.log
```
Output should look like this:

Time LOG file:

![Screenshot 2024-12-10 063008](https://github.com/user-attachments/assets/60052d62-8da9-49c4-99e1-3fc4b8653b37)

MSA file:

![Screenshot 2024-12-10 063126](https://github.com/user-attachments/assets/3429bed9-f0f0-47e8-8693-30e0c123f80c)

##### Memory and MSA Output
(Can go straight to this step.)
```bash
/usr/bin/time -v /home/aavalos4/Ebola_Virus_Bioi_500/clustalo-1.2.4-Ubuntu-x86_64 -i /home/aavalos4/Ebola_Virus_Bioi_500/Ebola.fasta -o /home/aavalos4/Ebola_Virus_Bioi_500/Ebola_align_Clustalo.aln 2> /home/aavalos4/Ebola_Virus_Bioi_500/memory_Clustalo.log 
```
The output should give you something similar:

![Screenshot 2024-12-10 063307](https://github.com/user-attachments/assets/070be00b-d5ec-45e9-be4c-e0fc9dd26b61)

The memory used by ClustalOmega to make a MSA is 2368316 kbytes = 2312.62 MB ≈ 2.26 GB.

Now, we have finished making an MSA for all software tools with the Ebola dataset!!

## Step 3: Comparing the MSA Software Tools
I first wanted to compare all 4 Software Tools with the results I got from when I ran them.
### A) Memory Usage, Time, CPU Usage
1) I made an excel spreadsheet with all the results that I had obtained from the LOG files.

![Screenshot 2024-12-10 064315](https://github.com/user-attachments/assets/2da68b50-c606-4516-b1e9-fa4935952c20)

2) I downloaded it as a CSV file.

![Screenshot 2024-12-10 064359](https://github.com/user-attachments/assets/2b4140fe-1c73-41cb-baf0-0334df1aa5ab)

3) I moved the CSV file, manually, into my VSCode.

![Screenshot 2024-12-10 064437](https://github.com/user-attachments/assets/b593fe64-41e6-491e-b03e-6ee8c53769fe)

It should look something like this:

![Screenshot 2024-12-10 064530](https://github.com/user-attachments/assets/1a70ad20-b29d-46c3-8bfe-7ef093dc96fe)

(I suggest adding quotation marks around all of the words like in the picture above, because it will give you an error in the code.) 

4) I then made a python file in VSCode to make plots for each section of information:

Using pandas and matplotlib, and grabbing information from our CSV file:
```bash
#import what we need to make the plots
import pandas as pd
import matplotlib.pyplot as plt

#reading the excel sheet (changed into CSV) 
data = pd.read_csv('/home/aavalos4/Ebola_Virus_Bioi_500/Software_Running_Compare.csv') 
#print(data.columns)
```

For Memory Usage:
```bash
#making a LINE PLOT for Memory Usage of each one

#making figure size
plt.figure(figsize=(8,5))
#getting data we want to use and how we want to put it into plot
plt.plot(data['Tool'], data['Memory Usage (GB)'], marker= 'o')
#making a title
plt.title('MEMORY USAGE')
#making a y-axis name
plt.ylabel('Memory Usage (GB)')
#making a x-axis name
plt.xlabel('Tool')
#to put y-axis in numerical order
plt.ylim(0.0,145.0) 
#to put numbers above the dots
for tool, memory in zip(data['Tool'], data['Memory Usage (GB)']):
    plt.annotate(f'{memory:.2f}',(tool, memory), textcoords = "offset points", xytext=(0,5), ha='center')
#so names don't look ugly and they aren't cut off
plt.xticks(rotation=45)
#making everything pretty with no overlaps
plt.tight_layout()
#saving figure for future use in presentation
plt.savefig('/home/aavalos4/Ebola_Virus_Bioi_500/Memory_Usage_Plot.png', dpi =300)
#showing plot
plt.show()
```
Memory Usage output should look like this:

![image](https://github.com/user-attachments/assets/da789c96-a531-4289-9534-0c07c0ad961a)



For amount of Time:
```bash
#making figure size
plt.figure(figsize=(8,5))
#getting data we want to use and how we want to put it into plot
plt.plot(data['Tool'], data['Elapsed Time (Sec.)'], marker= 'o')
#making a title
plt.title('TIME')
#making a y-axis name
plt.ylabel('Time (Seconds)')
#making a x-axis name
plt.xlabel('Tool')
#to put y-axis in numerical order
plt.ylim(0,350)
#to put numbers above the dots
for tool, time in zip(data['Tool'], data['Elapsed Time (Sec.)']):
    plt.annotate(f'{time}',(tool, time), textcoords = "offset points", xytext=(0,5), ha='center')
#so names don't look ugly and they aren't cut off
plt.xticks(rotation=45)
#making everything pretty with no overlaps
plt.tight_layout()
#saving figure for future use in presentation
plt.savefig('/home/aavalos4/Ebola_Virus_Bioi_500/Time_Plot.png', dpi =300)
#showing plot
plt.show()
```
Time Output should look like this:

![Screenshot 2024-12-10 065544](https://github.com/user-attachments/assets/3d93d199-29f8-48c3-8043-be5f20e1eec7)


For CPU Usage:
```bash
#making a LINE PLOT for CPU %

#making figure size
plt.figure(figsize=(8,5))
#getting data we want to use and how we want to put it into plot
plt.plot(data['Tool'], data['CPU Usage(%)'], marker= 'o')
#making a title
plt.title('CPU USAGE')
#making a y-axis name
plt.ylabel('CPU Usage(%)')
#making a x-axis name
plt.xlabel('Tool')
#to put y-axis in numerical order
plt.ylim(0,460)
#to put numbers above the dots
for tool, cpu in zip(data['Tool'], data['CPU Usage(%)']):
    plt.annotate(f'{cpu}',(tool, cpu), textcoords = "offset points", xytext=(0,5), ha='center')
#so names don't look ugly and they aren't cut off
plt.xticks(rotation=45)
#making everything pretty with no overlaps
plt.tight_layout()
#saving figure for future use in presentation
plt.savefig('/home/aavalos4/Ebola_Virus_Bioi_500/CPU_Usage_Plot.png', dpi =300)
#showing plot
plt.show()
```
CPU Usage Output should look like this:

![Screenshot 2024-12-10 065454](https://github.com/user-attachments/assets/d578ee8a-c524-41b7-83a8-a096f2344861)

### B) AliStat - Measurement of MSA Completeness
Alistat is a software tool that will allow us to analyze each of the MSA's created by each software tool, to see how "complete" the MSA is. It will check to see if there is any missing data, such as gaps or unknown nucleotides. 

It creates equations using the idea that there are (m) sequences and (n) sites. If you are further interested in the equations and how it works, you can check out the [paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC7671350/) and the [github](https://github.com/thomaskf/AliStat). 

#### Installing Alistat
1) Download the tar.gz file from the [website](https://github.com/thomaskf/AliStat/releases).

![image](https://github.com/user-attachments/assets/a73be060-0e84-4e53-8621-719f0940f582)

2) I manually added the file into VSCode and then extracted the file:
```bash
tar -zxvf /home/aavalos4/Ebola_Virus_Bioi_500/AliStat-1.15-Linux.tar.gz
```
It should output something like this:

![image](https://github.com/user-attachments/assets/135fcbf0-b7a7-4c08-a487-80be88466fe4)

3) Now, head into the directory of Alistat and go into the bin:

![image](https://github.com/user-attachments/assets/1fb7a94f-a9b4-425b-bfb0-1476fe863b79)

4) Make sure Alistat is working by putting in:
```bash
./alistat
```
It should output:

![image](https://github.com/user-attachments/assets/eed68969-361c-4b15-a028-d1d8ed6c93ae)

5) Run a practice test with a given example Alistat gives:
```bash
./alistat /home/aavalos4/Ebola_Virus_Bioi_500/AliStat-1.15-Linux/examples/test.fas 1 > alioutput_practice2.txt
```
The "1" stands for Single nucleotides - you will have to use a different number if this isn't the case, such as di-nucleotides or amino acids. 

Also, the last part (> alioutput_practice2.txt) is optional. Without it, it will still give you a summary.txt as output, but I just did it anyway to make sure that it ran all the way. 

Summary.txt will return:

![image](https://github.com/user-attachments/assets/3669da85-2629-40d3-8578-b6fca04172f8)

Alioutput_practice2.txt will return:

![image](https://github.com/user-attachments/assets/c99c0e91-0166-46a0-b134-a52e0d51a38c)

(Also keep in mind, Alistat will ALWAYS return a file called (Summary.txt) so after every run, make sure to change the name so it doesn't overrun the file.)

#### Running Alistat on the 4 MSA Software Tools

1) MAFFT
```bash
./alistat /home/aavalos4/Ebola_Virus_Bioi_500/MAFFT_output/Ebola_align_Mafft_global_1000.aln 1 > MAFFT_ali.txt
```
Output:

![image](https://github.com/user-attachments/assets/0a872a04-6321-4686-876d-84b5d8b9a120)

2) Halign3
```bash
./alistat /home/aavalos4/Ebola_Virus_Bioi_500/Halign3_output/Ebola_Halign3.fasta.aln 1 > Halign3_ali.txt
```
Output:

![image](https://github.com/user-attachments/assets/6a5f8e0f-26db-4fcb-a9f7-0b113e852a27)

3) MUSCLE
```bash
./alistat /home/aavalos4/Ebola_Virus_Bioi_500/MUSCLE_output/Ebola_align_MUSCLE.fasta 1 > MUSCLE_ali.txt
```
Output:

![image](https://github.com/user-attachments/assets/51be2c92-60a6-4702-b974-d0ad7d71e211)

4) ClustalOmega
```bash
./alistat /home/aavalos4/Ebola_Virus_Bioi_500/ClustalOmega_output/Ebola_align_ClustalOmega.aln 1 > ClustalOmega_ali.txt
```
Output:

![image](https://github.com/user-attachments/assets/c66ada1b-a4ea-4e47-87c8-56380df36b40)

#### Organize Data from Alistat Outputs
1) I made an excel sheet with all the information given by Alistat output.

![image](https://github.com/user-attachments/assets/318b5f88-c0cb-44d1-8c56-ddfecf859958)

2) I downloaded it as a CSV file and manually moved it to VSCode. It should look like this:

![image](https://github.com/user-attachments/assets/76b1f2ba-842f-4240-8179-84ae8df326cb)

Make sure to put quotations around all the words (like the picture above) so you don't get an error. 

3) I made a new python file in VSCode.

Using pandas and matplotlib again, also calling the CSV file:
```bash
#import what we need to make the plots
import pandas as pd
import matplotlib.pyplot as plt

#reading the excel sheet (changed into CSV) I made of all the numbers of alistat output
data = pd.read_csv('/home/aavalos4/Ebola_Virus_Bioi_500/Alistat_software_compare.csv') 
```

I decided to make plots for 3 of the analyses Alistat made:
   1) Completeness Score: How complete the alignment is.
   2) Sequence Completeness Score: How complete the sequences are.
   3) Pairs of Sequences Completeness Score: How many homologous pairs in sequences i and j.

(For the benchmarking project, I don't think it was necessary to do number 3, but since I did it, I will put it in here as well.) 

1. Completeness Score
```bash
#making a BAR CHART for overall completeness

#fixing figure size
plt.figure(figsize = (8,5))

#making bars different colors so its not bland
colors = ['blue', 'orange', 'green', 'purple']
#getting the data we want in the plot
plt.bar(data['Tool'], data['Completeness (C) score for the alignment (Ca)'], color = colors)

#making plot title
plt.title('Alignment Completeness (Ca) Across Tools')
#making y axis label
plt.ylabel('Completeness Score (Ca)')
#making x axis label
plt.xlabel('Tool')
#making a more zoomed in version
plt.ylim(0.80,1)
#rotating the tool names to be able to read them better
plt.xticks(rotation = 45)
#organizes the plot better so things aren't overlapping in the space it has
plt.tight_layout()
#to save plot
plt.savefig('/home/aavalos4/Ebola_Virus_Bioi_500/Zoomed_Overall_Completeness_Plot.png', dpi =300)
#showing the plot
plt.show()
```
Output:

![image](https://github.com/user-attachments/assets/194e7886-9db3-49c5-811a-c2fcc20ce96e)

2. Sequence Completeness Score
```bash
#making a LINE PLOT for sequence completeness

#making figure size
plt.figure(figsize=(8,5))
#getting data we want to use and how we want to put it into plot
plt.plot(data['Tool'], data['(Cr_max) Maximum C-score for individual sequences'], label = 'Cr_max', marker= 'o')
plt.plot(data['Tool'], data['(Cr_min) Minimum C-score for individual sequences'], label = 'Cr_min', marker= 'o')
#making a title
plt.title('Sequence Completeness Scores')
#making a y-axis name
plt.ylabel('Sequence Completeness Score')
#making a x-axis name
plt.xlabel('Tool')
#making it so it covers 0 to 1; unzoomed version
#plt.ylim(0,1)
#need a legend to differentiate the colors on plot
plt.legend()
#so names don't look ugly and they aren't cut off
plt.xticks(rotation=45)
#making everything pretty with no overlaps
plt.tight_layout()
#saving figure for future use in presentation
plt.savefig('/home/aavalos4/Ebola_Virus_Bioi_500/Sequence_Completeness_Plot.png', dpi =300)
#showing plot
plt.show()
```
Output:

![image](https://github.com/user-attachments/assets/790afab9-92a8-4851-8fcf-31195af9826b)

3. Pairwise Sequence Completeness
```bash
#making a SCATTER PLOT for pairwise completeness scores

#error on column names
#print(data.columns)

#making the figure size
plt.figure(figsize=(8,5))
#getting info for plot and how to incorporate it in
plt.scatter(data['Tool'], data['(Cij_max, i!=j) Maximum C-score for pairs of sequences'], label='Cij_max', color='blue')
plt.scatter(data['Tool'], data['(Cij_min, i!=j) Minimum C-score for pairs of sequences '], label='Cij_min', color='red')
#making a title
plt.title('Pairwise Completeness Score')
#making the x-axis name
plt.xlabel('Tool')
#bc olive will kill me if you are tricked by the values of the y-axis - however not a good visual so went back but used it for comparison
#plt.ylim(0,1)
#making a legend to see what means what
plt.legend()
#making it for nice and readable; moves it to the side so doesn't overlap
plt.xticks(rotation=45)
#making it more pretty
plt.tight_layout()
#saving figure for future use in presentation
plt.savefig('/home/aavalos4/Ebola_Virus_Bioi_500/Zoomed_Pairwise_Completeness_Plot.png', dpi =300)
#showing the plot
plt.show()
```
Output:

![image](https://github.com/user-attachments/assets/944691b5-2205-4f10-8fd3-3267cda70a05)






























