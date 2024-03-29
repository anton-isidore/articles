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
Use plaintext files, synchronize them with Syncthing and encrypt them with Cryptomator (which is currently not possible because of [this](https://github.com/cryptomator/android/issues/35)).

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

I have even annoucned $300 bounty 💰 to imlement this: https://github.com/keepassxreboot/keepassxc/issues/3560#issuecomment-1566742219

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
- There is https://juicysms.com/ - I haven't tested yet, but looks promising. They don't have Lightning Network, but they claim you can pay with onchain Bitcoin, Monero and Litecoin, so maybe some swap servicies like fixedfloat.com/ can help.

#### Graveyard 🪦
- The [lnsms.world](https://lnsms.world/) service was shut down.

### Sadness #8: Phones are tracked by the [IMEI](https://en.wikipedia.org/wiki/International_Mobile_Equipment_Identity)
- Phones are easily tracked by this. Switching SIMs wont help, this ID is unique to the phone itself.

#### Light at the end of the tunnel
- The IMEI can be changed.
- There is a project that allows to changed IMEI every day or so: https://invisv.com/pgpp/
   - More info about project: https://www.usenix.org/conference/usenixsecurity21/presentation/schmitt
- [Banana phone & Changing IMEI](https://wiki.lunardao.net/imei.html)

### Sadness #9: There are no reliable NoKYC Credit Cards providers

This is **problably unsolvable** as Credit Cards lives in the first realm of coercion of the state.

**Requirements**
- Card is bought (topped up) with Bitcoin (ideally Lightning Network)
- It must work over Tor or VPN (both aquisition of the card **and the payment**)
- Card must worldwide accepted (usually prepaid card are not working at many stores)

I don't believe it will be ever reliably solved. Here and then a new company pops up, it works for a while and they its shut down by card provider, bank or the regulator.

#### Very, very dimm light at the end of the tunnel

- ~[https://coindebit.io/](https://coindebit.io/)~ - It was great, but currently the company is not operational (["we transition to a card issuing provider who can better support our model"](https://www.coindebit.io/announcement)), they are promising to come back, but I doubt it.
- [https://thebitcoincompany.com/](https://thebitcoincompany.com/) - Not working over VPN, terrible UX (some browser from the app, copying of some weird code, ...). They use [prepaiddigitalsolutions.com]( https://www.prepaiddigitalsolutions.com) as provider, which is shit.
- [https://cakepay.com/](https://cakepay.com/) - It works, but they support only Bitcoin on-chain and Monero, so you have to use service like [FixedFloat](https://fixedfloat.com/) to swap Lightning to Monero. They use [prepaiddigitalsolutions.com]( https://www.prepaiddigitalsolutions.com) as provider, which is shit.
- ~[https://paywithmoon.com/](https://paywithmoon.com/)~ - No longer working (["termination of our contract with the card’s Issuing Bank"](https://twitter.com/paywithmoon/status/1624548553598111746?cxt=HHwWhMC4mayTx4stAAAA)), they are promising to come back, but I doubt it.

Many card providers are just re-selling cards from https://www.prepaiddigitalsolutions.com/ as provider, which is shit (no 3D-Secure, rejected by many vendors, terrible UX, ...)

### Saddness #10: All hotels and accomodations are KYC
In most countries (espetialy in the socialist EU) there is law that requires all hotels, etc. report everyone to the police. So they require IDs/Passports to do KYC.

**There is AFAIK no way around it.**

It is possible to raise chances that they "forgot" to check your documents by:

1. Using small family run accomodations
2. Doing remote check-in (you don't talk to any person) so it is hard to enforce the actual document check
3. Motivate them to not ask question by paying with cash and not wanting the receipt

### Saddness #11: Services requiring you to Sing-In with the email

This is cancer of the web. They try to get your real email and get your identity and ability to spam you. 

The think is, **they dont need it**. You can perfectly implement Sign-In with 

1. Just *any* username and password
2. Use [Lightning Network Auth](https://github.com/lnurl/luds/blob/luds/04.md)
3. Use public-private key pair in same way the [Nostr](https://nostr.com/) does it

But do NOT require my email. You don't need it!

#### "Solutions"
- Email alias providers alike [simplelogin.io](https://simplelogin.io/) 
- Create and manage ton of email addresses that you create for each service ([proton.me](proton.me/) can be used for this)
