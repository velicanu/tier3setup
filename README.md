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
source /path/to/root/bin/setxrd.sh /path/to/root/xrootd-4.0.4/
```

<h4>Certificates:</h4>

Now all the necessary software is set up, we just need to properly authenticate. This part is a little tricky and will probably only work if you've been able to set up these certificates in the past on some other computing center. You first need to get a certificate from your institution (CERN for example), to do that you need to follow the instructions here (or elsewhere on google) https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBookStartingGrid#ObtainingCert. 

From this point on I'll assume you have userkey.pem and usercert.pem with the proper permissions in the ~/.globus directory.

```bash
## get some required utils
sudo apt-get install globus-proxy-utils
sudo apt-get install voms-clients
## add the following to your .bashrc or equivalent
export X509_CERT_DIR=/etc/grid-security/certificates/
## setup some default directories with voms info
sudo mkdir -p /etc/grid-security/vomsdir/cms
sudo mkdir -p /etc/vomses
wget http://linuxsoft.cern.ch/wlcg/sl6/x86_64/wlcg-voms-cms-1.0.0-1.noarch.rpm
rpm2cpio wlcg-voms-cms-1.0.0-1.noarch.rpm | cpio -i --make-directories
sudo mv etc/grid-security/vomsdir/cms/* /grid-security/vomsdir/cms
sudo mv etc/vomses/* /etc/vomses
## now we're ready to try voms init and grid init
grid-proxy-init
voms-proxy-init --voms cms
## if you've made it to this point then xrootd should be up and running, can test it with xrdcp by inserting a proper file path
xrdcp root://xrootd.cmsaf.mit.edu//store/user/username/filename.root . 
```
