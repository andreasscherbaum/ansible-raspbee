# ansible-raspbee

Install a RaspBee II on a Raspberry Pi


## Description

This Ansible Role will install the Raspbee RTC kernel module and the software for a [Dresden Elektronik](https://www.dresden-elektronik.de/) [RaspBee II Zigbee Gateway](https://www.dresden-elektronik.de/produkt/raspbee-II.html) on a Raspberry Pi running Raspbian.


## Installation

### Directly into the Roles directory

Clone the repository into the "roles/" directory in your Playbook roles directory:

```
cd roles && git clone https://github.com/andreasscherbaum/ansible-raspbee.git raspbee && cd -
```


### Symlink into the Roles directory

```
cd ..
git clone https://github.com/andreasscherbaum/ansible-raspbee.git
cd -
cd roles
ln -s ../../ansible-raspbee raspbee
cd -
```


## Configuration

The Role uses variables to configure if the source code should be deleted after building the module, and if the compiled code should be removed. Removing the source will refresh the source after the next kernel update. Removing the build code removes any debugging information in case something fails.

Change the defaults in _vars/main.yml_:

* raspbee_remove_source: True/False, remove source after building the kernel module
* raspbee_remove_build: True/False, remove build after building the kernel module


## Usage

Use the role name ("raspbee") in your Playbook:

```
  roles:
    - raspbee
```


## Important notes:

This Playbook will reboot the Raspberry Pi when the kernel module is installed. Seems to be necessary, otherwise the module can't be loaded.

The error message in _/var/log/syslog_ is:

```
May 26 23:20:15 rasbpee systemd[1]: rtc-pcf85063.service: Service RestartSec=15s expired, scheduling restart.
May 26 23:20:15 rasbpee systemd[1]: rtc-pcf85063.service: Scheduled restart job, restart counter is at 39.
May 26 23:20:15 rasbpee systemd[1]: Stopped Enable RTC-PCF85063.
May 26 23:20:15 rasbpee systemd[1]: Started Enable RTC-PCF85063.
May 26 23:20:15 rasbpee bash[2449]: /bin/bash: line 0: echo: write error: Invalid argument
May 26 23:20:15 rasbpee kernel: [  996.219934] i2c i2c-1: Failed to register i2c client pcf85063 at 0x51 (-16)
May 26 23:20:15 rasbpee systemd[1]: rtc-pcf85063.service: Main process exited, code=exited, status=1/FAILURE
May 26 23:20:15 rasbpee systemd[1]: rtc-pcf85063.service: Failed with result 'exit-code'.
May 26 23:20:15 rasbpee systemd[1]: Configuration file /lib/systemd/system/rtc-pcf85063.service is marked executable. Please remove executable permission bits. Proceeding anyway.
May 26 23:20:15 rasbpee systemd[1]: Configuration file /lib/systemd/system/rtc-pcf85063.service is marked world-inaccessible. This has no effect as configuration data is accessible via APIs without restrictions. Proceeding anyway.
May 26 23:20:15 rasbpee systemd[1]: Reloading.
```

If you have an idea how to load the module without above error, please provide a PR, or open an Issue.


## Documentation

* https://phoscon.de/de/raspbee2/install#raspbian
* http://phoscon.de/app

