# Applied Bioinformatics

Commands that we have previously looked at:

```
ls
cd
pwd
clear
echo
cowsay
man
cat
head
tail
more
less
touch
mkdir
nano
rm
```

There's a few more commands I want to cover: `cut`, `grep` and the pipe `|`

#### Cut

We can use the `cut` command to extract text from a file or an output from a command.

Lets make a list of numbers using `echo` in comma-separated values format and direct that to a file:
```
echo "a,b,c,d,e" > cut_example.txt
```
How can we print the contents of this file to the terminal?

What if we only to extract the letter `c` from that file? We can use `cut` to do it.
```bash
cut -f3 -d',' cut_example.txt
```
`-f3` means we are extracting the 3rd field (column).\
`-d','` is how we set the delimiter (what the columns are going to be separated by).

So what we have said is break up the file into columns based on commas, then print the 3rd column.

#### Grep

The `grep` command is a pattern searcher. You supply it with some text and it will search for that text in whatever file or command output you give it.

Using the file you created for the `cut` command...
```bash
grep a cut_example.txt
```
`grep` will output each line that contains a match to your query text.

#### The pipe

`|` is used to direct the output of one command into another.

One example is counting the number of files in a directory...
```
ls | wc -l
```
`ls` will normally list the contents of a directory in the terminal.

But if we pipe it into the word count command it will instead give us the number of lines the `ls` command printed i.e. the number of files/folders.

## Plan for today

One of the main ways we manage the vast array of programs you need to install is with a package manager. We are going to look at one today - this is what you will be using for your assessment.

Each piece of software you use will have a number of required programs (called dependencies) that must be installed in order for the program to work. Dependencies from different programs can often conflict with one another, which will prevent you from running the software.

