# Development quickstart

**Get up and running with Highway API and start developing your own integration**

Integrating Highway into your app or webite can begin as soon as you create a Highway account, requiring only three steps:

1. [Obtain your API keys](#_1-obtain-your-api-keys) so Highway can authenticate your integration's API request
2. [Install a client library]() so your integration can interact with the Highway API
3. [Make a test API request]() to confirm everythin is up and running

## 1. Obtain your API keys

Highway authenticates your API requests using your account's API Keys. If you do not include your key when making an API request, or use one that is
incorrect or outdated, Highway will then return an authorization error.

You can find the private key under the [user settings](https://highway.smartmonkey.io/highway/account/keys). Private keys will always start with the preffix `priv_` for
users clarity.

If you can't see your keys in the user profile, this means that you don't have Highway API access. Check our subscriptions for further information.

## 2. Install a client library

## 2. Make a test API request

To check that your integration is working correctly, make a test API request using your test secret key to get a list of available plans.

You have to replace `{PRIVATE_KEY}` with your own key.
