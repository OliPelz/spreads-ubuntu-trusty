#Installing spreads for ubuntu 14.04 from scratch

this documentation shows howto install spreads for ubuntu 14.04. It's heavily based on the following resources:

the exact spreads git revision i am using here is

https://github.com/DIYBookScanner/spreads/commit/7c0fe30311c36735fc12efcb006d885f4e6138e4

##sources:
http://spreads.readthedocs.org/en/latest/

https://github.com/LabExperimental-SIUA/ilt/wiki/Spreads-Installation---Configuration-Guide-%28Debian-Jessie-Sid%29

http://www.diybookscanner.org/forum/viewtopic.php?f=35&t=3033


starting from a clean 14.04 system

```bash
sudo apt-get update
sudo apt-get upgrade	
```


now install all dependcies

```bash
sudo apt-get install python2.7 python2.7-dev python-virtualenv python-pip libusb-dev \
libjpeg-dev libtiff5-dev libqt4-core ruby ruby-dev ruby-rmagick libmagickwand-dev \
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

Next install latest chdkptp. Go to website, for me it was version v650 (chdkptp-r650-Linux-x86_64.zip), from https://www.assembla.com/spaces/chdkptp/documents
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
add the /usr/local/lib/chdkptp path to the systems dynamic library search path

create and open a new file
```bash
sudo nano /etc/ld.so.conf.d/spreads.conf
```
add the line
```bash
/usr/local/lib/chdkptp/
```
reload the system-wide libraries paths
```bash
sudo ldconfig
```

finally install spreads (ignore all warnings) in an virtualenv

```bash
virtualenv ~/.spreads
source ~/.spreads/bin/activate
```

```bash
sudo pip install git+git://github.com/DIYBookScanner/spreads.git
```

install dependancies for spreads (ignore warnings)
```bash
sudo apt-get install python-psutil libffi-dev  python-usb
```

```bash
sudo pip install pycparser
sudo pip install cffi
sudo pip install jpegtran-cffi

```
update for python usb
```bash
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

now add the lua env variable to the global path in order that the chdkptp command will work
open the file 
```bash
sudo vi ~/.bashrc 
```
add
```bash
export CHDKPTP_DIR=/usr/local/lib/chdkptp
export LUA_PATH="$CHDKPTP_DIR/lua/?.lua"
source ~/.spreads/bin/activate
```

open a new shell or type 
```bash
source ~/.bashrc
```

now run the spreads configuration program

```sh
spread configure
```
in the program set

* step 1 select : chdkcamera
* step 2 select : scantailor, tesseract, gui, autorotate
* step 3 select : select order - autorotate,scantailor,tesseract 
* step 4 and 5 select : no for both camera questions (target_page and focus)

this will save the config as config.yaml.

now its time to configure the cameras

Before rerunning the spreads config program again first make sure if you can use the 
chdkptp program.

First before using chdkptp in Ubuntu 14.04 you have to kill gphoto in order to gain access to the chdk-enabled cameras otherwise you will not be able to use them
```bash
ps aux | grep gphoto
kill -9 <PID of gphoto2 process>
```

I use the Canon A2200, enable CHDK in your camera 
(e.g. chdk.wikia.com/wiki/CHDK_1.2.0_User_Manual - section Using CHKD), plug in the micro USB after 
enabling (I use firmware update method) than plug in the other USB end into your computer, than run the
chdkptp program to test if it is installed correctly.

```bash
/usr/local/lib/chdkptp/chdkptp
```
now click on Connect button.

If it is running without problems end the program.

Now restart the spread configure program again, enable CHDK in your camera, than connect to the computer
 (if not done already) and configure target_page and focus. Should give no errors if camera and CHDK has been 
setup correctly.
