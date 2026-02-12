VLESS + Cloudflare + nginx-proxy + 3x-ui

Example main domain: example.com
Example panel domain: panel.example.com
Email: admin@example.com

FEATURES:
- https://example.com/       -> opens normal HTML page (camouflage)
- https://example.com/ws     -> VLESS WebSocket endpoint
- https://panel.example.com  -> 3x-ui admin panel (separate subdomain)

ARCHITECTURE:
nginx-proxy (SSL/HTTPS) -> nginx-router (main domain) -> static site / VLESS
                        -> 3x-ui-panel (panel subdomain) -> 3x-ui admin panel

SETUP:
1) Copy .env.example to .env:
   Linux/Mac: cp .env.example .env
   Windows:   copy .env.example .env

2) Edit .env file:
   - DOMAIN=example.com (your main domain)
   - PANEL_DOMAIN=panel.example.com (your panel subdomain)
   - ADMIN_EMAIL=admin@example.com

3) Point DNS A records to server IP:
   - example.com -> YOUR_SERVER_IP
   - panel.example.com -> YOUR_SERVER_IP

4) Enable Cloudflare Proxy (orange cloud) for BOTH domains

5) Set Cloudflare SSL mode: Full (strict)

6) Start the stack:
   docker compose up -d

ACCESS:
Main site: https://example.com
Admin panel: https://panel.example.com

Inbound settings (in 3x-ui panel):
Protocol: VLESS
Port: 10000
Transport: WebSocket
Path: /ws (or your custom path from .env)
TLS: OFF

Client settings:
Address: example.com (your main domain from .env)
Port: 443
Network: WebSocket
Path: /ws (or your custom path from .env)
TLS: ON
SNI: example.com (your main domain from .env)

SECURITY NOTES:
- xui-data/ directory is excluded from git (contains sensitive data)
- Keep your .env file private (not committed to git)
- All 3x-ui ports (2053, 10000) are only accessible within Docker network
- Admin panel on separate subdomain with automatic HTTPS
- Consider using Cloudflare Access or firewall rules to restrict panel access
