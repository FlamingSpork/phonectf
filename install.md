 this is left sparse and mostly serves as a reminder for me of how I did this and proof that I can (sometimes) figure things out
 setting this up will require significant Linux knowledge and probably some SIP/Asterisk know-how
 
 ```bash
 wget https://downloads.asterisk.org/pub/telephony/asterisk/asterisk-18-current.tar.gz
 sudo apt-get install git curl wget libnewt-dev libssl-dev libncurses5-dev subversion libsqlite3-dev build-essential libjansson-dev libxml2-dev uuid-dev
 tar xvf asterisk-18-current.tar.gz
 rm asterisk-18-current.tar.gz
 cd asterisk-18.10.0/
 contrib/scripts/get_mp3_source.sh
 sudo contrib/scripts/install_prereq install
 ./configure
 make menuselect
 ```
 
 overwrite `main/dsp.c` with the one from this repo
 
 make and install it
```bash
make
sudo make install
sudo make samples

sudo groupadd asterisk
sudo useradd -r -d /var/lib/asterisk -g asterisk asterisk
sudo usermod -aG audio,dialout asterisk
sudo chown -R asterisk.asterisk /etc/asterisk
sudo chown -R asterisk.asterisk /var/{lib,log,spool}/asterisk
sudo chown -R asterisk.asterisk /usr/lib/asterisk
sudo chmod -R 750 /var/{lib,log,run,spool}/asterisk /usr/lib/asterisk /etc/asterisk
sudo vim /etc/asterisk/asterisk.conf # set user to asterisk
sudo systemctl restart asterisk
```

installing `asterisk-perl`
```bash
wget https://github.com/asterisk-perl/asterisk-perl/archive/refs/tags/v1.06.tar.gz
tar xvf v1.06.tar.gz
cd asterisk-perl-1.06/
perl Makefile.PL
make all
sudo make install
```

then install configs and you should be good to go
