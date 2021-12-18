# Privacy: The browsing

Browsers tell a LOT about us, and it does not end with browsing history and cookies. Using a VPN to hide IP is not enough. The browser can be easily fingerprinted. This article summarises defense practices against such privacy invasions.

## Browser: Tor

It is simple: **Use Tor.** Seriously. Tor is not for criminals and drug-dealers. Tor is really for everybody, and **using it is easy**.

> *It\'s not that I have something to hide… I have nothing I want you to see.-- [Anon](https://www.youtube.com/watch?v=XU6cpB7GosE)*

It looks and works just like Firefox. Download it directly from [torproject.org](https://www.torproject.org/).

Sometimes, pages are not working with Tor. My general advice is: **If the page is not working or is actively blocking Tor, don\'t use it.** Do not use services that do not respect your privacy.

## Browser: Firefox

👉 There is amazing guide [Yet Another Firefox Hardening Guide](https://chrisx.xyz/blog/yet-another-firefox-hardening-guide/) that you should definitelly check.

However, sometimes, it is not possible to use Tor (governments, banks, airlines, …).

In this case, this article promotes using [Firefox](https://www.mozilla.org/en-US/firefox/new/) because:

- It is an independent browser (no Google involved)
- It supports DNS over HTTPS
- It has a unique extension for context separation called [Containers](https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/)

### Encryption

#### HTTPS

Browser encrypts communication between the client and the server using [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security). This extension to the HTTP protocol is called [HTTPS](https://en.wikipedia.org/wiki/HTTPS), and all websites you are visiting should support it.

Extension [HTTPS Everywhere](https://addons.mozilla.org/en-US/firefox/addon/https-everywhere/) will help you to never connect to sites that support HTTPS over old HTTP.

#### DNS over HTTPS

Before your browser starts to communicate with the server of the website you are about to visit, it will ask the [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) server where that page is on the internet. DNS Server will translate domain (`duckduckgo.com`) into IP address (`40.114.177.156`).

Communication between your browser and the DNS server is usually not encrypted, and your ISP can see pages you are visiting and eventually block them.

The solution is using **DNS over HTTPS** to encrypt the communication between the browser and the DNS.

- To enable it in Firefox: `Settings` -> `Network` -> `Enable DNS over HTTPS` ([official documentation of how to do it](https://support.mozilla.org/en-US/kb/firefox-dns-over-https#w_manually-enabling-and-disabling-dns-over-https) with screenshots)
- Use [Cloudflare test](https://www.cloudflare.com/ssl/encrypted-sni/) or [DNS leak test](https://dnsleaktest.com/) to check if it\'s working.

#### ESNI

During the TSL handshake, the server will present the TSL certificate. But because the certificate is issued per domain, and the server can host multiple domains, the browser will send domain (`duckduckgo.com`) during this handshake. Problem is, it will send it unencrypted. ESNI solves that by encrypting the communcation (more detail [here](https://www.cloudflare.com/learning/ssl/what-is-encrypted-sni/)).

- To enable it in Firefox: `about:config` by setting `network.security.esni.enabled` to `true`.
- Use [Cloudflare test](https://www.cloudflare.com/ssl/encrypted-sni/) to check if it\'s working.

(There is also another big step forward that would enable encryption for the whole TLS handshake called [ECH](https://tools.ietf.org/html/draft-ietf-tls-esni-08). It is explained in more detail [here](https://blog.cloudflare.com/encrypted-client-hello/). But it will probably take some time to see this rolled out.)

### Ad Blocking

[uBlock Origin](https://ublockorigin.com) is the best option here. Don\'t forget to turn on specific filters for your country and annoyances (EU cookies, …).

### Preventing IP leak

Your IP is one of the most identifying information. In most cases, it is the IP of your provider. However, IPS will always spy on you and collaborate with any state agency.

> IPS is the first enemy and you want to hide everything from him.

To hide your IP:

1. **Use anonymization networks** like [Tor](https://www.torproject.org/), [I2P](https://geti2p.net/en/), and hopefully in the future [NYM](https://nymtech.net/)
2. **Use a VPN** but be cautious. They claim a "no logs" policy, but there is no way how to truly verify it.
3. Disable WebRTC (used for video calls) because [it may leak your IP](https://restoreprivacy.com/webrtc-leaks/).
4. Make sure you do not leak your ISP IPv6.

Tools to test IP leak:

- [ipleak.net](https://ipleak.net/)
- [Browserleaks WebRTC Leak Test](https://browserleaks.com/webrtc)
- [ipleak.org](https://ipleak.org/)

To disable WebRTC in Firefox, go to `about:config` and change `media.peerconnection.enabled` to `false`. But services like [Jitsi Meet](https://meet.jit.si/) will stop working.

### Tracking prevention

The goal of tracking is to pair two different visits to a site made by the same person. This information is used to create a profile and used against the person (marketing, scamming, law enforcement, …)

Those two methods are being used:

1. Some identifier is put into users browser when he/she visit the page for the first time. The user\'s identity is then recognized by this identifier every time the user visits the page again ([cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies), [localstorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage), …).
2. The browser is fingerprinted and recognized next time.

### How to combat tracking

It is possible to prevent the browser from saving any data at all, but then most of the pages won\'t work anymore.

Instead, we need to:

#### 0. Enable strict mode
- Go to `about:preferences#privacy`
- Select `Strict`

More info [here](https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop).

#### 1. Make it temporary (delete cookies and other metadata often)

The [Cookie Autodelete](https://addons.mozilla.org/en-US/firefox/addon/cookie-autodelete/) extension solves that. But it is important to configure it properly:

1. Check "Enable Automatic Cleaning" and set time reasonable time (for example 15 seconds).
2. Check "Clean All Expired Cookies"
3. **Check all checkboxes in the section "Other Browsing Data Cleanup Options".** This is very important because the page can store some identifiers in other places than cookies (for example in the [localstorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)).

#### 2. Separate contexts

Context separation is one of the most important practices to maintain privacy. In each context, we may selectively reveal some information but if we maintain those contexts separated, our actions from one cannot be attributed to the "person" in the other.

##### Containers
[Firefox Multi-Account Containers](https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/) extension adds an option to create different containers that allow the user to browse the web in a sandboxed mode. Full documentation of those containers can be found on [Mozilla Wiki](https://wiki.mozilla.org/Security/Contextual_Identity_Project/Containers).

Best practices are:

1. **Create a separate container for every webpage** (domain, platform) where you want to stay logged-in.
2. **Limit container only for that specific domain** to prevent accidentally breaching the context of the container by opening a different page (domain) in it.
3. **If you will not log in, do NOT use the container** and let [Cookie Autodelete](https://addons.mozilla.org/en-US/firefox/addon/cookie-autodelete/) clear all data after each visit. A good example of this is YouTube. If you create a YouTube container, Google will be tracking you between different visits because the container will keep cookies and other data.

##### First-Party Isolation
First-Party Isolation is a privacy feature of the Firefox that restricts cookies, cache and other data access to the domain level so that only the domain that dropped the cookie or file on the user system can access it.

1. Load the URL `about:config` in the Firefox address bar.
2. Search for `privacy.firstparty.isolate` and set to `true`.
3. Search for `privacy.firstparty.isolate.restrict_opener_access` and set it to `false`. 

More infor about this feature can be found in [this arcicle](https://www.ghacks.net/2017/11/22/how-to-enable-first-party-isolation-in-firefox/).

#### 3. Block trackers

Many trackers will be blocked just by using uBlock Origin. On top of that, those extensions add extra protection:

- [Decentraleyes](https://addons.mozilla.org/en-GB/firefox/addon/decentraleyes/) extension will prevent the requests to CDNs if not needed and also strips sensitive data when the request is necessary.
- [Privacy Badger](https://addons.mozilla.org/en-US/firefox/addon/privacy-badger17/) blocks trackers not by using a blacklist [but by learning](https://privacybadger.org/#How-does-Privacy-Badger-work).

### How to combat fingerprinting

Combating fingerprinting is currently nearly impossible. [Tor is trying to do it](https://blog.torproject.org/browser-fingerprinting-introduction-and-challenges-ahead) but there are still many issues. For example [here](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/28290).

Some tools can tell, how much unique the browser is:

- [AmIUnique](https://amiunique.org)
- [Cover Your Tracks](https://coveryourtracks.eff.org/)
- [Browser Leaks](https://browserleaks.com/)

The [Indigo Browser](https://indigobrowser.com/) is an example of a browser that claims to be resistant to fingerprinting. However, the page is only in Russian and it is quite expensive. The Indigo Browser was not tested for this article and is mentioned here only as an example.

## Conclusion

This article explained why using Tor is very important and recommends to use Tor whenever is possible. **Tor should be the default.**

In cases, where Tor cannot be used, this article presented all currently known mitigations (to the author) to prevent leakage of private information through the browser.

The biggest challenge for further improvement is fingerprint prevention and this article will be updated with new findings.

## Bonus: Hide menu bar
To hide annoying menu bar and prevent it from popig back every time the ALT key is pressed: go to `about:config`, filter for `ui.key.menuAccessKeyFocuses` and disable it (source: [Reddit](https://www.reddit.com/r/firefox/comments/6451y1/can_i_disable_the_altkey_menu_bar_showing_behavior/)).

## Sources:

- [DNS over TLS vs. DNS over HTTPS | Secure DNS](https://www.cloudflare.com/learning/dns/dns-over-tls/)
- [Firefox DNS-over-HTTPS](https://support.mozilla.org/en-US/kb/firefox-dns-over-https)
- [Encrypt it or lose it: how encrypted SNI works](https://blog.cloudflare.com/encrypted-sni/)
- [Encrypted SNI Comes to Firefox Nightly](https://blog.mozilla.org/security/2018/10/18/encrypted-sni-comes-to-firefox-nightly/)
- [Good-bye ESNI, hello ECH!](https://blog.cloudflare.com/encrypted-client-hello/)
- [What is encrypted SNI? | How ESNI works](https://www.cloudflare.com/learning/ssl/what-is-encrypted-sni/)
- [Encrypted Client Hello: the future of ESNI in Firefox](https://blog.mozilla.org/security/2021/01/07/encrypted-client-hello-the-future-of-esni-in-firefox/)
- [Firefox setup for better privacy](https://hackyourself.io/courses/firefox-setup-for-better-privacy)
- [Browser Fingerprinting: An Introduction and the Challenges Ahead](https://blog.torproject.org/browser-fingerprinting-introduction-and-challenges-ahead)
- [Tor Gitlab: Add uBlock Origin to the Tor Browser](https://gitlab.torproject.org/tpo/applications/tor-browser/-/issues/17569)
- [How to enable First-Party Isolation in Firefox](https://www.ghacks.net/2017/11/22/how-to-enable-first-party-isolation-in-firefox/)
