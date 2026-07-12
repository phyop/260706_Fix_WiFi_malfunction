# Public Content Pack

This pack converts the Android Wi-Fi troubleshooting case into reusable public-facing content while preserving the concrete details: one-network failure boundary, static IP `192.168.1.150`, gateway `192.168.1.1`, DNS `1.1.1.1` and `8.8.8.8`, VPN/web protection validation, timeline, privacy handling, and sanitized diagrams.

## Resume STAR

### Short Bullet

- Diagnosed an Android connectivity failure where OKX, Binance, and Panasonic IoT failed only on one home Wi-Fi network, isolated the issue to an invalid Wi-Fi path and router IP conflict, and restored stable app connectivity with a static IPv4 configuration using `192.168.1.150`, gateway `192.168.1.1`, prefix `24`, DNS `1.1.1.1`/`8.8.8.8`, and no proxy.

### STAR Version

**Situation:** Several unrelated Android apps failed only on one home Wi-Fi network. The same phone and apps worked on cellular/mobile data and on other Wi-Fi networks, which made the local Wi-Fi path more likely than a global app outage or phone-wide failure.

**Task:** Identify the root cause without exposing private network data, preserve the complete debugging timeline, and produce a public case study that could help others troubleshoot similar Android Wi-Fi, DNS, IPv6, VPN/web-protection, and IoT connectivity issues.

**Action:** Mapped the failure boundary, checked DNS behavior, IPv6 DNS and routing, VPN/web-protection interception, app permissions, IoT local network behavior, and static IP assignment. Found the most concrete configuration error: the phone had previously used `192.168.1.1`, the same address as the router gateway. Replaced the unstable automatic Wi-Fi path with a deterministic static IPv4 setup: `Proxy: None`, `IP settings: Static`, `IP address: 192.168.1.150`, `Gateway: 192.168.1.1`, `Network prefix length: 24`, `DNS 1: 1.1.1.1`, and `DNS 2: 8.8.8.8`.

**Result:** OKX, Binance, and Panasonic IoT loaded normally. VPN and web protection were re-enabled, the phone was rebooted, and the fix persisted. The public write-up preserved the exact technical details while omitting Wi-Fi QR codes, full MAC addresses, Wi-Fi passwords, personal account identifiers, exact device identifiers, and private screenshots.

### Resume Project Entry

**Android Wi-Fi Debug Case Study**  
Documented a real Android network troubleshooting case where multiple unrelated apps failed only on one Wi-Fi network. Built a reusable debugging narrative around failure-boundary analysis, DNS and IPv6 checks, VPN/web-protection validation, IoT local network behavior, and a final static IPv4 fix using `192.168.1.150`, gateway `192.168.1.1`, and DNS `1.1.1.1`/`8.8.8.8`.

## LinkedIn

### Short Post

Some Android network bugs are not app bugs.

In this case, OKX, Binance, and Panasonic IoT failed only on one home Wi-Fi network. The same phone worked on cellular data and other Wi-Fi networks. That boundary pointed away from reinstalling apps and toward the local network path.

The key finding was an invalid IP assignment: the phone had previously used `192.168.1.1`, the same address as the router gateway. The stable fix was a static IPv4 setup:

- IP: `192.168.1.150`
- Gateway: `192.168.1.1`
- Prefix: `24`
- DNS: `1.1.1.1` and `8.8.8.8`
- Proxy: `None`

After applying it, OKX, Binance, and Panasonic IoT worked. VPN and web protection were re-enabled, the phone was rebooted, and the fix persisted.

Takeaway: when apps fail only on one Wi-Fi network, map the failure boundary before blaming the apps.

### Longer Post

I documented a real Android Wi-Fi debugging case where the phone looked connected, but several apps could not reliably use one home network.

The failure boundary was the useful clue:

- OKX, Binance, and Panasonic IoT failed only on one Wi-Fi network.
- The same phone worked on cellular/mobile data.
- The same apps worked on other Wi-Fi networks.
- Basic Wi-Fi signal looked normal.
- IoT behavior differed from ordinary outbound app traffic.

