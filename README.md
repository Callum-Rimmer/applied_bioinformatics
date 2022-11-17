# Applied Bioinformatics

Commands that we have previously looked at:

`ls` and `ls -l`\
`cd`\
`pwd`\
`clear`\
`echo`\
`cowsay`\
`man`\
`cat`\
`head` / `tail`\
`more` / `less`\
`touch` / `mkdir`\
`nano`\
`rm` and `rm -r`

## Plan for today

One of the main ways we manage the vast array of programs you need to install is with a package manager. We are going to look at one today - this is what you will be using for your assessment.

Each piece of software you use will have a number of required programs (called dependencies) that must be installed in order for the program to work. Dependencies from different programs can often conflict with one another, which will prevent you from running the software.

Using a package manager, it will install a program along with its dependencies and place this in an 'environment'. You can have as many environments as you like, each with a different program installed in it. You activate the environment that you wish to use, and you can then use that particular program. This way you will never run into problems with different software conflicting with each other.

### Accessing the files for today

You've previously downloaded files using GitHub. We are going to use the same method today.

Type the following into your terminal:

```bash
git clone https://github.com/Callum-Rimmer/applied_bioinformatics.git
```
This should download the repository, including all the files for today.

### Using Conda

The package manager we are going to use is called Conda. This should already be installed on your virtual machines.

Let's check Conda is working by typing...
```bash
conda --version
```

#### Creating an environment
