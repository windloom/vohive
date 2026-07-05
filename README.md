# VoHive

VoHive binary distribution and deployment repository maintained for private/lab use.

This repository provides:

- Linux binary installation script for systemd and OpenWrt/procd systems
- Release binaries for `linux/amd64` and `linux/arm64`
- Docker Hub multi-architecture image usage instructions

## Disclaimer

VoHive is intended only for personal technical testing and internal lab use.
Do not use it for commercial, illegal, abusive, or unauthorized telecom/network activity.
You are responsible for complying with local laws, carrier terms, and hardware/SIM usage rules.

## Supported Platforms

Recommended systems:

- Debian / Ubuntu
- Raspberry Pi OS / Debian on ARM64
- NAS or other Linux hosts
- OpenWrt systems with enough storage and USB modem driver support

Supported release binary architectures:

- `linux/amd64`
- `linux/arm64`

## Host Requirements

Install the basic modem tools and drivers for your host distribution.

Debian / Ubuntu:

```sh
sudo apt-get update
sudo apt-get install -y socat usbutils iproute2 pciutils
```

OpenWrt package names vary by build, but these are commonly needed:

```sh
opkg update
opkg install socat usbutils ip-full \
  kmod-usb-serial kmod-usb-serial-option \
  kmod-usb-wdm kmod-usb-net-qmi-wwan
```

VoHive needs direct access to modem devices such as:

```text
/dev/ttyUSB*
/dev/cdc-wdm0
wwan0
```

If another service is managing the same modem, stop it first. In particular, do not let
`ModemManager`, `uqmi`, `netifd`, or other QMI dialers manage the same modem at the same time.

## Install From GitHub Releases

Install latest release:

```sh
curl -fsSL https://raw.githubusercontent.com/windloom/vohive/main/install.sh | sh
```

Or with `wget`:

```sh
wget -O - https://raw.githubusercontent.com/windloom/vohive/main/install.sh | sh
```

Install a specific version:

```sh
curl -fsSL https://raw.githubusercontent.com/windloom/vohive/main/install.sh | sh -s -- --version v1.5.5
```

Install only the binary and config files, without registering systemd/procd service:

```sh
curl -fsSL https://raw.githubusercontent.com/windloom/vohive/main/install.sh | sh -s -- --no-systemd
```

Default paths:

```text
/opt/vohive/bin/vohive
/opt/vohive/config/config.yaml
/opt/vohive/data
/opt/vohive/logs
```

Default web access:

```text
http://HOST_IP:7575
admin / admin
```

Change the password after first login.

## Docker Hub Image

Multi-architecture Docker image:

```text
skyhotspur/vohive:1.5.5
skyhotspur/vohive:latest
```

Supported Docker platforms:

```text
linux/amd64
linux/arm64
```

Pull:

```sh
docker pull skyhotspur/vohive:1.5.5
```

Run with Docker:

```sh
docker run -d \
  --name vohive \
  --restart unless-stopped \
  --network host \
  --privileged \
  -e TZ=Asia/Singapore \
  -e CONFIG_PATH=/app/config/config.yaml \
  -v /opt/vohive/config:/app/config \
  -v /opt/vohive/data:/app/data \
  -v /opt/vohive/logs:/app/logs \
  -v /dev:/dev \
  skyhotspur/vohive:1.5.5
```

Docker Compose:

```yaml
services:
  vohive:
    image: skyhotspur/vohive:1.5.5
    container_name: vohive
    restart: unless-stopped
    network_mode: host
    privileged: true
    volumes:
      - /opt/vohive/config:/app/config
      - /opt/vohive/data:/app/data
      - /opt/vohive/logs:/app/logs
      - /dev:/dev
    environment:
      - TZ=Asia/Singapore
      - CONFIG_PATH=/app/config/config.yaml
```

Docker deployment intentionally uses `network_mode: host`, `privileged: true`, and `/dev`
passthrough because VoHive directly manages modem control devices.

## Release Assets

The `v1.5.5` GitHub Release contains:

```text
vohive_v1.5.5_linux_amd64
vohive_v1.5.5_linux_arm64
```

The install script selects the correct asset based on `uname -m`.

## Common Commands

Systemd:

```sh
systemctl status vohive
journalctl -u vohive -f
systemctl restart vohive
```

OpenWrt/procd:

```sh
/etc/init.d/vohive status
logread -f
/etc/init.d/vohive restart
```
