# **Anbox on Debian 11 «Bullseye»**

This guide applies only to Debian 11 (codename "bullseye"). In Debian 12, Anbox has been removed from the official repositories. To install Anbox on Debian 12, it is recommended to use Snap.

## **Install Anbox**

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

## **Install Apps on Anbox**

First you need install _adb_ (Android Debug Bridge) software. 

    sudo apt-get update && sudo apt-get install adb

Download the APK to install in your Anbox, from [here](https://www.apkmirror.com/), and then install with ADB Tools; replace `emulator-555` with your device ID.

Check the connected devices.

    adb devices

Install the packed to the target device.

    adb -s emulator-5558 install grit.storytel.app_apkmirror.com.apk


## **«Self-Promotion»**

If you wish, you can visit my YouTube channel [MizaqScreencasts](https://www.youtube.com/MizaqScreencasts) and follow me on [Twitter](https://twitter.com/mismatso).

## **License ![by-nc-sa](https://licensebuttons.net/l/by-nc-sa/4.0/80x15.png)**

This manual is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en). This means you are free to:

- **Share** — copy and redistribute the material in any medium or format.
- **Adapt** — remix, transform, and build upon the material, but **not for commercial purposes**.

Under the following conditions:

- **Attribution** — you must give appropriate credit, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.
- **NonCommercial** — you may not use the material for commercial purposes.
- **ShareAlike** — if you remix, transform, or build upon the material, you must distribute your contributions under the same license as the original.

---

[Anbox On Debian](https://github.com/mismatso/anboxondebian) © 2022 by [Misael Matamoros](https://t.me/mismatso) is licensed under [CC BY-NC-SA 4](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en).








