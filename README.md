# kioskPi
Configure a Raspberry Pi to become a kiosk.  

Using a Raspberry Pi 3 Model B and running NOOBs to install Raspian OS

## Install Chromium Web Browser

```
$ wget https://dl.dropboxusercontent.com/u/87113035/chromium-browser-l10n_45.0.2454.85-0ubuntu0.15.04.1.1181_all.deb
$ wget https://dl.dropboxusercontent.com/u/87113035/chromium-browser_45.0.2454.85-0ubuntu0.15.04.1.1181_armhf.deb
$ wget https://dl.dropboxusercontent.com/u/87113035/chromium-codecs-ffmpeg-extra_45.0.2454.85-0ubuntu0.15.04.1.1181_armhf.deb
$ sudo dpkg -i chromium-codecs-ffmpeg-extra_45.0.2454.85-0ubuntu0.15.04.1.1181_armhf.deb
$ sudo dpkg -i chromium-browser-l10n_45.0.2454.85-0ubuntu0.15.04.1.1181_all.deb chromium-browser_45.0.2454.85-0ubuntu0.15.04.1.1181_armhf.deb
```

Ref http://raspberrypi.stackexchange.com/questions/41603/installing-chrome-on-raspbian


## Set up Chromium
Go to `Settings > On start-up` and select `Open a specific page or set of pages` and add your URLs to the list.

If using multiple pages, use a tab rotator extension which needs to be configurated to run at start up, for example `TabCarousel` https://chrome.google.com/webstore/detail/tabcarousel/ddldimidiliclngjipajmjjiakhbcohn?hl=en and adjust the timings for the rotation as necessary.


## Install MS Core Fonts
Make web pages look prettier

```
$ sudo apt-get install ttf-mscorefonts-installer
```

## Disable ScreenSaver
Disable text terminals from blanking by change two settings in `/etc/kbd/config`

```
BLANK_TIME=0
POWERDOWN_TIME=0
```

Disable Xsession from blanking by adding these lines to `/etc/xdg/lxsession/LXDE/autostart` and commenting out the xscreensaver option

```
#@xscreensaver -no-splash
@xset s noblank 
@xset s off 
@xset -dpms
```

Ref: https://www.raspberrypi.org/forums/viewtopic.php?t=57552


## To make the Chromium auto-start
You'll need a startup script.  Create the following file in the given location: -
```
$ sudo nano ~/.config/autostart/autoChromium.desktop
```

Add the following: -

```
[Desktop Entry]
Type=Application
Exec=/usr/bin/chromium-browser --noerrdialogs --disable-session-crashed-bubble --disable-infobars --kiosk
Hidden=false
X-GNOME-Autostart-enabled=true
Name[en_US]=AutoChromium
Name=AutoChromium
Comment=Start Chromium when GNOME starts
```

Ref: http://raspberrypi.stackexchange.com/questions/38515/auto-start-chromium-on-raspbian-jessie-11-2015

## To put into portrait mode

Add the following at the bottom of the file `/boot/config.txt`

```
display_rotate=3
```
Ref: https://www.raspberrypi.org/forums/viewtopic.php?f=108&t=120793

## Hide mouse cursor

Install unclutter

```
$ sudo apt-get install unclutter
```




## Enable SSH
Use SSH to remotely connect to the Pi if needed.

Enter `sudo raspi-config` in the terminal, then navigate to ssh, hit Enter and select Enable or Disable ssh server.

Connect using `pi@192.168.xx.xxx` and the default password is `raspberry`, obviously putting in your own details.


If testing you are setup and wish to launch chromium remotely, don't forget the command `export DISPLAY=:0`

