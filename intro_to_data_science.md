#Chapter One: An Introduction to Project Organization

##Introduction: How can we maintain tractable data structures for use with phylogenetic 
projects?

"I just wasted five hours running an analysis on the wrong input file."

"I can't remember where I saved my output."

"I accidentally overwrote my raw data."

Many of us have probably said one or more of the above sentences. When you're balancing 
multiple projects and lots of data, it can be easy to lose track of files.
One of the biggest challenges to any project is placing project files in a structure that 
is easy for you, the scientist, to access and maintain. 

This chapter will cover the basics of project organization and data organization and management. 
At the end of this chapter, you should be familiar with:

1. *Concept:* How to store project input files, scripts and project outputs
2. *Concept:* How to organize your data in a way that is reusable for you, and for others
3. *Hands-On:* How to use the command line to create directories, move files and view the 
files that you have

## Creating Project Workspaces

This book will discuss not solely the Dendropy computing library, but efficient Python computation
for phylogenetic analyses. In this section, we are going to address a very specific aim:
setting up projects on your computer in a way that will allow you to manage your projects 
efficiently. We will use hands-on examples throughout this section.

When managing a project, you want to *keep files together as much as possible*. Doing this 
makes it easier to document your project, and avoid losing data files. For example, let's create
a directory at the command line for all of our project data. We will do this in UNIX, not 
Python right away. The reason for this is that most servers, Macintosh and Linux machines 
will have the tools to do this available. UNIX is a very commonly-used tool. Open your 
command line and type:

```UNIX
mkdir phylo_data_science
```

This has created a new directory in your computer called phylo_data_science. You can think of this
as a workspace for all the phylo_data_science files. If a file is related to this tutorial, 
it should go here. The directory is currently empty. You can try navigating to this directory 
using your normal file viewer. We're now going to put some content into this directory. 
Open a test editor of your choosing. In the file write a short note about why you have 
created this directory. For example, in my directory, I wrote 

```
This directory contains files for a tutorial on using data science to learn phylogenetics
```

save this as README.txt. Now, at the command line, type:

```
cd phylo
```

then hit tab. What happens when you hit tab? The whole file name pops up! This behavior is 
called *tab completion*, and it's incredibly useful. Tab completion saves you from typing 
out the whole file name (and potentially making typos). If you press tab and nothing comes 
up? Then you know that either you haven't entered enough characters to make a unique match 
- for example, if you have phylo_data_science and phylo_Bayesian, or that the file doesn't 
exist in the directory you're in. 

We are now in the phylo_data_science directory. If you type 

```UNIX
ls
```
you should see the README file you made. If you don't see it, check that you've saved the 
file correctly. If you have, check that you're in the right directory by typing

```UNIX
pwd
```

for print working directory. If something went wrong in your cd command, this will let you 
know.

Now, we will make four subdirectories.

```UNIX

mkdir scripts
mkdir data
mkdir output
mkdir documentation
```

These four subdirectories will all serve important purposes. Our scripts directory will house
the Python code we will write. We will talk in further chapters about why it is important
and useful to keep all of the scripts for a single project together. For now, just know that 
it is. 

Our data directory will house our raw data. We want our raw data to be housed on its own. If 
we make a mistake in how we save output files, we can always go back to our raw data and 
run the analysis again ... so long as we have maintained the integrity of our raw data. 
Once we have populated our data directory with the necessary data, we do not write to it. 
A simple motto for this philosophy is that *data are read-only*. 

We do, however, write to our output directory. We will talk in future chapters about using 
Python to make readable and informative output file names. For now, just know that the results 
of _any_ analysis go in the output directory.

Documentation is where we can put miscellaneous, informative files. For example, papers that
you are reading for your project, cost sheets for sequencing, talk slides.

This is the basic setup all chapters of this book will rely on. This manner of file organization
is very transparent: anyone looking at your file directory can understand what components 
of your research project are stored where. If you follow this structure, when you go to 
publish your paper, you can simply archive your entire project directory to meet most 
funders' and journal's guidelines for providing data and software code. And who couldn't 
use a little less on their plate when it comes time to submit a paper?

###Recap: Our Three Goals

So far, we have introduced two important concepts: *keep files together as much as possible* 
and *data are read-only*. The first principle means to keep data, output and code related to
one project in one place on your computer to increase your organization. The second principle
means to treat raw data as untouchable. Any outputs of analyses should be kept separate to
avoid loss of data, and to ensure you can rerun any steps as needed.

In learning these two concepts, we have used hands-on commands: mkdir to create directories, 
ls to look at their contents and cd to navigate. We also learned to check our navigation with
the pwd command and to use tab-complete to increase the efficiency of our typing.

#Chapter Two: Moving From Spreadsheets to Python

##Introduction

"I know I made a plot of this ... I just can't remember how."

"This looks totally different on my computer."

