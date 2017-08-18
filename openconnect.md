# Openconnect

Openconnect is a replacement for "Cisco AnyConnect VPN"

Install with:

```
brew install openconnect
```

Use with Biohub credentials:


```
sudo openconnect --user [username] https://vpn.czbiohub.org
```

Use with UCSF Credentials

```
sudo openconnect --authgroup="Single-Factor Pulse Clients" --user sfXXXXXX --juniper https://remote.ucsf.edu/openconnect
```
