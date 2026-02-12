VLESS + Cloudflare + nginx-proxy + 3x-ui

Example domain: example.com
Email: admin@example.com

FEATURES:
- https://example.com/  -> opens normal HTML page
- https://example.com/ws -> VLESS WebSocket endpoint

ARCHITECTURE:
nginx-proxy (SSL/HTTPS) -> nginx-router (traffic routing) -> 3x-ui (VLESS) / static site

SETUP:
1) Copy .env.example to .env:
   Linux/Mac: cp .env.example .env
   Windows:   copy .env.example .env

2) Edit .env file and replace example.com and admin@example.com with real values.

3) Point DNS A record to server IP.

4) Enable Cloudflare Proxy (orange cloud).

5) Set Cloudflare SSL mode: Full (strict)

6) Start the stack:
   docker compose up -d

3x-ui panel:
http://SERVER_IP:2053

Inbound settings:
Protocol: VLESS
Port: 10000
Transport: WebSocket
Path: /ws (or your custom path from .env)
TLS: OFF

Client settings:
Address: example.com (your domain from .env)
Port: 443
Network: WebSocket
Path: /ws (or your custom path from .env)
TLS: ON
SNI: example.com (your domain from .env)

SECURITY NOTES:
- xui-data/ directory is excluded from git (contains sensitive data)
- Keep your .env file private (not committed to git)
- Port 10000 (VLESS) is only accessible within Docker network
- nginx-router handles all traffic routing internally
- Consider restricting access to port 2053 using firewall rules
