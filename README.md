# Setup Keys Pi

The TFT touch screen i was using was a piece of garbage that the manufacturer doesn't even support anymore. This is why I am now writing a second version of this setup giude that uses the latest kernel and rpios version.


### Install stuff

```
wget https://warmplace.ru/soft/sunvox/sunvox-2.1.2.zip
unzip sunvox-2.1.2.zip
sudo apt install jackd a2jmidid
```


### Edit `/boot/firmware/config.txt`

```
# uncomment this
dtparam=i2s=on

# comment this
#dtparam=audio=on

# edit this line to add the audio=off part
# https://github.com/raspberrypi/linux/issues/2489
dtoverlay=vc4-kms-v3d,audio=off
dtoverlay=hifiberry-dac
```


### JACK Config

start qjackctl and open it (needs a graphical UI environment)

~/.config/rncbc.org/QjackCtl.conf

```
[Settings]
...
Driver=alsa
Frames=256
...
SampleRate=44100
```

### Sunvox settings



https://warmplace.ru/soft/sunvox/sunvox_config.ini

```
audiodriver jack
buffer 512
fullscreen
```


## The Old Settings

I wrote these originally when i was trying to use that keidei screen. Some things changed (VC4 fkms driver, namely)

keys2.local

starting with raspbian buster (not latest)

keys
uwus

flash drive to transfer files
- connect to network
- sunvox
- drivers for screen
- any setup script



wget https://warmplace.ru/soft/sunvox/sunvox-2.1.1c.zip
unzip sunvox-2.1.1c.zip

# set up driver https://blog.himbeer.me/2018/12/27/how-to-connect-a-pcm5102-i2s-dac-to-your-raspberry-pi/

sudo nano /boot/config.txt

# uncomment
#dtparam=i2s=on

# comment this line
dtparam=audio=on

# add this line
dtoverlay=hifiberry-dac

sudo apt install jackd a2jmidid

# edit asound.conf

sudo nano /etc/asound.conf

pcm.!default  {
 type hw card 0
}
ctl.!default {
 type hw card 0
}


reboot


# enable the routing of alsa midi to jack2
a2j_control --ehw && a2j_control --start

# route the midi to sunvox
qjackctl


# stgartup?

start_jack.sh

#!/bin/sh

jack_control start
jack_control ds alsa
jack_control dps device hw:HD2
jack_control dps rate 44100 
jack_control dps nperiods 2
jack_control dps period 64
sleep 10
a2j_control --ehw
a2j_control --start
sleep 10
qjackctl &
