# When Apps Fail Only on One Wi-Fi: Debugging Android, DNS, IPv6, VPN, and IoT Connectivity

![Case overview: one Wi-Fi caused several app failures, then a static IPv4 setup restored the path.](medium-assets/01-case-overview.png)

Three unrelated Android apps failed only on one home Wi-Fi network. The same phone worked on mobile data. The same apps worked on other networks. That pattern made the network path, not the apps, the most likely suspect.

The fix was to stop relying on the unstable automatic Wi-Fi configuration and give the phone a clean static IPv4 path:

```text
Phone IP: 192.168.1.150
Gateway: 192.168.1.1
Prefix: 24
DNS: 1.1.1.1 and 8.8.8.8
Proxy: None
```

After that change, OKX, Binance, and Panasonic IoT all worked. VPN and web protection were turned back on, the phone was rebooted, and the fix still held.

That is the short version. The rest of this article is the field-debugging path: how I narrowed the issue from "some apps say the network is broken" to a phone-side static IPv4 workaround that held up after reboot.

## The Debug Starts Here

The first useful clue was the failure boundary. This was not a global outage, and it was not a single broken app.

- The affected apps failed only on one Wi-Fi network.
- The same phone worked on mobile data.
- The same apps worked on other Wi-Fi networks.
- Basic Wi-Fi connection status still looked normal.

That pushed the investigation toward the local network configuration: DNS, IPv6, VPN interception, router behavior, or a bad IP assignment.

## The Critical Mistake: IP Conflict

![Router IP conflict: the phone cannot use the same IP address as the gateway.](medium-assets/02-ip-conflict-vs-fixed.png)

One incorrect configuration stood out immediately:

```text
Phone IP: 192.168.1.1
Gateway: 192.168.1.1
```

That cannot work reliably. In this network, `192.168.1.1` was the router. The phone cannot use the same address as the gateway.

The corrected version gave the phone its own unique address while keeping the router as the gateway:

```text
Phone IP: 192.168.1.150
Gateway: 192.168.1.1
Prefix: 24
```

This was the most concrete bug in the investigation. It also explained why the phone could look connected while apps still behaved unpredictably.

## Why DNS Alone Was Not Enough

Changing DNS to public resolvers helped clarify the problem, but it did not fully solve it by itself.

The network was also handing out ISP-provided IPv6 DNS servers. Public DNS could resolve the exchange domains, but the phone's automatic configuration path still behaved inconsistently. That made the issue broader than "bad DNS."

The eventual static IPv4 setup worked because it controlled more than one variable at the same time:

- valid phone IP
- correct gateway
- known IPv4 DNS resolvers
- no proxy
- less dependence on the unstable automatic path

## VPN Was a Clue, Not the Root Cause

![Troubleshooting path: narrow the failure domain from symptoms to DNS, VPN, IoT behavior, and final static IPv4 fix.](medium-assets/03-troubleshooting-path.png)

During the investigation, Android showed a VPN/web-protection warning from a mobile security app. That was worth checking because Android VPN profiles can intercept or block traffic when the tunnel is unhealthy.

But VPN was not the final root cause. After the static IPv4 fix, VPN and web protection could be enabled again, and the apps still worked after reboot.

That distinction mattered: the VPN warning was a useful clue, but the stable fix came from correcting the Wi-Fi network path.

## Why Panasonic IoT Behaved Differently

OKX and Binance recovered before Panasonic IoT did. That made sense because IoT apps often do more than call a remote API.

An IoT app may depend on:

- cloud access
- local network discovery
- nearby-device permissions
- background connectivity
- stable LAN routing

The Panasonic app working on mobile data but failing on Wi-Fi suggested that local network behavior still mattered. Once the phone had a deterministic IPv4 configuration, Panasonic IoT also recovered.

## The Final Working Configuration

The final setup was:

```text
Proxy: None
IP settings: Static
IP address: 192.168.1.150
Gateway: 192.168.1.1
Network prefix length: 24
DNS 1: 1.1.1.1
DNS 2: 8.8.8.8
```

This was not meant as a universal rule that every phone should use a static IP. It was a practical workaround for a constrained environment where I could not change the router settings directly.

## Validation

The fix was tested in the way a real fix should be tested:

- OKX loaded normally.
- Binance loaded normally.
- Panasonic IoT connected normally.
- VPN and web protection were re-enabled.
- The phone was rebooted.
- The fix persisted.

That last point mattered. A network workaround that disappears after reconnect or reboot is not stable enough to trust.

## Takeaways

The lesson was not "static IP fixes everything." The lesson was to follow the failure boundary.

If apps fail only on one Wi-Fi network, do not start by blaming the apps. Check the path:

1. Is the phone using a valid IP?
2. Is the gateway correct?
3. Is DNS behaving differently from other networks?
4. Is IPv6 adding another path that some apps dislike?
5. Is a VPN or web-protection layer intercepting traffic?
6. Does an IoT app depend on local network discovery?

In this case, the stable answer was simple only after the evidence pointed there: give the phone a valid static IPv4 configuration with a correct gateway and known public DNS.

Connected does not always mean healthy. Sometimes the phone is on Wi-Fi, the signal is strong, and the apps still cannot rely on the path they were given.

That is where real troubleshooting starts.
