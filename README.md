# Ansible playbook for Raspberry Pi

## Overview

Target: Raspberry Pi 3B+, Raspbian Lite ( I'm using `Raspbian Lite (A port of Debian with no desktop environment))
This ansible playbook support you run [Rosetta@home](https://boinc.bakerlab.org/rosetta/) with BOINC client on Raspberry Pi 3B+.

## How to use

1. Setting up your Raspberry Pi 3B+ as you can connect with SSH.

1. get the 64bit kernel

    WARNING

    As <https://www.raspberrypi.org/forums/viewtopic.php?t=25073> says "This is for bleeding edge testers. Things may not work. Backing up, or using a fresh sdcard is a wise precaution."

    execute below command on Raspberry Pi.

    ```shell
    sudo rpi-update
    ```

    then reboot OS.

    ```shell
    sudo reboot
    ```

1. Clone git repository

    ```shell
    git clone https://github.com/revsystem/raspberry-ansible.git
    cd raspberry-ansible/
    ```

1. Setting up for Ansible playbook

    Check your Raspberry Pi 3B+ IP address and write down to ssh.config like below.

    ```shell
    vi ssh.conf
    ```

    ```text
    Host boinc-1
    HostName 192.168.1.45
    ```

    This ansible playbook set the hostname based on `Host` in ssh.conf.
    If you want to set another host name, please edit `Host` section.

    ---

    SSH user account name is in group_vars/all.yml.

    ```yaml
    ansible_ssh_user: pi
    ```

1. Setting up for BOINC.

    ```shell
    ansible-playbook boinc.yml -i hosts --diff
    ```

    then reboot OS.

    ```shell
    sudo reboot
    ```

1. Create your Rosetta@home account.

    Access to [Rosetta@hone](https://boinc.bakerlab.org/rosetta/) and create your account if you don't have it.

1. Get you account key.

    ```shell
    boinccmd --lookup_account https://boinc.bakerlab.org/rosetta/ <your_email> <your_password>
    ```

    then you see the below message. copy the account key string.

    ```shell
    oll status: operation in progress
    poll status: operation in progress
    account key: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```

    If you get `authentication error`, move to /etc/boinc-client and execute boinccmd and `sudo service boinc-client restart`.

1. Join to the "Rosetta@home"

    ```shell
    boinccmd --project_attach https://boinc.bakerlab.org/rosetta/ <your_account_key>
    ```

### GUI like interface

You may be comfortable using the `boinctui`.

```shell
boinctui
```

<img src="https://github.com/revsystem/raspberry-ansible/blob/master/images/boinctui.png" width="640" alt="boinctui" />

Using `F9` key, you could access the menu bar. select the 'Projects', you can see 'Rosetta@home'. In this menu, you can suspend / resume / update the project same as Windows/MacOS BOINC Client manager.

## References

- [Fold for Covid - Donate spare compute capacity for COVID-19 research](https://foldforcovid.io/)
- [Raspberry PiでRosetta@homeに参加する - Qiita](https://qiita.com/izewfktvy533zjmn/items/0d520a6d1ec381bd65a2)
- [Pi4 64-bit raspbian kernel for testing - Focus on Pi4 - Raspberry Pi Forums](https://www.raspberrypi.org/forums/viewtopic.php?t=250730)
- [Running Rosetta (COVID-19 workunits) on Raspberry Pi 3B+ (how to guide) : BOINC](https://www.reddit.com/r/BOINC/comments/g0r0wa/running_rosetta_covid19_workunits_on_raspberry_pi/)
- [RaspberryPiで新型コロナウイルスの解析 - ディーズガレージ wiki](http://dz.plala.jp/wiki/index.php?title=RaspberryPi%E3%81%A7%E6%96%B0%E5%9E%8B%E3%82%B3%E3%83%AD%E3%83%8A%E3%82%A6%E3%82%A4%E3%83%AB%E3%82%B9%E3%81%AE%E8%A7%A3%E6%9E%90&mobileaction=toggle_view_desktop)
- [Raspberry Pi 3 Model BとRaspbian Buster LiteでRosetta@home - Pastebin.com](https://pastebin.com/jCqJDp7N)
