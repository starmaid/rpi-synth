keys2.local

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

# enable the routing of alsa midi to jack2
a2j_control --ehw && a2j_control --start

# route the midi to sunvox
qjackctl


