# Lex Integrity on MacOS 10.12 Sierra and later (and Linux?)
This guide solves the problem of connecting to the PPTP VPN service [Lex Integrity](https://integrity.st/) by [The 5th of July Foundation](https://5july.org/) from MacOS since support for PPTP was removed in 10.12 Sierra and later.

Even though the GUI doesn't show support for PPTP anymore the underlying daemon is still installed and working just as before. By using the daemon directly we are able to connect to any PPTP VPN service we like.

PPPD is available for Linux as well which probably means that this solution might work there too. If you manage to connect to Lex Integrity on Linux using this guide, please let me know!

## Configuration
Create the file `/etc/ppp/peers/lexintegrity` and add this:

```bash
## VPN server address
remoteaddress lexvpn.integrity.st

## Credentials
user YOUR_USERNAME
password YOUR_PASSWORD

## Logging
logfile /var/log/ppp.log

## Other settings
plugin PPTP.ppp
noauth
redialcount 1
redialtimer 5
idle 1800
mru 1436
mtu 1436
receive-all
novj 0:0
ipcp-accept-local
ipcp-accept-remote
refuse-eap
refuse-pap
refuse-chap-md5
hide-password
looplocal
nodetach
usepeerdns
debug
defaultroute
```

Edit the lines `user` and `password` accordingly.

## Connect to VPN
`sudo pppd call lexintegrity`

## Disconnect VPN
`sudo killall pppd`

## Logs
The PPPD log is found here:
`/var/log/ppp.log`