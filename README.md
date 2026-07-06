# Android Wi-Fi Debug Case Study

This repository documents a real Android network troubleshooting case: several apps failed only on one Wi-Fi network, while the same phone and apps worked on mobile data and other networks.

The practical fix was a valid static IPv4 configuration with a correct gateway and public DNS.

![Case overview](medium-assets/01-case-overview.png)

## Problem

Three unrelated Android apps showed network errors only on one home Wi-Fi network:

- a crypto exchange app
- another exchange app
- a Panasonic IoT app

The phone was connected to Wi-Fi and signal strength looked normal, but app behavior was inconsistent.

## Key Fix

The stable configuration was:

```text
Proxy: None
IP settings: Static
IP address: 192.168.1.150
Gateway: 192.168.1.1
Network prefix length: 24
DNS 1: 1.1.1.1
DNS 2: 8.8.8.8
```

After applying this setup, the affected apps worked. VPN and web protection were re-enabled, the phone was rebooted, and the fix persisted.

## Why This Was Interesting

The issue was not solved by reinstalling apps. The failure boundary pointed to the network path:

- worked on mobile data
- worked on other Wi-Fi networks
- failed only on one Wi-Fi
- affected unrelated apps
- IoT behavior differed from ordinary outbound app traffic

## Debugging Path

![Troubleshooting path](medium-assets/03-troubleshooting-path.png)

The investigation checked:

- DNS behavior
- IPv6 DNS and routing
- VPN and web-protection interception
- app permissions
- IoT local network behavior
- static IP conflict

The most concrete configuration error found was an IP conflict: the phone had previously been assigned the same IP as the router.

![IP conflict versus fixed path](medium-assets/02-ip-conflict-vs-fixed.png)

## Medium Article

The article version is kept in:

- [medium-android-wifi-debug-article.md](medium-android-wifi-debug-article.md)

## Privacy Notes

This repository intentionally does not include:

- Wi-Fi QR codes
- full MAC addresses
- Wi-Fi passwords
- personal account identifiers
- exact device identifiers
- private screenshots

All diagrams are sanitized reconstructions.

## Disclaimer

This is a personal troubleshooting case study. Network environments vary, and static IP settings should be chosen carefully to avoid IP conflicts.

