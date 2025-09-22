## Először létrehozunk egy fájlt, amely megakadályozza, hogy a Linux betöltse a DVB-T dongle szabványos illesztőprogramjait (más néven modulokat)
    cat <<EOF > ~/no-rtl.conf
    blacklist dvb_usb_rtl2832u
    blacklist rtl2832
    blacklist rtl2830
    EOF
