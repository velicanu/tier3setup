tier3setup
==========
  This is a guide to set up vanilla Ubuntu 14.04 to run some of the basic software needed for LHC data analysis. Some parts are specific to the CMS experiment but most will be universal.
  
<h4>Installing Root:</h4> 

At the time of writing I was able to build root 6.00.02 from source by doing the following:

```bash
## First install prerequisites ##
sudo apt-get install git dpkg-dev make g++ gcc binutils libx11-dev libxpm-dev libxft-dev libxext-dev
##  Next pull the root source and build it
wget http://root.cern.ch/download/root_v6.00.02.source.tar.gz .
tar -xzvf root_v6.00.02.source.tar.gz
cd root
./configure
## this part will take a while, you should see "ROOT BUILD SUCCESSFUL." at the end, otherwise troubleshoot
make
## next add the following to your .bashrc or equivalent
source /path/to/root/bin/thisroot.sh
## Test that it's working
root
```

<h4>Installing xrootd:</h4> 

In addition to root, xrootd might be useful to access data stored on the computing grid at various datacenters around the world. Fortunatley the root source comes with a xrootd setup script.

```bash
## install xrootd, this is for version 4.0.4, but you can specify some other
cd /path/to/root
build/unix/installXrootd.sh -v 4.0.4
## add the following to your .bashrc or equivalent, note the version number
source bin/setxrd.sh /path/to/root/xrootd-4.0.4/
```

<h4>Certificates:</h4>



