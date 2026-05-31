# Tabla explicativa

En este apartado incluiremos una tabla de como han quedado todos nuestros servicios una 
vez implementados incluyendo puertos internos, dominio local, subdominio y redes docker: 

| Servicio | Puerto | Red Local | Subdominio | Red docker |
|----------|--------|------------|---------|---------|
| [Caddy](./caddy.md) | - | - | - | Caddy_net |
| [Dockge](./dockge.md) | 5001 | dockge.lan | dockge.bitcld.com | - |
| [AdGuard Home](./adguard.md) | 53, 3000 | dns.lan | - | Caddy_net |
| [Nextcloud](./nextcloud.md) | 8080 | cld.lan | cld.bitcld.com | - |
| [Inteligencia artificial](./ia.md) | 3003 | chat.lan | chat.bitcld.com | - |
| [Vaultwarden](./vaultwarden.md) | 3002 | pass.lan | pass.bitcld.com | Caddy_net |
| [Uptime Kuma](./uptime-kuma.md) | 3001 | monitor.lan | monitor.bitcld.com | Caddy_net |
| [Netdata](./netdata.md) | 19999 | stats.lan | stats.bitcld.com | - |

El único servicio en el que actualmente tenemos una IP estática es Vaultwarden, la cuál es `172.28.0.2` . Sin embargo, es algo interesante de añadir en todos los servicios para que todo el tráfico pase cifrado mediante `https://`.
  