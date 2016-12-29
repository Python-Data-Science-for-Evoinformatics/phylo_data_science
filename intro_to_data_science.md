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

There are many ways to process data at the command line. These range from simple methods 
available in UNIX to elaborate and complex libraries in R or Python. In this section, we'll 
get to know Pandas a little better. We will use Pandas as a gateway to getting familiar with 
Python. Once we have learned how to carry out some common spreadsheet operations, we will
discuss Python programming more generally in the next chapter. This is by no means an exhaustive 
look at the Pandas library. It is simply a teaser to show some useful ways that you might 
interact with data in Python. 

Pandas was written by Wes McKinney to facilitate efficient data processing, manipulation and 
plotting in Python. The fundamental data type of Pandas is a dataframe, an object containing
rows and columns of data. For our Ants.csv data, the rows will be the fossil ants. The columns
will be the observations (fossil minimum ages, maximum ages, and taxonomy). It's instructive
to look at an example. At the command line, type:

```python
ipython notebook
```

This will open an iPython Notebook. These notebooks are Python instances, run on your computer,
but rendered in the browser. Browser rendering allows the notebook to have lots of nice features,
like plot rendering and clean visualization of dataframes. Each space for you to type is called
a 'cell'. A cell is a unit of code that can be run independently of all other cells. This 
is very nice for debugging, as we will see later.

In the first cell, type:

```python
import pandas as pd
```

Pandas is a library for the Python language. Libraries are sets of code, created for a purpose,
and contributed back to Python. Python, being an open source and user-developed language, 
welcomes this. When we tell Python to import a library, we are telling Python to look into
that codebase and make its functions open to us. The 'as pd' is called an alias. Many programmers
prefer to type fewer characters than more, and so will use aliases to shorten the names of 
libraries.

Let's load some data into Python, via the Pandas library:

```UNIX
ant_data = pd.read_csv('data/Ants.csv')
```

To view the data, type:

```python
ant_data
```

What we see is the data. We are able to call up the data to view because the data have been 
saved to a variable, ant_data, which was stored in memory as a dataframe when we called 
pd.read_csv. Let's unpack the syntax a little bit:

|Command Piece | Meaning|
|--------------|--------|
|pd. | Informs Python to look inside the Pandas library for the code we want to access |
| read_csv | The code we want to access. read_csv is called a function - a small unit of code that works together to perform a small set of operations. In this case, the read_csv code works to take in a file, read the contents, and convert the contents to a dataframe 
| ant_data = | Takes the output of read_csv (a dataframe) and saves it to a variable in memory, so we can access it later |

### Challenge:
Try this command again without saving it to a variable. What happens? Is it
what you expected? Why did this happen?

Let's take a closer look at the ant_data object. On the face of it, it looks very much like 
our Excel file. Now, we'll explore how to access data in this dataframe. Let's start by getting
a look at all the names in the file. There are a couple ways we can do this:

```python
ant_data[[0]]
```

or

```python 
ant_data.specimen
```

In our first block of code, we call the dataframe object and use what is called indexing 
to get the first column. Now, you may be thinking, what is going on with the notation? The 
brackets indicate that we will be accessing a column. Python counts from zero. This may be
somewhat surprising - that the language doesn't start with one. It's actually quite natural-
you weren't born at one year old, were you? But it still takes a little to get used to - 
programmers often verbally say the first item, but they mean the zeroeth item.

In our second block of code, we call the column by its name - specimen. This is a very natural
way to access the data. We gave attributes of the data names, why not use them?

##Challenge:

Try accessing other columns. When will it be useful to access by name? When will it not?






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