![linux-package-manager-explanation copy](https://user-images.githubusercontent.com/72881801/202696960-655d33be-1e86-46f8-bd0e-c99be19e7609.jpg)

Using a package manager, it will install a program along with its dependencies and place this in an 'environment'. You can have as many environments as you like, each with a different program installed in it. You activate the environment that you wish to use, and you can then use that particular program. This way you will never run into problems with different software conflicting with each other.

![conda_logo](https://user-images.githubusercontent.com/72881801/202698181-ccb6bbc3-c6c9-4733-9cfe-e47f06001546.png)


The package manager we are going to use is called Conda. This should already be installed on your virtual machines.

Let's check Conda is working by typing...
```bash
conda --version
```

#### Creating an environment

The first program we are going to install is called `fastqc`. This program allows you to check the quality of the data from a DNA sequencer.

Before we install the program, we need to create an environment for it...
```
conda create -n fastqc
```
Now we need to turn on this environment...
```
conda activate fastqc
```
We're now in our `fastqc` environment. All we need to do now is install the program. Conda is also used for installing programs, not just managing environments.

Conda installs programs by looking for them in `channels`. To see what `channels` your Conda setup is using, type the command:
```
conda config --show channels
```
`Bioconda` should be one of them. This contains most of the programs you need for bioinformatics.

Now we've checked we have the appropriate channels setup, we can install fastqc in our environment by running...
```
conda install fastqc
```
Now fastqc is installed in our fastqc environment. We can check this is installed by typing `conda list`.

Let's check `fastqc` is working by typing `fastqc -h`, if it prints the help page the program is working.

You can turn off your environment by typing `conda deactivate`.

There are a few other conda commands which you need to know...

`conda config --add channels channel_name`          This adds a specific channel if the one you want isn't being used by default

`conda search program_name`                         This searches for a specific program and tells you whether you can install it

`conda info --envs`                                 This prints a list of all your environments

There is a way you can bundle creating an environment and installing a program. Using the example of `fastqc`, you could instead type...
```
conda create -n fastqc fastqc
```
Where the syntax is `conda create -n environment_name program_name`

Another problem you may come across is that some older bioinformatics tools are designed to run on older versions of python. You can specify the version of python an environment is created with by typing...
```
conda create -n environment_name python=python_version
```
## Trying out a small workflow

### Checking the quality of DNA sequencing data

Now that you have an environment setup with fastqc, let's use it.

Download the two sequence read files from NOW. The R1 file contains all the DNA sequencing data for the forward reads, R2 contains the data for the reverse reads. [Check out this webpage for more information.](https://www.illumina.com/science/technology/next-generation-sequencing/plan-experiments/paired-end-vs-single-read.html)

Make sure you have activated your fastqc environment.

What commands do you need to use to move into your Downloads folder and then make a directory called `workshop`.

Move your read files into the workshop folder.

You can run `fastqc` on your read files by typing:
```
fastqc *.fastq -t 4 
```
Can you answer these questions:

1. What does the `-t 4`mean
2. What are we saying with `*.fastq`?
3. How would you find out what parameters fastqc needs


fastqc should output a html file per read file, you can view this with Firefox. You can find out more information about the statistics fastqc outputs by looking at the [project webpage.](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)

The read quality checks out, so we can move onto the next step.

### Double checking adapter sequences have been removed from our reads

We add adapter sequences to our samples of DNA prior to sequencing so that the DNA binds to the flowcell in the sequencer, as you can see below.

![image](https://user-images.githubusercontent.com/72881801/202675159-54f51a37-45d1-404d-8e68-a2b89bc0e7df.jpeg)

We need to double check that the adapter sequences have been removed before doing any more analysis on the files.

The program we are going to use is called `trimmomatic`.

1. Create an environment called `trimmomatic`
2. Activate this environment
3. Install `trimmomatic`, using conda, in this environment
4. Download the `NexteraPE-PE.fa` file from NOW and place this in your `workshop` directory

Trimmomatic uses a few more parameters than fastqc. The command we are going to use is:
```
trimmomatic PE -threads 4 bacteria_R1.fastq bacteria_R2.fastq bacteria_R1p.fastq bacteria_R1u.fastq bacteria_R2p.fastq bacteria_R2u.fastq ILLUMINACLIP:NexteraPE-PE.fa:2:20:10
```

`bacteria_R1.fastq` and `bacteria_R2.fastq` are your input files.

`ILLUMINACLIP:NexteraPE-PE.fa:2:20:10` is where the adapter sequences are stored that trimmomatic will look for in your data, along with some extra tweaks to the settings

The output files are...\
`bacteria_R1p.fastq` and `bacteria_R1u.fastq` (paired and unpaired)\
`bacteria_R2p.fastq` and `bacteria_R2u.fastq` (paired and unpaired)

The unpaired files can be deleted since they contain all the garbage reads, we just want the paired files.

Now let's tidy things up:

1. Make a folder called `raw_reads` and move your original files from NOW into this folder
2. Make a folder called `trimmed_reads` and move your paired files from `trimmomatic` into this folder
3. Remove the `.zip` files from fastqc
4. Make a folder called `fastqc` and move your html files into this folder

### Assembling the genome

Now our read files have been quality checked and trimmed, we can assemble the reads into a complete genome.

The program we are going to use to do this is called `spades`

1. Create an environment called `spades`
2. Activate the environment and install the program `spades`
3. In your `workshop` folder, create a folder called `spades`

Now we can run the main command:
```
spades.py -1 trimmed_reads/bacteria_R1p.fastq -2 trimmed_reads/bacteria_R2p.fastq -o spades -t 4
```
The `-1` flag is for your trimmed forward reads file.\
The `-2` flag is for your trimmed reverse reads file.\
The `-o` flag is the output folder\
The `-t 4` flag is for using for CPU threads

What we're doing is trying to piece the sequencing reads into larger fragments called contigs. The contigs then get stiched together to form the assembled genome.

![image](https://user-images.githubusercontent.com/72881801/202688417-8d9fdf14-2009-4ed4-b17c-0dd48dda76df.png)

Once spades has finished assembling your genome, you will have a `contigs.fasta` file. This file contains all the contigs spades was able to piece the sequence reads into.

Q.  Each contig begins with a header line containing all the information about that contig, starting with a greater than `>` symbol.\
  a)  How can we view this file?\
  b)  How could you extract all the header lines i.e. contig names and print them to the terminal?
  
### Species identification

Now we have our assembled genome, let's find out what species of bacteria it belongs to.

You can upload your fasta file to [PubMLST](https://pubmlst.org/species-id), which uses ribosomal multi-locus sequence typing for species identification.

### Searching for antibiotic resistance genes

We can also upload our genome to the Comprehensive antibiotic-resistance gene database [(CARD)](https://card.mcmaster.ca/analyze/rgi)

Does the genome encode any antibiotic resistance genes?
