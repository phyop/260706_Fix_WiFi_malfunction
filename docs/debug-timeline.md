# Debug Timeline

## 1. Initial Symptom

Several Android apps failed only on one Wi-Fi network. The same apps worked on mobile data and other Wi-Fi networks.

## 2. Network Boundary

The phone appeared connected, but app behavior suggested the network path was not reliable for all destinations or app types.

## 3. DNS and IPv6

Public DNS could resolve the relevant domains, but the automatic network configuration remained suspicious because the network also supplied IPv6 DNS.

## 4. VPN and Web Protection

A VPN/web-protection warning appeared during testing. It was useful to check, but it was not the final root cause because the final fix continued to work after VPN and web protection were re-enabled.

## 5. IoT Difference

The Panasonic IoT app behaved differently from exchange apps, likely because IoT apps may depend on local network discovery, background permissions, or stable LAN routing.

## 6. IP Conflict

The phone had previously been configured with the same IP as the router:

```text
Phone IP: 192.168.1.1
Gateway: 192.168.1.1
```

This was invalid because the phone and router cannot share the same LAN IP.

## 7. Final Fix

The phone was assigned a valid static IPv4 configuration with a unique address, correct gateway, and public DNS.

## 8. Validation

After applying the fix:

- affected apps loaded normally
- VPN and web protection were turned back on
- the phone was rebooted
- the fix persisted

