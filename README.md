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
docker run -it -v $PWD:$PWD -w $PWD bcftools bcftools -h 
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

So we run:
```bash
docker images
```

and we see:

```bash
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
bcftools      latest    4acd7249c0f6   2 hours ago   566MB
star          latest    264b10d079ae   2 days ago    125MB
trimmomatic   latest    633fec609229   5 days ago    790MB
samtools      latest    cb5d97224fcd   5 days ago    576MB
```

Now we re-tag it for pushing to our own personal [CAVATICA](cavatica.sbgenomics.com) docker container registry.  In my case I am using my CAVATICA USERID **`deslattesmaysa2`**

```bash
docker tag 4acd7249c0f6 pgc-images.sbgenomics.com/[YOUR CAVATICA USERID]/bcftools:1.16
```

To see what has happened run the command **`docker images`** again and we see:

```bash
docker images
```

returns:

```bash
REPOSITORY                                           TAG       IMAGE ID       CREATED       SIZE
bcftools                                             latest    4acd7249c0f6   2 hours ago   566MB
pgc-images.sbgenomics.com/deslattesmaysa2/bcftools   1.16      4acd7249c0f6   2 hours ago   566MB
star                                                 latest    264b10d079ae   2 days ago    125MB
trimmomatic                                          latest    633fec609229   5 days ago    790MB
samtools                                             latest    cb5d97224fcd   5 days ago    576MB
```

### Docker registry login

There is actually another step required before you can do your push to the registry.  You need to authenticate.

Navigate to the CAVATICA Developers Tab.

Select Authentication Token, if you have not done so, generate that token.

Then Copy the token and paste it in the proper location in the command below (after -p for password)

For security, in stead of passing the **`AUTHENTICATION`** token on the command line.  

Provided it when prompted for your **`Password`** as shown below:

```bash
docker login pgc-images.sbgenomics.com -u deslattesmaysa2
Password: 
Login Succeeded
```


### Push

Now that we have

* tagged :whitecheck our docker image and

* we have authenticated :whitecheck 

```bash
docker push pgc-images.sbgenomics.com/[YOUR CAVATICA NAME/bcftools:v1.16
```

You know things are going correctly when you see something to the effect of:


```bash
The push refers to repository [pgc-images.sbgenomics.com/deslattesmaysa2/bcftools:v1.16]
ea25457229f1: Pushed 
17280cc0fa6b: Pushed 
ab2731ec3f53: Pushed 
6fa1f4185aa2: Pushed 
ad6562704f37: Pushed 
latest: digest: sha256:3b1976baa7c4aaa2afd25098c41c754e2579060b6c1da32282c45ac8a10293a9 size: 1373
```

### (optional) Build and Tag in one step

An alternative to building and tagging in two steps (which allows testing of the image before commitment to the repository -- but all things can be undone if there is an error - you can delete an image from the repository 

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


## CAVATICA documentation

Please refer to [CAVATICA documentation](https://docs.cavatica.org/docs/upload-your-docker-image) for further details on creating, tagging and uploading your Docker images 
