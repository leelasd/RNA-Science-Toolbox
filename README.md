# RNA Science Toolbox

The RNA Science Toolbox provides a Python API (PyRNA) to do RNA science. PyRNA allows you to:

* parse RNA data from "classical" file formats (PDB, CT, FASTA, VIENNA,...) and convert them into easy-to-use and easy-to-analyse data structures:
  * [Pandas Series and DataFrames](http://pandas.pydata.org/pandas-docs/stable/dsintro.html)
  * "PyRNA objects" (as defined in the module pyrna.features).
* compute RNA data from RNA algorithms (see below for details) and convert them into Pandas data structures and PyRNA objects,
* recover RNA data from public databases ([PDB](http://www.rcsb.org/pdb/home/home.do), [RFAM](http://rfam.sanger.ac.uk),...) and convert them into Pandas data structures and PyRNA objects,
* deploy some functionalities as REST Web services.

This project is related to the [DockeRNA project](https://github.com/fjossinet/DockeRNA) which provides Docker images containing the RNA algorithms you may need.

You can [follow this project on twitter](https://twitter.com/RnaSciToolbox) to get updates as they happen.

# Basic installation

To use the RNA Science Toolbox, you will need to go through several steps. But don't be afraid, each step is really easy to follow. We do suppose that you are using either MacOSX or Linux.

We will provide soon a script allowing to fully configure an Ubuntu [openstack](https://www.openstack.org) image with the RNA Science toolbox.

## Prerequisites

### Python environment

You need at first to have a Python distribution installed on your computer. If you don't have one, we recommend you a distribution like [Anaconda](https://www.continuum.io/why-anaconda).

### Fabric

You also need the tool [Fabric](http://www.fabfile.org). If you're using the [Anaconda distribution](https://www.continuum.io/why-anaconda), you can get it by typing:

    conda install fabric

### Docker

To install the RNA algorithms, you need first to get the tool Docker Community Edition. You will find all the details [here](https://docs.docker.com/engine/installation/).

## RNA Science Toolbox dependencies

### Python libraries

Once done, download the RNA Science Toolbox and go into its directory. To install its Python dependencies, you can use either the package manager conda (from the [Anaconda distribution](https://www.continuum.io/why-anaconda)) or pip. To use conda, type:

    fab python

To use pip, type:

    fab python:manager=pip

### RNA algorithms

Each Docker image available contains several algorithms:

* [fjossinet/assemble2](https://hub.docker.com/r/fjossinet/assemble2/): provides [RNAVIEW](http://ndbserver.rutgers.edu/ndbmodule/services/download/rnaview.html), [Vienna RNA package](https://www.tbi.univie.ac.at/RNA/), [foldalign](http://rth.dk/resources/foldalign/), [LocARNA](http://rna.informatik.uni-freiburg.de/LocARNA/)
* [fjossinet/rnaseq](https://hub.docker.com/r/fjossinet/rnaseq/): provides [SAMtools](http://samtools.sourceforge.net), [Tophat2](https://ccb.jhu.edu/software/tophat/), [Bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml)

To install these images, you just have to type:

    fab docker

If you need more details about these images, check their Web page.

### How to configure your environment?

In the configuration file of your shell (.bashrc, .zshrc,...), add the following lines:

    export TOOLBOX=THE_PATH_TO_YOUR_RNA_SCIENCE_TOOLBOX
    export PYTHONPATH=$PYTHONPATH:$TOOLBOX
    export PATH=$PATH:$TOOLBOX/pyrna:$TOOLBOX/scripts/python:$PATH

Restart your shell and type:

    pyrna_tests.py

Your RNA Science Toolbox is fully configured if you get something like:

<pre>
Recovering entry 1EHZ from Protein Databank...

## 3D annotation ##

List of base-pairs computed with RNAVIEW:

edge1 edge2 orientation  pos1  pos2
0      (     )           c     1    72
1      (     )           c     2    71
2      (     )           c     3    70
[...]
</pre>

# Advanced installation

## Option #1: RNA 3D modeling

If you're interested in 3D modeling with the tool Assemble2, you will need to import, annotate and store RNA 3D fragments derived from PDB structures.

To do so, you will need first to install [MongoDB](https://www.mongodb.com/fr) on your computer. If you're using Ubuntu, just type:

    sudo apt install mongodb-server

With OSX, you can use [homebrew](https://brew.sh/index_fr.html) and then type:

    brew update ; brew install mongodb ; brew services start mongodb

Once MongoDB installed, you need to feed the database with a script provided with the RNA Science Toolbox:

    import_3Ds.py -annotate

## Option #2: Jupyter notebooks

The RNA Science Toolbox provides several interactive notebooks with code samples. To use them efficiently, you need [Jupyter](http://jupyter.org) installed on your computer. To do so, type:

    fab jupyter

To use pip, type:

    fab jupyter:manager=pip  

Then go in the directory notebooks and type:

    jupyter notebook

Click on a notebook and enjoy!!

## Option #3: IPython configuration

To automatically import the PyRNA API from the IPython REPL, go into the directory $HOME/.ipython/profile_default/startup. Create a file named load_config.py containing the following lines:

<pre>
from pyrna.db import *
from pyrna.features import *
from pyrna.computations import *
from pyrna.parsers import *
from pyrna.utils import *
</pre>

Start a new IPython session from the command-line and type directly, without any import:

<pre>
pdb = PDB()
tertiary_structures = parse_pdb(pdb.get_entry('1EHZ'))
for ts in tertiary_structures:
  print ts.rna.sequence
</pre>

## Option #4: deploy Web Services

The RNA Science Toolbox allows you to give access to some of its functionalities as Web Services. These services are made available through a Web server.

### Node.js

You need to have [node.js](https://nodejs.org/en/) installed on your computer.

### Install the Web server dependencies

From the directory of the RNA Science Toolbox, type:

    fab website

### Launch the Web server

You have to use the script server.py located in the pyrna folder.

Just type:

    server.py

Open your browser at [http://localhost:8080](http://localhost:8080)

You can use a custom hostname and/or port by typing:

    server.py -h your_hostname_OR_IP_adress -p your_port

Open your browser at http://your_hostname_OR_IP_adress:your_port
