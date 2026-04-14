# IP Tracer - Infrastructure Geo Detector

A browser-based tool that performs deep infrastructure discovery on URLs — looking behind CDNs to find origin servers — and flags hosting in any selected countries.

## Features

- **Paste URLs or upload CSV** — auto-extracts domains from any column
- **Configurable country flagging** — select individual countries or use presets (Netherlands, EU, EU+EEA, Five Eyes)
- **Deep scan mode** with six discovery layers:
  - **A/AAAA/CNAME** — standard DNS resolution, CDN chain detection
  - **MX records** — mail server infrastructure (often reveals origin hosting)
  - **SPF/TXT parsing** — extracts IPs from SPF records, follows `include:` chains recursively
  - **NS records** — nameserver infrastructure geolocation
  - **Subdomain probing** — checks 20 common subdomains (`mail.`, `direct.`, `origin.`, `cpanel.`, `vpn.`, `admin.`, etc.)
  - **Certificate Transparency** — queries crt.sh for historical subdomains, resolves them to IPs
  - **HTTP header probing** — extracts `Server`, `X-Powered-By`, `Via`, `CF-Ray`, etc. (CORS permitting)
- **Origin mismatch detection** — flags when origin IPs are in different countries than CDN IPs
- **CDN detection** — identifies Cloudflare, Akamai, Fastly, CloudFront, and other major CDNs
- **Expandable per-domain cards** — each domain shows all layers with geolocated IPs
- **Export** results as CSV, JSON, or flagged-only CSV

## Usage

1. Open the [live tool](https://bjornih.github.io/IP-Tracer/) or `index.html` locally
2. Select which countries to flag (defaults to EU) using chips or preset buttons
3. Toggle **Deep scan** on for full infrastructure discovery
4. Paste URLs (one per line) or upload a `.csv` file
5. Click **Trace All**
6. Review results — flagged domains auto-expand, showing all discovery layers
7. Export results as needed

## How it looks behind CDNs

Most websites use CDNs like Cloudflare, which mask the origin server's location. This tool uses several techniques to find the real infrastructure:

| Technique | What it reveals |
|---|---|
| MX records | Mail servers are rarely behind CDNs — they often sit on or near the origin |
| SPF records | SPF `ip4:` and `include:` directives list IPs authorized to send mail — often origin servers |
| Subdomain probing | `mail.`, `direct.`, `cpanel.`, `ftp.` subdomains frequently bypass CDN |
| Certificate Transparency | Historical certificates reveal subdomains that may point to origin IPs |
| HTTP headers | `Server`, `Via`, `X-Powered-By` headers can leak backend technology info |

## Limitations

- No ICMP traceroute (browser security model) — uses DNS + geolocation instead
- CDN edge nodes resolve based on the **user's location**, not the origin
- HTTP header probing works only for CORS-enabled sites (most will block it)
- crt.sh can be slow or timeout — results are best-effort
- Geolocation accuracy depends on IP registry data (ipinfo.io)
- SPF recursion capped at depth 3 and 50 IPs to avoid excessive lookups

## Tech

Zero dependencies. Single HTML file. Runs entirely in the browser.

- DNS: [Cloudflare DoH](https://developers.cloudflare.com/1.1.1.1/encryption/dns-over-https/)
- Geolocation: [ipinfo.io](https://ipinfo.io/) (free, HTTPS, 50k/month)
- Certificate Transparency: [crt.sh](https://crt.sh/)

## License

MIT
