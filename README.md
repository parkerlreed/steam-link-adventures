# steam-link-adventures

Using this to chronical what all I have done with my Steam Link.

## On a USB drive (FAT32)

Create `/steamlink/config/system/enable_ssh.txt` (Just create the file) Enables SSH after power cycling Link.

## USB headsets/DACs work and show up in pactl (pulseaudio)

````
# pactl list sinks
Sink #1
        State: RUNNING
        Name: alsa_output.0.analog-stereo
        Description: Logitech G930 Headset Analog Stereo
        Driver: module-alsa-card.c
        Sample Specification: s16le 2ch 48000Hz
        Channel Map: front-left,front-right
        Owner Module: 17
        Mute: no
        Volume: front-left: 65536 / 100% / 0.00 dB,   front-right: 65536 / 100% / 0.00 dB
                balance 0.00
        Base Volume: 82505 / 126% / 6.00 dB
        Monitor Source: alsa_output.0.analog-stereo.monitor
        Latency: 9851 usec, configured 9977 usec
        Flags: HARDWARE HW_MUTE_CTRL HW_VOLUME_CTRL DECIBEL_VOLUME LATENCY
        Properties:
                alsa.resolution_bits = "16"
                device.api = "alsa"
                device.class = "sound"
                alsa.class = "generic"
                alsa.subclass = "generic-mix"
                alsa.name = "USB Audio"
                alsa.id = "USB Audio"
                alsa.subdevice = "0"
                alsa.subdevice_name = "subdevice #0"
                alsa.device = "0"
                alsa.card = "0"
                alsa.card_name = "Logitech G930 Headset"
                alsa.long_card_name = "Logitech Logitech G930 Headset at usb-f7ee0000.usb-1.2, full speed"
                alsa.driver_name = "snd_usb_audio"
                device.bus_path = "/devices/soc.0/f7ee0000.usb/usb1/1-1/1-1.2/1-1.2:1.0/sound/card0"
                sysfs.path = "/devices/soc.0/f7ee0000.usb/usb1/1-1/1-1.2/1-1.2:1.0/sound/card0"
                device.string = "front:0"
                device.buffering.buffer_size = "1760"
                device.buffering.fragment_size = "880"
                device.access_mode = "mmap"
                device.profile.name = "analog-stereo"
                device.profile.description = "Analog Stereo"
                device.description = "Logitech G930 Headset Analog Stereo"
                alsa.mixer_name = "USB Mixer"
                alsa.components = "USB046d:0a1f"
                module-udev-detect.discovered = "1"
                device.icon_name = "audio-card"
        Ports:
                analog-output: Analog Output (priority: 9900)
        Active Port: analog-output
        Formats:
                pcm
````

## Arch chroot running from USB drive

### On computer

* Download http://os.archlinuxarm.org/os/ArchLinuxARM-armv7-latest.tar.gz
* Format a flash drive with an ext3 partition
* Extract downloaded rootfs to said partition

### On Steam Link

* Power on
* Insert flash drive
* Over SSH
 * `mkdir /mnt/usb`
 * `mount /dev/block/sda1 /mnt/usb`
 * `mount -t proc proc /mnt/usb/proc/`
 * `mount -o bind /dev /mnt/usb/dev/`
 * `mount -t devpts devpts /mnt/usb/dev/pts/`
 * `mount -t sysfs sys /mnt/usb/sys/`
 * `chroot /mnt/usb /bin/bash`

## Use Steam Link and USB headphones/DACs to receive Bluetooth audio from Android

### On Steam Link

* `bluetoothctl`
* scan on (Put phone in discovery mode)
* pair \<discovered-mac\>
* trust \<discovered-mac\>
* connect \<discovered-mac\>
* exit

## Control the running pulseaudio instance from your computer

### On Steam Link

* `pactl load-module module-native-protocol-tcp auth-anonymous=1`

### On computer

* `PULSE_SERVER=<steam-link-ip> pavucontrol`

Same method will let you send audio from your computer to the Link (again only works with USB audio devices attached to the Link. HDMI doesn't show up as a device at all in pulse)

* `PULSE_SERVER=<steam-link-ip> some-program-that-uses-libpulse`

## Volume control from CLI

### On Steam Link

* `pactl set-sink-volume 0 90%` Or whatever value you want
* `pactl set-sink-mute 0 toggle` Toggle mute

# More to come...
