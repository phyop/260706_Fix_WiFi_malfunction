# Final Fix

The final working Wi-Fi configuration was:

```text
Proxy: None
IP settings: Static
IP address: 192.168.1.150
Gateway: 192.168.1.1
Network prefix length: 24
DNS 1: 1.1.1.1
DNS 2: 8.8.8.8
```

## Why It Worked

This setup gave the phone:

- a valid unique LAN IP
- the correct router gateway
- known public IPv4 DNS resolvers
- no proxy
- a more deterministic IPv4 path

## Caution

Static IP settings should be chosen carefully. Do not assign the phone the same IP as the router or another device.

