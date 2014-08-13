#Installing spreads for ubuntu 14.04 from scratch

this documentation shows howto install spreads for ubuntu 14.04. It's heavily based on the following resources:

##sources:
http://spreads.readthedocs.org/en/latest/

https://github.com/LabExperimental-SIUA/ilt/wiki/Spreads-Installation---Configuration-Guide-%28Debian-Jessie-Sid%29

http://www.diybookscanner.org/forum/viewtopic.php?f=35&t=3033


starting from a clean 14.04 system

```bash
sudo apt-get upgrade	
```


now install all dependcies

```bash
sudo apt-get install python2.7 python2.7-dev python-virtualenv python-pip libusb-dev\
libjpeg-dev libtiff5-dev libqt4-core ruby ruby-dev ruby-rmagick libmagickwand-dev\
 ruby-hpricot scantailor lua5.2 git 

```

next install pdfbeads

```bash
sudo gem install pdfbeads
```

next install latest djvubind (for me was 1.2.1)

```bash
wget http://djvubind.googlecode.com/files/djvubind_1.2.1.deb
sudo dpkg -i djvubind_1.2.1.deb
sudo apt-get -f install
sudo dpkg -i djvubind_1.2.1.deb
```

Download chdkptp. For me it was v650, from https://www.assembla.com/spaces/chdkptp/documents
unzip to /usr/local/lib/chdkptp

```bash
sudo unzip chdkptp-r650-Linux-x86_64.zip -d /usr/local/lib/chdkptp
```

install latest canvas draw, mine was 5.8
```bash
wget http://downloads.sourceforge.net/project/canvasdraw/5.8/Linux%20Libraries/cd-5.8_Linux35_64_lib.tar.gz
sudo tar zxf cd-5.8_Linux35_64_lib.tar.gz -C /usr/local/lib/chdkptp
```

next install IUP - portable user interface in the latest version, mine was 3.11
```bash
wget http://downloads.sourceforge.net/project/iup/3.11/Linux%20Libraries/iup-3.11_Linux35_64_lib.tar.gz
sudo tar zxf iup-3.11_Linux35_64_lib.tar.gz -C /usr/local/lib/chdkptp
```

finally install spreads

```bash
sudo pip install git+git://github.com/DIYBookScanner/spreads.git
```

install dependancies for spreads
```bash
sudo apt-get install python-psutil libffi-dev  python-usb
```

```bash
sudo pip install cffi
sudo pip install jpegtran-cffi

```

```bash
sudo apt-get install python-usb
sudo pip install --upgrade --pre pyusb
```

enable spreads GUI packages

```bash
sudo apt-get install python-pyside
sudo ln -s /usr/lib/python2.7/dist-packages/PySide /usr/local/lib/python2.7/site-packages/PySide
```

add current user to staff group

```bash
sudo adduser username staff (username must be replaced by the current username)
```