"My coworker doesn't have Excel."

Almost everyone used a spreadsheet program to manage data at some point in our career. These
graphical programs offer a clean interface to view and manage the data we work so hard to 
collect. But these types of programs can also introduce problems in biological workflows.

There are three main issues with using Excel and other similar programs to carry out computations:

1. *They can be black boxes.* Most spreadsheet programs are closed-source, meaning that their 
inner workings might not be examineable by users because the source code is not available. 
Exceptions to this do exist, such as Open Office. Behaviors can also vary between platforms
(i.e., Macintosh vs. Windows) or across versions of the software in ways that are somewhat 
unpredictable.
2. *You need to maintain a separate log file with your commands.* As we'll see in this section,
programmatically working with data provides an inherent log of all the commands you did. As
long as you provide the version number of the Python distribution and any libraries you're 
working with, a colleague should be able to reproduce what you did exactly. By contrast, to
do this in a spreadsheet program requires either writing out exactly what data were
 highlighted  and what options were clicked on.
3. *Reuse and batching is tricky.* When you are using a spreadsheet program, you are 
generally performing operations on one file at a time. Many spreadsheet programs have a system,
often called macros, that allows for a series of operations to be carried out in several 
spreadsheet files. But you often still need to click each file to open it, and start your
macro. Writing code that can be reused because it explicitly lists every operation performed 
allows us to process large batches of files in a way that simply isn't possible with spreadsheet 
programs.

In this chapter, we will discuss moving data management and analysis past the traditional
spreadsheet paradigm. Along the way, we will learn about how to store data in a way that is
both human- and machine-readable. Using a test dataset on ant (Formicidae) taxonomy, we will
do some hands-on exercises to use Python programming to perform common tasks that we might 
ordinarily perform in a spreadsheet program. At the end of this chapter, you will be familiar 
with:

1. *Concept:* How to store data and documentation in a way that is useful to you, colleagues
and is machine-readable.
2. *Concept:* How the Pandas Python library can be used to make analyses more reproducible
and scaleable.
3. *Hands-On:* Commands to perform common spreadsheet operations, such as data sorting,
grouping and cleaning using Pandas.

## Storing Data

When you save a file in Excel, you don't simply save the data. You save, encoded in binary,
information about cell positioning, coloring, and other document attributes. In the previous 
section, we mentioned that the behavior of a file loaded into a spreadsheet viewing program 
can vary between versions of the software. This is not because the data are changing. This
is because there are often subtle changes to how the data are displayed or how statistics 
are calculated. These subtle changes can cause dramatically different renderings of files
across versions and platforms. 

All of this extraneous information also makes the file harder to read at the command line, 
or in Python. For example, in the data directory, you will find two data files: Ants.csv and 
Ants.xlsx. At the command line, type:

```UNIX

head Data/Ants.csv
head Data/Ants.xlsx
```

Which of these files are you able to visualize? 

The Ants.csv file is stored in what is called a *flat file*. Flat files are plain text - 
they don't contain any characters that can't be typed with a keyboard, or viewed at the 
command line. Any fancy formatting in a plain text file comes from the viewer. For example,
this book is written in plain text. The nice formatting you see is the result of rendering 
software. If you were to view this file at the command line, you would still be able to 
access all of the data within it. Because flat files can be read by both human and machine,
they are often considered preferable to files with extensive encoding of extra information.

For the purposes of this lesson, we will show you how to get data out of spreadsheet files 
(such as Excel files), but we will predominantly be describing how you can avoid using these
file types. 

Inside each flat file, data should be organized with each row being an observation, and each 
column being the variables observed. If you open the Ants.csv file, you will note that each
row corresponds to one fossil ant. Each column is labeled as a variable observed about the 
ant - taxonomy, age of the fossil and notes. Most programmatic ways of handling data assume 
this structure.  

You will also notice that all the column names have underscores, rather than spaces. Most 
programming languages will assume that a space indicates the end of a name, so it's best to
avoid spaces. Lastly, notice that the data directory also contains a README. This README
tells the user what data files they should expect to find when they download your data.

## The Pandas Library

## Common Spreadsheet Operations






Following this chapter, there is a short practicum where we will use what we've learned to 
subsample taxonomic data for use in a later phylogenetic analysis.

    Pandas for parsing data
    Grouping data by taxonomic level
    Choosing a subset of data to include - at random, and with clade-structured subsampling
    Writing out the relevant info about what taxa were subsampled

2) Reading character data

    Subsampling character data according to part (1)
    Managing the namespaces
    Using pack to ensure that all data are represented in both datasets (important in BEAST, will shortly be unimportant in RevBayes)
    Writing out the data

3) Processing the output

    Pruning non-data tips from a tree (ie occurrance time taxa)
    Summarizing
    Some sort of short capstone, such as showing how to extract the width of the HPD to demonstrate something about sampling or something.