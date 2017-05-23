
Halef Setup Process
===================

How to setup Kaldi Recognizer and Test Recognition Model

How to setup Cairo Speech Server

How to setup JvoiceXML Voice Browser Application




How to setup Kaldi Recognizer and Test Recognition Model
--------------------------------------------------------

1. Make sure that you have latest Ubuntu updates

*sudo apt-get update*

*sudo apt-get dist-upgrade*

*sudo apt-get upgrade*



2. Install libraries required for Kaldi recognizer

*sudo apt-get install zlib1g-dev*

*sudo apt-get install libatlas3-base*

*sudo apt-get install libwebsockets-dev*

*sudo apt-get install libssl-dev*


3. Create Kaldi directory, clone custom Kaldi repository and run build script

*mkdir -p /export/Apps/KALDI/*

*cd /export/Apps/KALDI/*

*git clone git://git.code.sf.net/p/halef/cassandra halef-cassandra*

*cd /export/Apps/KALDI/halef-cassandra*

*bash ./BUILD_KALDI.sh*


4. Copy recognition models and binaries to destination directories

*mkdir -p /export/Apps/KALDI/2015-12-21/deployment/bin*

*cd /export/Apps/KALDI/2015-12-21/deployment*

*cp /export/Apps/KALDI/2015-12-21/halef-cassandra/kaldi-trunk/src/online2bin/STRM-ASR-server /export/Apps/KALDI/2015-12-21/deployment/bin*

*chown -R <your_login>:<your_group> /export/Apps/KALDI/2015-12-21*

5. Copy recognition models and startup scripts 

*cd /export/Apps/KALDI/2015-12-21/deployment/*

*tar xzvf models.tar.gz*

*tar xzvf 9404.tar.gz*

6. Start Kaldi instance

*cd /export/Apps/KALDI/2015-12-21/deployment/srv/9404*

*./server.sh &*


How to setup Cairo speech server
--------------------------------

To setup Cairo instances a convenient setup script has been created.

1. Download setup script and Kaldi settings files by opening the following links and clicking "Download this file" link on top of each page.

https://sourceforge.net/p/halef/halef-cairo/ci/master/tree/cairo-SETUP/SETUP.sh

https://sourceforge.net/p/halef/halef-cairo/ci/master/tree/cairo-SETUP/SETUP.kaldi.properties

2. Copy downloaded SETUP.* files into /export/Apps directory on the server and edit them. You will see direcory settings and database connection settings in the top part of SETUP.sh file which you need to correct. SETUP.kaldi.properties should contain at least 1 entry defining item name, server ip and port on which Kaldi is listening. For example: *default=localhost:9404*

3. Make sure that you have ~2GB of free space in /export/Apps and server can connect to the Internet for Cairo download.

4. To install Cairo with Java, please run setup in unattended mode:

*./SETUP.sh --quiet*

Cairo installation can take considerable time depending on your Internet connection speed.

5. You can install and run Cairo as service by running CairoService script (please review CairoService file content first to check if all directories are correct there):

*cd /export/Apps/Cairo/cairo-VM/scripts*

*sudo CairoService install*

*sudo service cairo start*



How to setup JvoiceXML Voice Browser Application
------------------------------------------------

1. For JvoiceXML we recommend using latest version directly from JvoiceXML developer repository (https://github.com/JVoiceXML/JVoiceXML) :

*mkdirs -p /export/Apps/JVXML*

*cd /export/Apps/JVXML*

*git clone https://github.com/JVoiceXML/JVoiceXML.git*







