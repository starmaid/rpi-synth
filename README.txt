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