The investigation checked DNS, IPv6 DNS and routing, VPN/web-protection interception, app permissions, IoT local network behavior, and static IP assignment. The most concrete bug was an IP conflict: the phone had previously been assigned `192.168.1.1`, which was also the router gateway.

The final working configuration used a unique phone IP and known public DNS:

```text
Proxy: None
IP settings: Static
IP address: 192.168.1.150
Gateway: 192.168.1.1
Network prefix length: 24
DNS 1: 1.1.1.1
DNS 2: 8.8.8.8
```

After the fix, OKX, Binance, and Panasonic IoT loaded normally. VPN and web protection were re-enabled, the phone was rebooted, and the configuration still held.

The public write-up uses sanitized diagrams and excludes Wi-Fi QR codes, full MAC addresses, passwords, personal account identifiers, exact device identifiers, and private screenshots.

## Commit Copy

### Suggested Commit Message

```text
docs: expand Android Wi-Fi troubleshooting case study
```

### Commit Body

```text
Rewrite the README as a full public case-study template with the complete failure boundary, timeline, final static IPv4 configuration, validation steps, privacy notes, diagrams, and reusable checklist.

The Medium draft is a long-form English article with title options, SEO metadata, tags, and a complete debugging narrative covering DNS, IPv6, VPN/web protection, IoT behavior, the router IP conflict, and the final static IP fix.

Add a content pack with Resume STAR, LinkedIn copy, commit/PR text, and future extension ideas.
```

## PR Copy

### PR Title

```text
Expand public Android Wi-Fi debugging content pack
```

### PR Summary

- Rewrites `README.md` into a reusable case-study template while preserving the full Android Wi-Fi debugging details.
- Expands `medium-android-wifi-debug-article.md` into a publication-ready English story with 6 title options, SEO metadata, tags, diagrams, and complete validation notes.
- Adds `docs/content-pack.md` with Resume STAR, LinkedIn, commit, PR, and future extension copy.

### PR Testing

- Verified the article word count is within the requested 1,500-2,500 word range.
- Verified required constants and case facts are present: `192.168.1.150`, `192.168.1.1`, `1.1.1.1`, `8.8.8.8`, cellular/other Wi-Fi success, VPN/web protection re-enable, reboot persistence, privacy exclusions, timeline, and diagram references.
- Reviewed the documentation diff for accidental thinning or privacy leaks.

## Future Extensions

1. **Router-side appendix:** Add a companion note explaining how DHCP reservations could avoid manual phone-side static IP configuration when router access is available.
2. **Android settings walkthrough:** Add a sanitized step-by-step guide for changing Android Wi-Fi IP settings to Static, with warnings about avoiding router IP conflicts.
3. **DNS and IPv6 explainer:** Expand the DNS section into a deeper article about IPv4 DNS, IPv6 DNS, and why mixed paths can confuse app-level symptoms.
4. **VPN/web protection checklist:** Add a decision tree for testing Android VPN profiles and web-protection apps without leaving security disabled as the final state.
5. **IoT troubleshooting appendix:** Document why IoT apps may need cloud access, local discovery, nearby-device permissions, background connectivity, and stable LAN routing.
6. **Diagram pack:** Publish the sanitized diagrams as reusable assets: case overview, IP conflict versus fixed path, and troubleshooting path.
7. **Support-runbook version:** Convert the case into a helpdesk checklist for support engineers handling "apps fail only on one Wi-Fi" reports.
8. **Privacy template:** Add a reusable redaction checklist for public troubleshooting write-ups that involve home networks and phone screenshots.

## Privacy Reminder

Keep the public repository free of Wi-Fi QR codes, full MAC addresses, Wi-Fi passwords, personal account identifiers, exact device identifiers, and private screenshots. Use sanitized reconstructions for diagrams and only include LAN IP examples when they are necessary to explain the technical fix.
