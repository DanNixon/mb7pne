# Software deployment

## Setup

- `ansible-galaxy install -r requirements.yml`

## Alpine install: prepare USB stick

Using [alpine-raspberry-pi-install](https://github.com/DanNixon/alpine-raspberry-pi-install).

- `./make_boot_media.sh ...`
- `echo "dtparam=audio=on" | sudo tee boot/usercfg.txt`
- `./umount_boot_media.sh`

## Alpine install: configuration

- Put USB stick in Pi and boot
- `mkdir /media/sdX2`
- `mount /dev/sdX2 /media/sdX2/`
- `setup-alpine`
  - (follow instructions for obvious things)
  - (do not configure WiFi)
  - (use `https://dan-nixon.com/ssh_pubkey.txt` as SSH key URL)
  - (ensure `sdX2` is set for APK cache and LBU)
- `apk add python3`
- `lbu include /root/.ssh/authorized_keys`
- `lbu commit`

## Deployment via Ansible

- `ansible-playbook main.yml`
- `tailscale up --accept-dns=false --advertise-tags=tag:radio`
- `lbu include /var/lib/tailscale`
- `lbu commit`
