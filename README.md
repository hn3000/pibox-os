# PiBox OS

[Download the latest version](https://github.com/kubesail/pibox-os/releases)

This repository contains scripts and kernel modules used to modify Raspberry Pi OS in order to take full advantage of the PiBox hardware.

## Setup Script

Install this setup script

```bash
curl -s https://raw.githubusercontent.com/kubesail/pibox-os/main/setup.sh | sudo bash
```

Then run

```bash
# sets up KubeSail and installs your SSH keys
sudo kubesail

# run the following to update SSH and Kubernetes certs
sudo touch /boot/refresh-ssh-certs
sudo touch /boot/refresh-k3s-certs
sudo reboot now
```

**About this script**

This script installs two services which run at boot. These are lightweight services and add no overhead to the boot time if nothing needs to be done.

- `kubesail-init.service`
  - Verifies that the KubeSail agent is installed (for your KubeSail user) after K3s has started.
- `pibox-first-boot.service`
  - Install SSH keys from GitHub public keys if `/boot/github-username.txt` contains a GitHub username
  - Refresh SSH host certs
  - Refresh K3s certs

## PWM Fan Support

To make the fan quiet and only spin as fast as necessary, we install a service that sends the correct signal to the fan using the Pi's hardware PWM controller. See the [pwm-fan](pwm-fan) directory for details.

## LCD display

The python code used to render stats to the LCD display lives in the [lcd-display](lcd-display) directory. More info can be found on the PiBox docs: https://docs.kubesail.com/guides/pibox/os/#enabling-the-13-lcd-display

## Building ISO for release

The following commands builds the pibox OS image

    vagrant up
    vagrant ssh
    sudo -E packer build packer.json
