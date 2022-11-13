# bcftools-docker
## Bcftools build done with v1.16.1

In this case, the conda install does not work -- instead, this Dockerfile gets the release.

[Bcftools GitHub](http://samtools.github.io/bcftools/)

Steps to build this docker container.
1. Determine the dependencies by iterative running of .configure
2. Use ubuntu base image
3. get each of the dependencies
4. run the steps specified for [building and installing](http://www.htslib.org/download/)

To edit the files:
1. Use a simple editor (Text Edit on Mac is fine) - we want to be cautious about unseen characters
2. Use emacs (can be installed with conda)
```bash
conda install -c conda forge emacs
```

To build your image from the command line:
* Can do this on [Google shell](https://shell.cloud.google.com) - docker is installed and available

```bash
docker build -t bcftools .
```

To test this tool from the command line 

Set up an environment variable capturing your current command line:
```bash
PWD=$(pwd)
```

Then mount and use your current directory and call the tool now encapsulated within the environment.

Any bcftools command can be used to check.

```bash
docker run -it -v $PWD:$PWD -w $PWD bcftools bcftools fastq
```

## (Optional) Deposit your container in the your [CAVATICA](cavatica.sbgenomics.com)  Docker Registry

If you are working with Kids First or INCLUDE data, and you are registered with either the [Kids First DRC](https://kidsfirstdrc.org/) or the [INCLUDE Data Hub](https://includedcc.org/), you have access to a platform as a service, CAVATICA by Seven Bridges.

If you do, you Docker Image Repository Specific to you is at the location of pgc-images.sbgenomics.com/[YOUR CAVATICA USERNAME]

There are three steps to building and using a container

1. build
2. tag
3. push

We have built and we did tag this (I built the above on my desktop which is a mac, but it could have been built within a google shell.  Examples of how to do this are found in the [Elements of Style in Workflow Creation and Maintenance, Day 3](https://github.com/NIH-NICHD/Kids-First-Elements-of-Style-Workflow-Creation-Maintenance/blob/main/classes/Building-A-Nextflow-Script/README.md#preamble-to-building-workflows-using-containers)

### Tag

To tag the image just built, you need the image id, to get that simply use the command **`docker images`**.

```bash
docker images
REPOSITORY                                           TAG       IMAGE ID       CREATED          SIZE
rmats                                                latest    0ca8aaf01be0   16 minutes ago   1.53GB
```

Now we re-tag it for pushing to our own personal [CAVATICA](cavatica.sbgenomics.com) docker container registry.

```bash
docker tag 0ca8aaf01be0 pgc-images.sbgenomics.com/[YOUR CAVATICA USERID]/rmats:v4.1.2
```

### Docker registry login

There is actually another step required before you can do your push to the registry.  You need to authenticate.

Navigate to the CAVATICA Developers Tab.

Select Authentication Token, if you have not done so, generate that token.

Then Copy the token and paste it in the proper location in the command below (after -p for password)

```bash
docker login pgc-images.sbgenomics.com -u [YOUR CAVATICA USERNAME] -p [YOUR AUTHENTICATION TOKEN]
```

### Push

Now that we have

* tagged :whitecheck our docker image and

* we have authenticated :whitecheck 

```bash
docker push pgc-images.sbgenomics.com/[YOUR CAVATICA USERNAME]/bcftools:v1.16.1
```

You know things are going correctly when you see something to the effect of:


```bash
The push refers to repository [pgc-images.sbgenomics.com/deslattesmaysa2/bcftools:v1.16.1]
ea25457229f1: Pushed 
17280cc0fa6b: Pushed 
ab2731ec3f53: Pushed 
6fa1f4185aa2: Pushed 
ad6562704f37: Pushed 
latest: digest: sha256:3b1976baa7c4aaa2afd25098c41c754e2579060b6c1da32282c45ac8a10293a9 size: 1373
```

## (Optional) Using bcftools outside of a container

If you don't have the container pulled and you want to install it on your MacBook Pro,
which is what I do to test out the steps before putting them in a container.

***Rule of thumb -- test test test***

So that means - test the install before you put something in a container and make it
available for others to use.   Then test that container.

`bcftools` and `samtools` are staples of biology and have been around a long time and a recent publication

Please cite this paper when using BCFtools for your publications:

Twelve years of SAMtools and BCFtools
Petr Danecek, James K Bonfield, Jennifer Liddle, John Marshall, Valeriu Ohan, Martin O Pollard, Andrew Whitwham, Thomas Keane, Shane A McCarthy, Robert M Davies, Heng Li
GigaScience, Volume 10, Issue 2, February 2021, giab008, https://doi.org/10.1093/gigascience/giab008


@article{10.1093/gigascience/giab008,
    author = {Danecek, Petr and Bonfield, James K and Liddle, Jennifer and Marshall, John and Ohan, Valeriu and Pollard, Martin O and Whitwham, Andrew and Keane, Thomas and McCarthy, Shane A and Davies, Robert M and Li, Heng},
    title = "{Twelve years of SAMtools and BCFtools}",
    journal = {GigaScience},
    volume = {10},
    number = {2},
    year = {2021},
    month = {02},
    abstract = "{SAMtools and BCFtools are widely used programs for processing and analysing high-throughput sequencing data. They include tools for file format conversion and manipulation, sorting, querying, statistics, variant calling, and effect analysis amongst other methods.The first version appeared online 12 years ago and has been maintained and further developed ever since, with many new features and improvements added over the years. The SAMtools and BCFtools packages represent a unique collection of tools that have been used in numerous other software projects and countless genomic pipelines.Both SAMtools and BCFtools are freely available on GitHub under the permissive MIT licence, free for both non-commercial and commercial use. Both packages have been installed \\&gt;1 million times via Bioconda. The source code and documentation are available from https://www.htslib.org.}",
    issn = {2047-217X},
    doi = {10.1093/gigascience/giab008},
    url = {https://doi.org/10.1093/gigascience/giab008},
    note = {giab008},
    eprint = {https://academic.oup.com/gigascience/article-pdf/10/2/giab008/36332246/giab008.pdf},
}

