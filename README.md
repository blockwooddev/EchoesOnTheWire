# Echoes On The Wire

This is a project for the WAKE Art show at Ship In The Woods by Kyle Stewart and Barry Lockwood. We modified an old rotary phone to record ghostly messages from the other side and play them back to adventurous listeners. In this repo you can find code used in the project and tips for setting it up. Thank you for your interest! If you are going to use the code or ideas herein, please give us a mention, noncommercial use only.

## Summary

In 2017, our creative coding group, San Diego Code Kitchen, was invited to participate in a show at the Ship In The Woods gallery entitled WAKE. The theme of the show was death and the afterlife. For our project, we retrofitted an old rotary phone modern electronics to allow recording and playback of audio on the phone. Historic moments captured in audio could be dialed up by the participant using the rotary input, and dialing '0' allowed someone to record a message that would be played back with added effects for future listeners. 

An embedded linux computer provided the ability to record and playback audio. Interfacing with the 1960s technology was easier than expected and we were able to not only read the state of the handset but also the exact number dialed. Python was used to capture this input and send it onto Pure Data via OSC. From there PD was in charge of audio generation, capturing, and effects. It was chosen for its ability to run on embedded linux and the quick build time it provided us. The phone has so much more potential that we hope to build upon with future ideas.

## Themes

Through this project we hoped to explore humanity's relation with technology and our own and that technology's mortality. Human life and our human interactions are ephemeral, and the technology that enables them outlives them, but is often meaningless without them. The rotary telephone (expecially older telephones) is a splendid example of this. We will never hear the conversations that a telephone has been privy to. So, in Echoes on the Wire we hoped to simulate the experience of hearing all those possible conversations, people's dreams and aspirations that have simply vanished. 

Of course, it's also just a fun, spooky project, so we added some sound effects to give people some fun interactions.

## Setting up:

Below you can find setup instructions, and documentation to the pd patch with audio examples is here: [pd readme](./patch/README.md)

## Connecting via USB OTG
(only when you need to get the wifi working)

    $ ls /dev/tty*
    $ screen /dev/tty.usbmodem1423 115200

## Get on wifi

    $ nmcli device wifi list
    $ sudo nmcli device wifi connect 'Young Hickory WiFi' password 'hotcoffee' ifname wlan0
    sudo nmcli device wifi connect 'Young Hickory WiFi' password 'hotcoffee' ifname wlan0
    $ nmcli device status
    $ sudo ifconfig

## SSH-ing into the device
(if on the same wifi)

    $ ssh chip@chip1.local


## installing Python
Make sure you are on python 3.4 or greater


## installing GPIO library
https://github.com/xtacocorex/CHIP_IO

sudo apt-get update
sudo apt-get install git build-essential python-dev python-pip flex bison chip-dt-overlays -y
sudo pip install CHIP-IO

## Installing OSC library
https://github.com/attwad/python-osc

sudo pip3 install python-osc


## Running final build

sudo /usr/bin/python /home/chip/spookyphone/hardware-io/phone_in.py && /usr/bin/pd -nogui -rt /home/chip/spookyphone/patch/main.pd


## Updating systemd service for PHONE IO

sudo cp /home/chip/spookyphone/phoneio.service /etc/systemd/system/phoneio.service

sudo systemctl daemon-reload
sudo systemctl enable phoneio.service
sudo systemctl start phoneio
systemctl status phoneio

## Stopping
sudo systemctl stop phoneio


## Updating systemd service for PATCH

sudo cp /home/chip/spookyphone/spookyphone.service /etc/systemd/system/spookyphone.service

sudo systemctl daemon-reload
sudo systemctl enable spookyphone.service
sudo systemctl start spookyphone
systemctl status spookyphone


## Stopping
sudo systemctl stop spookyphone
