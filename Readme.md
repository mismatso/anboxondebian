# Anbox on Debian

## Install Anbox

Retrieve Anbox from Debian repositories.

    sudo apt-get update && sudo apt-get install anbox

Download an Android image for your Anbox.

    wget https://build.anbox.io/android-images/2018/07/19/android_amd64.img

Move the file to the Anbox images folder.

    sudo mv android_amd64.img /var/lib/anbox/android.img

Create the next directories to override the default Anbox service.

    sudo mkdir -pv /etc/systemd/system/anbox-container-manager.service.d /var/lib/anbox/rootfs-overlay/system/etc/

Create the _media_codecs.xml_ file and copy this [text](./media_codecs.xml "Media codecs").

    touch /var/lib/anbox/rootfs-overlay/system/etc/media_codecs.xml

Create the file _override.conf_.

    sudo tee /etc/systemd/system/anbox-container-manager.service.d/override.conf<< EOF
    [Service]
    ExecStart=
    ExecStart=/usr/bin/anbox container-manager --daemon --privileged --data-path=/var/lib/anbox --use-rootfs-overlay
    EOF

Restart the Anbox service.

    sudo service anbox-container-manager stop

    sudo systemctl daemon-reload

    sudo service anbox-container-manager start

Then launch Anbox using the following command.

    anbox launch --package=org.anbox.appmgr --component=org.anbox.appmgr.AppViewActivity

## Install Apps on Anbox

First you need install _adb_ (Android Debug Bridge) software. 

    sudo apt-get update && sudo apt-get install adb

Download the APK to install in your Anbox, from [here](https://www.apkmirror.com/), and then install with ADB Tools; replace `emulator-555` with your device ID.

Check the connected devices.

    adb devices

Install the packed to the target device.

    adb -s emulator-5558 install grit.storytel.app_apkmirror.com.apk











