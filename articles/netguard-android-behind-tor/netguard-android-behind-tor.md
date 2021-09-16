> ❗❗❗ **IMPORTANT**: Netguard leaks IP, do not use it like this! ❗❗❗


# Using Netguard to run Android behind Tor

1. Install [Netguard](https://f-droid.org/en/packages/eu.faircode.netguard/) and [Orbot](https://guardianproject.info/fdroid/)
2. Buy [netguard license](https://contact.faircode.eu/?product=netguardstandalone) 
3. Follow [FAQ: (54) How to tunnel all TCP connections through the Tor network?](https://github.com/M66B/NetGuard/blob/master/FAQ.md)
4. You can check that your browser is using tor now by visiting [check.torproject.org](https://check.torproject.org/).
5. Apps that have Tor already bundled should be whitelisted (Tor Browser, Samourai Wallet, Phoenix Wallet, ...)

## Keep in mind
1. It is not possible to combine other VPN with Netguard. Android can have only one VPN active at a time and Netguard is taking that place.
2. Some apps simply wont work behind Tor, because Tor is blocked or walled by endless captchas. You have to whitelist them (and loose privacy) or unninstall them and find Tor friendly alternative.
