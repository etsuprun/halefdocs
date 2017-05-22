
Halef Setup Process
===================

How to setup Kaldi Recognizer and Test Recognition Model

How to setup Cairo Speech Server

How to setup JvoiceXML Voice Browser Application




How to setup Kaldi Recognizer and Test Recognition Model
--------------------------------------------------------

1. Make sure that you have latest Ubuntu updates

sudo apt-get update

sudo apt-get dist-upgrade

sudo apt-get upgrade



2. Install libraries required for Kaldi recognizer

sudo apt-get install zlib1g-dev

sudo apt-get install libatlas3-base

sudo apt-get install libwebsockets-dev

sudo apt-get install libssl-dev


3. Create Kaldi directory, clone custom Kaldi repository and run build script

mkdir -p /export/Apps/KALDI/

cd /export/Apps/KALDI/

git clone git://git.code.sf.net/p/halef/cassandra halef-cassandra

cd /export/Apps/KALDI/halef-cassandra

bash ./BUILD_KALDI.sh


4. Copy recognition models and binaries to destination directories

mkdir -p /export/Apps/KALDI/2015-12-21/deployment/bin

cd /export/Apps/KALDI/2015-12-21/deployment

tar xzvf models.tar.gz

cp /export/Apps/KALDI/2015-12-21/halef-cassandra/kaldi-trunk/src/online2bin/STRM-ASR-server /export/Apps/KALDI/2015-12-21/deployment/bin

chown -R <your_login>:<your_group> /export/Apps/KALDI/2015-12-21

