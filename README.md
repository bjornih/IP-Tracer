# IP Tracer - EU/NL Infrastructure Detector

A browser-based tool that resolves URLs to IP addresses and detects if any associated infrastructure is located in the Netherlands or EU.

## Features

- **Paste URLs or upload CSV** — auto-extracts domains from any column
- **DNS resolution** via Cloudflare DNS-over-HTTPS (A, AAAA, CNAME chain following)
- **IP geolocation** — country, city, ISP/org, AS number
- **Flags infrastructure** as NL (Netherlands), EU, or Non-EU with color-coded tags
- **Summary dashboard** with counts at a glance
- **Export** results as CSV, JSON, or flagged-only (EU/NL entries)

## Usage

1. Open `index.html` in a browser (or visit the [GitHub Pages site](https://bjornih.github.io/IP-Tracer/)))
2. Paste URLs (one per line) or upload a `.csv` file
3. Click **Trace All**
4. Review results — Netherlands and EU infrastructure is highlighted
5. Export results as needed

## Limitations

- No ICMP traceroute (browser security model) — uses DNS resolution + IP geolocation
- CDNs (Cloudflare, Akamai, etc.) may resolve to edge nodes near the user, not the origin
- ip-api.com free tier: HTTP only, rate-limited (batch queries mitigate this)
- Geolocation accuracy depends on IP registry data

## Tech

Zero dependencies. Single HTML file. Runs entirely in the browser.

- DNS: [Cloudflare DoH](https://developers.cloudflare.com/1.1.1.1/encryption/dns-over-https/)
- Geolocation: [ip-api.com](http://ip-api.com/) batch API

