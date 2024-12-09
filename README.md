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







