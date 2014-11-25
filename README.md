tier3setup
==========
  This is a guide to set up vanilla Ubuntu 14.04 to run some of the basic software needed for LHC data analysis. Some parts are specific to the CMS experiment but most will be universal.
  
<h3>Installing Root:</h3> 

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

