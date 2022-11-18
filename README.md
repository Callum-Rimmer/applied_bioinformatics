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

## Plan for today

One of the main ways we manage the vast array of programs you need to install is with a package manager. We are going to look at one today - this is what you will be using for your assessment.

Each piece of software you use will have a number of required programs (called dependencies) that must be installed in order for the program to work. Dependencies from different programs can often conflict with one another, which will prevent you from running the software.

Using a package manager, it will install a program along with its dependencies and place this in an 'environment'. You can have as many environments as you like, each with a different program installed in it. You activate the environment that you wish to use, and you can then use that particular program. This way you will never run into problems with different software conflicting with each other.

### Using Conda

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

