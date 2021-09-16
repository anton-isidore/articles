# Design of Identity Manager with Identity Generator

Most of the services today don\'t respect privacy and require our identity. The only way how to use those services anonymously is **to generate unique fake-identities** (name, email, phone or, even physical address) for each of those services.

This article describes the theoretical concept and proposes a design to a technical solution to this problem at scale.

## The concept of Identity Manager

The best practice with passwords is to have a unique password for each service. **This article proposes the same approach to identities.**

`Identity Manager` is an extension to the Password Manager concept. Every entry (username+password) is also linked to identity, and the good practice of "having one unique identity per account" is emphasized.

> There are use-cases where it is unavoidable to have some accounts under one identity (mostly because of governments). Bank accounts or airline accounts are good examples.

This can be easily achieved with most Password Managers. The only requirement is that they support some hierarchical organization of records.

The image above shows that each fake-identity is used only for one service (and email account fired by that service). It also shows how the "real identity" contains multiple accounts where using the "real identity" is enforced by the government.

This part is easy to implement and the tools needed are already available. **The challenging part is to create and maintain the identities at scale.**

## The concept of Identity Generator

To create an account, the services require some information about the user\'s identity. The amount of information that those services require vary. This article focuses on the name, email, phone number and physical address. In the context of this article, they are called *features*.

This article proposes the `Identity Generator` as a tool similar to Password Generator. Identity Generator will generate an identity on-demand with all requested features.

Possible features that Identity Generator should be able to generate:

1. A believable name in the context of the service (language, gender, ...) capable of receiving and storing emails on a non-blacklisted domain
2. The valid phone number for a given country (there are rare cases when a phone number is required but is not validated)
3. Non-blacklisted phone number capable of receiving and storing SMS
4. Physical address that exists (in most cases it\'s not validated by the service)
5. Physical address with service of forwarding (scanning) the incoming mail (there are rare cases when a physical address is validated)

## The problem

Most services do validate both the email address and the phone number upon registration by sending a verification code to it. Additionally, verification code is sent not just for registration but also for future logins. This means, both email and phone number needs to be **maintained** for the whole lifetime of the account associated with it.

The second problem is that as services battle spam and "inauthentic activity" they block many of free or one-time email providers and SMS receivers.

Free email providers can be used, but:

- The email registration process is quite complicated and time-consuming.
- There are usually some anti-robot measures that make it hard to use a VPN.
- The user is vulnerable to random mistakes and metadata leakages, resulting in the connection of the identities or even exposing his real identity.
- The user needs to do a lot of log-ins to different email boxes to get login verification codes which are annoying, time-consuming, and susceptible to mistakes.

## Proposed solution

The proposed **solution is a software that will be a Password Manager that will be *identity-oriented* with the capability of generating fresh identities on-demand**. It\'s UI will be designed to guide the user for using the best practice of *one identity per service*.

This **`Password and Identity Manager`** will contain both Password Generator and the `Identity Generator`.

Some features may be generated directly in the software, some will require communication to an external service. In this article, we call such a service responsible for the generation of the feature the *feature provider*.

- Communication with feature providers will be done via Tor to protect the user from metadata leaks.
- To allow modularization and 3rd party feature providers the **standardized protocol** to communicate to those providers will be used.

This protocol will handle:

- the **payment process** for both feature generation and maintenance
- transfer of the generated feature to the user
- **access to remote data** related to the feature (email box, SMS inbox, physical address mailbox, …)

## Feature providers

### Name and surname

The Identity Generator will be able to generate a random name for any selected context (country, language, gender). There are databases of names already available on the internet which can be utilized.

For example:

- The generator of [English (US) names](https://namey.muffinlabs.com/) that also provides [API](https://namey.muffinlabs.com/name.json?with_surname=true&frequency=all)
- [fakenamegenerator.com](https://www.fakenamegenerator.com) that beside name can generate a lot of other details about the identity

> Note: The generation of names appears to be easy, but doing so for various cultural contexts may turn to be very challenging. [[1]](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/)

### Email

The Identity Generator will be able to create an email address and the email box that will persist for an extensive period (because of login verification codes).

- Email address will be semi-similar to the generated name, sometimes containing the generated name (or its parts), sometimes not. The real, leaked databases can be used to train some models for this.
- To prevent the misuse of such a service to send spam, the email address should be able **only to receive emails** and not to send them.
- To **cover the costs** of running the service, the reasonable **payment in a crypto** (preferably Bitcoin Lightning Payment) will be required. The micro-transaction model is heavily recommended because the user of Identity Generator needs to create many email addresses. This is also a big reason to use Bitcoin Lightning Network.
- There must be a mechanism that will mitigate the threat of domains getting blacklisted by the services.

Creating and using such an email address should be cheap to not incentivize users to reuse them.

### Phone number

The Identity Generator should be able to create a phone number, and the SMS box that will persist for an extensive period (because of login verification codes).

- The phone number **should be unique** (no number reuse) because most of the services won\'t allow registration on the already taken number.
- There will be a **choice of the country code** to match the context of the identity.
- To prevent the misuse of such a service to send spam, it won\'t be possible to send SMS nor make a call from such a phone number.
- Same as in the case of the email the reasonable **payment in a crypto** will be required. However, it is expected that payment will be higher because running the phone number farm is much more expensive than running an email server.

There are already some "anonymous" SIM cards providers that maybe can be utilized for such a service. But the research needs to be made in this field. Also, there are still some countries where it is possible to just buy an anonymous sim card, so it may be possible to run a farm. [[2]](https://prepaid-data-sim-card.fandom.com/wiki/Country_List)

There is great example, there is great service [smsprivacy.org](https://smsprivacy.org), that provides physical UK SIM cards, but it\'s quite expensive 0.0015 BTC/24h and because of that you cannot keep phone number just for yourself. In case the service requires you to re-authorize (usually on-login), you will lose that service\'s account.

Creating and using such a phone number should be cheap to not incentivize users to reuse them. The phone numbers will be of course significantly more expensive than an email address, but on the other side, not so many services require them.

> If it still turns to be too expensive for the user, it may be a valid reason to break to best practice and use one identity for multiple services. But it is necessary to communicate this to the user and provide the UI that will allow adequate risk management.

### Physical address

The Identity Generator will be able to generate random, but the existing address for the given geographical context.

In rare cases the service may require a verification of the physical address. They send a paper letter with a verification code. Thus, the Identity Generator needs to be able to provide a unique physical address with the service of providing scans of all incoming mail.

This is the most expensive feature described in this article. It is expensive because it probably requires direct human labor. The cost will be also covered by payment in cryptocurrency (preferably "micro"-transaction in Bitcoin Lightning Network).

## Conclusion

The article presents the problem of the de-anonymization of the internet and provides the conceptual design of the `Password and Identity Manager` to help to solve this problem to some degree.

The solution proposed in this article would provide an **easy and comfortable solution** that would **allow the mass-adoption** of the practice of having **one unique fake-identity per each service**.

- - - - - -

## Sources

1. [Falsehoods Programmers Believe About Names](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/)
2. [Prepaid Data SIM Card Wiki](https://prepaid-data-sim-card.fandom.com/wiki/Country_List)
3. [James Stanley - What if we could assume new identities at will?](https://incoherency.co.uk/blog/stories/identities.html)
