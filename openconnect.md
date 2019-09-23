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

## Debugging "Can't assign requested address"

From [here](https://www.ovpn.com/en/faq/troubleshooting/mac-cant-assign-requested-address-code49), if you get an error that looks like this:

```
(base)
 ✘  Mon 23 Sep - 08:25  ~ 
  biohubvpn
Password:
POST https://64.71.0.146/
Failed to connect to 64.71.0.146:443: Can't assign requested address
Failed to connect to host 64.71.0.146
Failed to open HTTPS connection to 64.71.0.146
Failed to obtain WebVPN cookie
```

There are two ways of fixing this issue. The first one is easier as you simply have to restart your computer. Otherwise, you can solve the problem through the terminal. If you have a cable connection, you type the following into a terminal:

```
sudo ifconfig en0 down

sudo route flush

sudo ifconfig en0 up
```

If you instead use WiFi, you type:

```
sudo ifconfig en1 down

sudo route flush

sudo ifconfig en1 up
```
