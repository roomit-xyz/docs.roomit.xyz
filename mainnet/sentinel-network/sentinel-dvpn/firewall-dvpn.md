# Firewall DVPN

We used ufw for firewall, install ufw

```
apt install -y ufw        
```

Create rules SSH and API

```
ufw allow 22/tcp # For ssh access
ufw allow 7777/tcp # For API Sentinel Wireguard
ufw allow 7776/tcp # For API v2Ray Wireguard
```

Create rules Wireguard

```
WIREGUARD_PORT=$(cat /app/mainnet/sentinel-wireguard/.sentinelnode/wireguard.toml | grep listen_port | awk -F"=" '{print $2}' | sed "s/ //")
ufw allow ${WWIREGUARD_PORT}/udp
```

Create rules for V2Ray

```
V2RAY_PORT=$(cat /app/mainnet/sentinel-v2ray/.sentinelnode/v2ray.toml | grep listen_port | awk -F"=" '{print $2}' | sed "s/ //")
ufw allow ${V2RAY_PORT}/tcp
```
