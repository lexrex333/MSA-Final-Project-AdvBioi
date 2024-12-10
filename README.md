# MSA-Final-Project-AdvBioi
Welcome! This is a walkthrough of benchmarking 4 different multiple sequence alignment (MSA) tools using an Ebola dataset. These 4 MSA software tools include: MAFFT, Halign3, MUSCLE, and ClustalOmega. 

5 Ebola species were used in this benchmarking project: [Bundibugyo](https://www.ncbi.nlm.nih.gov/nuccore/NC_014373?report=genbank), [Reston](https://www.ncbi.nlm.nih.gov/nuccore/NC_004161?report=genbank), [Sudan](https://www.ncbi.nlm.nih.gov/nuccore/NC_006432?report=genbank), [Tai](https://www.ncbi.nlm.nih.gov/nuccore/NC_014372?report=genbank), and [Zaire](https://www.ncbi.nlm.nih.gov/nuccore/NC_002549?report=genbank). The DNA sequences can be downloaded from NCBI Genbank as a FASTA file. The Ebola dataset was recieved from: [Ebola Paper](https://www.sciencedirect.com/science/article/pii/S2352914818300029). 

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

2. FFT-NS-i: Slower, refinement for higher accuracy 

3. G-INS-1: For sequences with global homology (add --globalpair)

4. G-INS-i: For globally homologous sequences (add --globalpair)

5. L-INS-1: For sequences with local homology (add --localpair)

6. L-INS-i: For locally homologous sequences (add --localpair)

##### Time and MSA Output
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

In this case, 5335280 kbytes = 5208.87 MB ≈ 5.21 GB. MAFFT used 5.21 GB of memory to run and create a MSA.

There you go! On to the next software tool :).

### 2. Halign3
Halign3's paper can be found: [Paper](https://academic.oup.com/mbe/article/39/8/msac166/6653123)

Halign3's github can be found: [Github](https://github.com/malabz/HAlign-3)
#### Installation










