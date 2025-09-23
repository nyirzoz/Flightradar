## Orange Pi Zero2 - Armbian OS

     sudo apt-get update
     sudo apt-get upgrade
     sudo reboot

####  Installation Dump1090-FA
     sudo bash -c "$(wget -O - https://raw.githubusercontent.com/abcd567a/piaware-ubuntu20-amd64/master/install-dump1090-fa.sh)"
#### IP:8080 elérhető
####
    reboot

#### Installation FR24
    sudo sh -c 'test -e /etc/apt/sources.list || printf "# placeholder for FR24 installer\n" > /etc/apt/sources.list'

#### 
    sudo bash -c "$(wget -O - http://repo.feed.flightradar24.com/install_fr24_rpi.sh)"

####  http://<IP of OrangePi>:8754/settings.html elérhető
#### A dump 1090 már kezeli a DVB stick-et, így itt már a avr-tcp-t kell kiválasztani


    

<img width="815" height="472" alt="Képkivágás" src="https://github.com/user-attachments/assets/3c7a9b13-691b-4236-8574-b8bd2282210f" />

#### Fr24 automatikus indítás
    sudo systemctl enable fr24feed

#### Fr24 státusz
    sudo systemctl status fr24feed

