#### Először létrehozunk egy fájlt, amely megakadályozza, hogy a Linux betöltse a DVB-T dongle szabványos illesztőprogramjait (más néven modulokat)
    cat <<EOF > ~/no-rtl.conf
    blacklist dvb_usb_rtl2832u
    blacklist rtl2832
    blacklist rtl2830
    EOF

#### Ezután áthelyezzük az újonnan létrehozott fájlt a kívánt könyvtárba
    sudo mv ~/no-rtl.conf /etc/modprobe.d/

#### Ezután telepítünk néhány függőséget az RTL SDR felépítéséhez
    sudo apt-get install -y cmake libusb-1.0-0-dev build-essential
    sudo apt install build-essential cmake libusb-1.0-0-dev pkg-config

#### Git telepítése
    sudo apt install git

#### Most klónozzuk az RTL SDR-t egy új, sdr nevű könyvtárba a saját könyvtárunk alatt
    mkdir ~/sdr
    cd ~/sdr
    git clone git://git.osmocom.org/rtl-sdr.git
    cd ~/sdr/rtl-sdr

#### És felépítjük az RTL SDR-t
    mkdir build
    cd build
    cmake ../ -DINSTALL_UDEV_RULES=ON   

#### És telepítsd
    sudo make install
    sudo ldconfig
    sudo cp ~/sdr/rtl-sdr/rtl-sdr.rules /etc/udev/rules.d/ 

#### Most újra kell indítanunk az Orange Pi Zerót
    reboot

#### Győződjön meg arról, hogy a DVB-T dongle csatlakoztatva van az Orange Pi Zero USB-portjához
     rtl_test

#### A Ctrl + C billentyűkombinációval kiléphet az rtl_test programból

#### A dump1090-nek pkg-config szükséges , ezért először azt telepítjük
     sudo apt-get install -y pkg-config

#### Most már klónozhatjuk és lefordíthatjuk a dump1090 forráskódját
     cd ~/sdr
     git clone https://github.com/antirez/dump1090.git
     cd ~/sdr/dump1090
     make  

#### A dump1090-et úgy indíthatod el, hogy átváltasz a telepítési könyvtárra, és a következőképpen futtatod
     cd ~/sdr/dump1090
     ./dump1090  --interactive --aggressive --net  

#### És ha megnyitsz egy webböngészőt a http:// Orange Pi Zero IP-címére : 8080, akkor ugyanazon adatok térképes ábrázolását kell látnod

#### Rendszerindításkor elindítsa a dump1090-et
     sudo apt-get install screen

#### Most váltsunk a dump1090 telepítési könyvtárára
     cd ~/sdr/dump1090/

#### Hozz létre egy run.sh nevű szövegfájlt
     #!/bin/bash
     cd "${HOME}/sdr/dump1090/"
     ./dump1090 --net --interactive --aggressive     

#### Most állítsd be a jogosultságokat, hogy a fájl végrehajtható legyen
     chmod u+x ~/sdr/dump1090/run.sh

#### Ezután szerkeszd a crontab fájlodat
     crontab -e

#### hogy a következő sort is belefoglaljuk
     @reboot screen -d -m -S "dump1090 at boot" ${HOME}/sdr/dump1090/run.sh
     
