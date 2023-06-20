# My client or employer forces me to use Google

If your client or employer user Google Suite and you are unable to avoid using it. Here is what to do:

## Company enforces 2FA

Google (due to bug or evil nature of the company) forces you to provide phone number. Alternatively you can use some hardware key as alternative option but Google supports only U2F (and **NOT** FIDO2), which will cluster all your Google accounts where you use that particular hardware key.

So the trick is:
- Enable 2FA with phone number that you get from [sms4sats.com](https://sms4sats.com/) or any other service that will work. You maybe need to try couple of numbers before you get one that works.
- Then go to Account -> Security -> 2-Step Verification and add the Authenticator App (thats the [OTP](https://en.wikipedia.org/wiki/One-time_password)). Use any OTP app on your phone that is secure and private (obviously **NOT** the Google Authenticator).
- Once it is sucessfully set-up, **remove** the phone 2FA option.

And thats it! You have won small battle against Google. You haven't give them your number. 🎉

## ConnectCalendar to your Phone

To setup it:

1. Base URL is `https://www.google.com/calendar/dav/your.email@company.com/events/`.
2. Option [allow less secure apps](https://myaccount.google.com/lesssecureapps) needs to be enabled.
