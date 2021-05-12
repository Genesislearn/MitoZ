Installation of MitoZ

Guanliang Meng, 2019-01-24

********************************************************************

We provide three options to use MitoZ:
* A ready to run version of [Singularity container](https://www.sylabs.io/) [![https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg](https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg)](https://singularity-hub.org/collections/1955)

* A ready to run version of [Docker container](https://hub.docker.com/r/guanliangmeng/mitoz)

* Source code with dependency to be installed with Anaconda or Miniconda.

If you install MitoZ from source code or Singularity version, verify whether your taxonomy database is valid!!!!

    $ python3
    >>> from ete3 import NCBITaxa
    >>> a = NCBITaxa()
    >>> a.get_name_translator(["Arthropoda"])
    {'Arthropoda': [6656]}

If error occurs, then your taxonomy database is invalid, which could be due to there is not enough space in your HOME directory. The solution is already in MitoZ issues.

# 1 Singularity container

## 1.1 Install Singularity
See [https://www.sylabs.io/docs/](https://www.sylabs.io/docs/) for instructions to install Singularity.

Note: according to the offical documention (Oct. 2019), the Singularity must be installed with root privilege.

And the Singularity installed via conda (e.g. `conda install -c bioconda singularity`) may not work (at least when installing as normal users)!

## 1.2 Download the MitoZ container

    $ singularity pull  --name MitoZ.simg shub://linzhi2013/MitoZ:v2.3

For China users, it can be difficult to pull the Singularity container from https://www.singularity-hub.org/
for known network problem, you can download the containers from https://pan.genomics.cn/ucdisk/s/uMFvum.

## 1.3 Run the MitoZ container

    $ /path/to/MitoZ.simg --help

## 1.4 You can also `shell` into the container

In the host, run

    $ singularity shell /path/to/MitoZ.simg

In the container, run

    $ /app/anaconda/bin/python3 /app/release_MitoZ_v2.3/MitoZ.py -h

some useful scripts are in `/app/release_MitoZ_v2.3/useful_scripts`

    $ ls -lhrt /app/release_MitoZ_v2.3/useful_scripts

To learn more about how to use Singularity, please refer to https://www.sylabs.io/docs/.


# 2. Docker container

## 2.1 Install Docker

Please refer to https://docs.docker.com/.

## 2.2 Download the MitoZ container

    $ docker pull guanliangmeng/mitoz:2.3

## 2.3 Run the container

In your working directory (i.e. `$PWD`) (the fastq files should be in there),
shell into the container:

    $ sudo docker run -v $PWD:$PWD -w $PWD --rm guanliangmeng/mitoz:2.3 /app/release_MitoZ_v2.3/MitoZ.py


## 2.4 You can also `shell` into  the container

In your **host** working directory (i.e. `$PWD`) (**the fastq files should be in there**),
shell into the container:

    $ sudo docker run -v $PWD:$PWD -w $PWD --rm -it guanliangmeng/mitoz:2.3

The `-v $PWD:/project` here means mounting your current host directory into `/project` of the Docker container. Multiple `-v` options can be used at the same time.

Say, if we do `-v /opt/project1/data:/project`, we can see the files of the host's `/opt/project1/data` directory from the container, but we won't be able to access any other files (or soft-links or maybe hard-links) outside the `data` directory.

In the container,

    $ python3 /app/release_MitoZ_v2.3/MitoZ.py

some useful scripts are in `/app/release_MitoZ_v2.3/useful_scripts`

    $ ls -lhrt /app/release_MitoZ_v2.3/useful_scripts


To learn more about the Docker usage, please go to https://docs.docker.com/.


# 3. Udocker container

**Before you decide install MitoZ from source code, you should try Udocker first!!**

**Unlike, Singuarlity and Docker, you don't need root/sudo previlege to install or/and run Udocker!!**

## 3.1 Install Udocker

    $ mkdir /my_soft/udocker/
    $ cd /my_soft/udocker/
    $ curl https://raw.githubusercontent.com/indigo-dc/udocker/master/udocker.py > udocker
    $ chmod u+rx ./udocker
    $ ./udocker install

You can add the `udocker` command to your `PATH` envrionmental variable:

    $ echo 'export PATH="/my_soft/udocker/:$PATH"' >>~/.bashrc
    $ source ~/.bashrc

Keep in mind that Udocker install dependencies (and images) into your `~/.udocker/` direcotry. If your `HOME` directory has limited space,
you can move this directory to other place, then use `ln -s` command to link it back to your `HOME` directory.

Go to https://github.com/indigo-dc/udocker for more details.

## 3.2 Download the MitoZ container

    $ udocker pull guanliangmeng/mitoz:2.3
    $ udocker images
      REPOSITORY
      guanliangmeng/mitoz:2.3 

## 3.3 Run the container

In your working directory (i.e. `$PWD`) (the fastq files should be in there),
shell into the container:

    $ udocker run --volume=$PWD --workdir=$PWD  --rm guanliangmeng/mitoz:2.3 /app/release_MitoZ_v2.3/MitoZ.py


There is an example on https://github.com/linzhi2013/MitoZ/tree/master/test


# 4. Install from source code

**Warning: this way of installation can be depressing and time-wasting.**

**If you run into troubles with MitoZ installed from source code, it is probably because of a broken `mitozEnv` environment.** See https://github.com/linzhi2013/MitoZ/issues/84 and https://github.com/linzhi2013/MitoZ/issues/80#issuecomment-690376705 as examples. **If similar things happen to you, you'd better re-run the analysis with the Docker version BEFORE raise a new issue.**

**Strongly recommend to use the Docker/Udocker/Singularity version instead!! You're a researcher focusing on biological questions, and surely you don't want to waste your time on  software installation.**



## 4.1 System requirment: Linux

developed under: CentOS release 6.9 (Final), 2.6.32-696.30.1.el6.x86_64

## 4.2 Install Anaconda or Miniconda
1. Anaconda: https://anaconda.org/anaconda/python
2. Miniconda: https://conda.io/miniconda.html (recommended)


## 4.3 Install dependency with `conda` command

Thanks to @mptrsen (see https://github.com/linzhi2013/MitoZ/issues/47#issuecomment-581938066), the quickest way to create a `mitozEnv` environment is:
    
    # find the mitozEnv.yaml file after you download the source codes, see section 3.6 below
    $ conda env create -n mitozEnv -f mitozEnv.yaml

If this method works, you can jump to **section 3.4** directly.

### 4.3.1 Set up channels

    $ conda config --add channels defaults
    $ conda config --add channels bioconda
    $ conda config --add channels conda-forge

### tips
The download speeds from the above channels can be very slow, if this is the case to you,
you can set up other mirror channels, but make sure the `conda-forge` channel has the highest
priority (the last one to be added), followed by `bioconda` channel.

For example, in China, you can set:

    $ conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
    $ conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/

Or,

    $ conda config --add channels http://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/
    $ conda config --add channels http://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/

but I cannot ensure using mirror channels will always work. Good luck!

If the custom mirror channels do not work, you may need to remove them firstly:

    # show all your channels
    $ conda config --show channels

    # remove the the mirror channles
    $ conda config --remove  channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
    $ conda config --remove  channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
    
    # If you have other mirrors, please also remove them.
    
Make sure now you have only these three channels:

    $ conda config --show channels
    channels:
      - conda-forge
      - bioconda
      - defaults
      # or 
    channels:
      - conda-forge/label/cf201901
      - bioconda/label/cf201901
      - defaults
     
### 4.3.2 Set up an isolated enviroment for MitoZ

**The versions MATTER!!! Please just copy the command below to create your mitozEnv environment, other versions may not work.**


It is a good idea to install MitoZ into an isolated enviroment, e.g., `mitozEnv`.

    $ conda create  -n mitozEnv libgd=2.2.4 python=3.6.0 biopython=1.69 ete3=3.0.0b35 perl-list-moreutils perl-params-validate perl-clone circos=0.69 perl-bioperl blast=2.2.31  hmmer=3.1b2  bwa=0.7.12 samtools=1.3.1 infernal=1.1.1 tbl2asn openjdk
    $ conda update -c bioconda tbl2asn ete3
 
If the above commands fail, try to run:

    $ conda clean -a -y
    
firstly, then run the above create and update commands again.

## 3.4 Activate the `mitozEnv` environment

    $ source activate mitozEnv

## 4.5 Install NCBI taxonomy database for ete3 package
1. Network connection required.
2. `HOME` directory must have more than 500M space available. If not, please refer to `https://github.com/linzhi2013/taxonomy_ranks/blob/master/README.md`

In the terminal, type `python3` then `Enter`, you will be into the Python interactive interface. Now run the following commands line-by-line in the Python interactive interface.

    from ete3 import NCBITaxa
    ncbi = NCBITaxa()
    ncbi.update_taxonomy_database()

For more details, please refer to http://etetoolkit.org/docs/latest/tutorial/tutorial_ncbitaxonomy.html

**Notice**
If you have difficulties in downloading the database from NCBI, please forward to https://github.com/linzhi2013/MitoZ/issues/72#issuecomment-666215301.

**Recently, the NCBI taxonomy database (Sep 2020) seems to have some problems (e.g. `sqlite3.IntegrityError: UNIQUE constraint failed: synonym.spname, synonym.taxid`)**, see https://github.com/linzhi2013/MitoZ/issues/81#issue-699246501 and https://github.com/linzhi2013/MitoZ/issues/79#issuecomment-689582348. If this happens to you, you should refer to https://github.com/linzhi2013/MitoZ/issues/72#issuecomment-666215301 first.


**Now verify whether the NCBI Taxonomy database is valid**

```
$ python
>>> from ete3 import NCBITaxa
>>> a = NCBITaxa()
>>> a.get_name_translator(["Arthropoda"])
{'Arthropoda': [6656]}
```

If there's error for you, then your NCBI taxonomy databse is broken and you should reinstall it. **This issue has been asked several times in the Github issue page and there are solutions for this. But others met the same problem wouldn't even check through these issues and just raise a new issue. I'm tired of repeating the same answers now, so I WILL NOT reply to any new issues on this.**


## 4.6 Download the MitoZ source codes

Go to `https://github.com/linzhi2013/MitoZ/tree/master/version_2.3` and download the file `release_MitoZ_v2.3.tar.bz2` (https://raw.githubusercontent.com/linzhi2013/MitoZ/master/version_2.3/release_MitoZ_v2.3.tar.bz2). You can put it to anywhere.

    $ tar -jxvf release_MitoZ_v2.3.tar.bz2
    $ cd release_MitoZ_v2.3
    $ python3 MitoZ.py

## 4.7 Important: make sure you are in the `mitozEnv` environment when you run MitoZ!
If you write the run commands into a script file (e.g. `work.sh`), you should also add `source activate mitozEnv` into the
script file ahead of the MitoZ commands.

For example, a file named `work.sh` with following content:

    source activate mitozEnv
    python /path/to/MitoZ.py all2 --help

Then you can run this script in your terminal as:

    $ sh work.sh

# 5. Run a test
It is wise to check everything is fine before you run your own samples. Here is a small example https://github.com/linzhi2013/MitoZ/tree/master/test, it only takes 4 CPUs, about 2 G memory and 15 minutes to get finished.

********************************************************************
