# Big unsolved sadnesses in my life

### ~~Sadness #1: Missing "read-only" solution for Twitter~~ `SOLVED`
~~I dont want to have an acoount there. Even information what I am subscribed to shall be private.~~

**🎉 `SOLVED` 🎉 See the [Fritter](https://github.com/jonjomckay/fritter) app.**

### Sadness #2: Messaging apps (Signal) requires phone number
Linking phone number with Singal account is not really good for privacy. There is no valid reason for Signal (or any other messigin app) to require phone number.

For reference: [Feature Request: Using Signal without a phone number or SIM card, especially for children #10432](https://github.com/signalapp/Signal-Android/issues/10432).

#### Light at the end of the tunnel
Possible solution may be in 
1. usingapp like [Wickr](https://wickr.com) 
2. or utilizing open protocols like [Matrix](https://matrix.org/).

Matrix seems like a best solution so far. There is quite nice app [Element](https://element.io/) that is for both desktop and mobile. But sadly it is unusable because [notifications are not working](https://github.com/vector-im/element-android/issues/3263).

### Sadness #3: Absence of open-source, end-2-end encrypted Trello alternative
There is no such a solution on the market which provides:

- Trello like efficient UI
- end-2-end encryptoion
- encryption at rest
- mobile app + desktop app with concistent UI

#### Compromise but small light at the end of the tunnel
Use plaintext files, synchronize them with Syncthing and ecrypt them with Cryptomator (which is currently not possible because of [this](https://github.com/cryptomator/android/issues/35)).

### Sadness #4: ProtonMail app is not ~~open-source~~ available on F-Droid

Progress:
- Protonmail Android is now [open-source](https://github.com/ProtonMail/proton-mail-android).
- [Github Issue about FDroid](https://github.com/ProtonMail/proton-mail-android/issues/1)
- [Issue in F-droid](https://gitlab.com/fdroid/rfp/-/issues/1323)

#### Light at the end of the tunnel
- Izzy [added Protonmail App into his reposiory](https://github.com/ProtonMail/proton-mail-android/issues/1#issuecomment-677775345)

### Sadness #5: Cannot use TREZOR as key into my Password Manager
> *Traditional password managers share one weak point - the master password. Once someone knows your Master Password, they can get all of your passwords. Trezor Password Manager doesn't require a master password. Instead, your Trezor is the master key to your passwords. [source: [trezor.io](https://trezor.io/passwords/)]* 

Problem is, that TREZOR Password manager sucks. There is no Android support. It is [not actiely](https://github.com/trezor/trezor-password-manager) developed and is for Chrome only.

#### Light at the end of the tunnel
- TREZOR supports [FIDO2](https://fidoalliance.org/specs/fido-v2.0-rd-20180702/fido-client-to-authenticator-protocol-v2.0-rd-20180702.html#sctn-hmac-secret-extension) and there is [an issue for implementing it into KeepassXC](https://github.com/keepassxreboot/keepassxc/issues/3560)
- TREZOR also supports 2FA over U2F standart (see [wiki](https://wiki.trezor.io/User_manual:Two-factor_Authentication_with_U2F) or [blog-post](https://blog.trezor.io/secure-two-factor-authentication-with-trezor-u2f-e940fd5a60af)).

### Sadness #6: Missing FIDO2 support in various servicies
Why FIDO2? Read [this article](https://blog.trezor.io/make-passwords-a-thing-of-the-past-a402745750dc).

- ProtonMail, there are [maybe some plans](https://www.reddit.com/r/ProtonMail/comments/g2iovq/when_is_u2f_in_protonmail/fnn36xe/) to add it. They also metioned it in [this tweet](https://twitter.com/protonmail/status/979100397087444992).

Also Tor Browser does not enables `webauthn` by default. You have to enable it in `about:config` by setting `security.webauth.webauthn` to `true`.


### Sadness #7: No good service to buy virtual phone number
I need anonymous phone numbers to generate virtual identities. 

**Required parameters:**
- Anonymous
- Can be paid with Bitcoin over Lightning Network
- Available over Tor or at least over VPN
- Scalable: I need to be able to buy a lot of numbers
- Durable: I need to keep that number for life (if I need to attach an identity to it)
- Reliable: They need to work, most servicies bans virtual numbers

The is [Hushed](https://hushed.com/), but its is shit. Does not work on Degoogled phone, does not accepts Bitcoin/Lightning/Monero and it is quite expensive if you want to have a lot of numbers.

#### Light at the end of the tunnel
- There is https://sms4sats.com that partially solves that for SMS verifications.
- There is also https://lnvpn.net/phone-numbers but thay are just proxy to https://sms4sats.com 

#### Graveyard 🪦
- The [lnsms.world](https://lnsms.world/) service was shut down.

### Sadness #8: Phones are tracked by the [IMEI](https://en.wikipedia.org/wiki/International_Mobile_Equipment_Identity)
- Phones are easily tracked by this. Switching SIMs wont help, this ID is unique to the phone itself.

#### Light at the end of the tunnel
- The IMEI can be changed.
- There is a project that allows to changed IMEI every day or so: https://invisv.com/pgpp/
   - More info about project: https://www.usenix.org/conference/usenixsecurity21/presentation/schmitt
