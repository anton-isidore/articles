# My client or employer forces me to use Google

If your client or employer user Google Suite and you are unable to leave them or you are unable to avoid using it. Here is what to do:

## Company enforces 2FA

Google (due to bug or evil nature of the company) forces you to provide phone number. Alternatively you can use some hardware key as alternative option but Google supports only U2F (and **NOT** FIDO2), which will cluster all your Google accounts where you use that particular hardware key.

So the trick is:
- Enable 2FA with phone number that you get from [sms4sats.com](https://sms4sats.com/) or any other service that will work. You maybe need to try couple of numbers before you get one that works.
- Then go to Account → Security → 2-Step Verification and add the Authenticator App (thats the [OTP](https://en.wikipedia.org/wiki/One-time_password)). Use any OTP app on your phone that is secure and private (obviously **NOT** the Google Authenticator).
- Once it is sucessfully set-up, **remove** the phone 2FA option.

And thats it! You have won small battle against Google. You haven't give them your number. 🎉

## Connect calendar to your Phone

To add Google calendar to Android Phone (obviously the de-googled one). You need [DAVx5](https://www.davx5.com/) app ([available on F-Droid](https://f-droid.org/en/packages/at.bitfire.davdroid/))

1. If you use 2FA on your Google Account you have to [create app password](https://support.google.com/accounts/answer/185833?hl=en).
2. If you don't use 2FA you can just enable [allow less secure apps](https://myaccount.google.com/lesssecureapps)
3. Open DAVx5, give it the neccessary permissions and add new account.
4. Select "Login with URL and user name"
5. Base URL is `https://www.google.com/calendar/dav/your.email@company.com/events/`
6. Username is your email (`your.email@company.com`)
7. If you use 2FA, password is the app's password from step 1. Otherwise is's your password.

Finally, you then just enable your calendar to be synced and you should see it in your Calendar App (my recomandation is [Etal](https://github.com/Etar-Group/Etar-Calendar).

