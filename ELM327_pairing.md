# Instructions how to pair ELM327 clone in Linux

## Setup

Ubuntu 20.04, Thinkpad 400s

## Steps

Assuming bluetooth adapter is `hci0`.

1. Add yourself to `dialout` group, because you need an access to serial port later, `sudo usermod -a -G dialout <user>`. *Logout and login*!

1. Enable legacy pairing mode. For more info see [this](https://stackoverflow.com/questions/12888589/linux-command-line-howto-accept-pairing-for-bluetooth-device-without-pin).
```
hciconfig hci0 sspmode 1
hciconfig hci0 sspmode
hci0:   Type: BR/EDR  Bus: USB
BD Address: AA:BB:CC:DD:EE:FF  ACL MTU: 1021:8  SCO MTU: 64:1
Simple Pairing mode: Enabled
```

1. Check bluetooth device ID. This can be found with `bluetoothctl` too.
```
hciconfig hci0 piscan
sdptool add SP
hcitool scan
    00:11:22:33:44:55    My_Device
```

1. Connect manually to device. If you want to use `rfcomm` as normal user, change permissions: `sudo chmod u+s /usr/bin/rfcomm`.
```
sudo rfcomm connect /dev/rfcomm0 00:11:22:33:44:55 1
Connected /dev/rfcomm0 to 00:11:22:33:44:55 on channel 1
Press CTRL-C for hangup
```

1. (optional) in case you are not in `dialout` group, allow access to all users See [this](https://stackoverflow.com/questions/53375587/serial-serialutil-serialexception-could-not-open-port-dev-ttyama0-errno-13]).
```
sudo chmod 666 /dev/rfcomm0
```

Now `obd` Python lib should be able to connect.