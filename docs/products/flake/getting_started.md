# Getting started

In this section we describe the first steps necessary to start using SmartMonkey's API with Flake. 

## Flake 
Flake is a web application available at [https://flake.smartmonkey.io](https://flake.smartmonkey.io). 
It allows you to: 
- manage account
- manage keys
- see the quota use
- and see the results of API requests. 

## Manage account
Flake allows to manage your account with different options:
- Register
- Login
- Forgot password

**Register
Register at [Flake home](https://flake.smartmonkey.io) or [Register page](https://flake.smartmonkey.io/register). You will receive an email with a link to validate your account. 

**Login
Login at [Flake home](https://flake.smartmonkey.io) or [Login page](https://flake.smartmonkey.io/login). 

**Forgot password
If you forgot your password you can use the link Forgot password in the [Login page](https://flake.smartmonkey.io/login) or you can go directly to [Reset password page](https://flake.smartmonkey.io/reset-password). There you can enter your email address to request a link to create a new password.

## API Key
Once you are logged into Flake on the [Keys page](https://flake.smartmonkey.io/console/keys) you have the list of keys that displays the available keys that are stored in the system for the user. 

**Create key
You can create a key by pressing the button CREATE. A dialogue appears with different options:
- Name: name of the key
- Tag (optional): you can add tags to the key. 
- Test mode (optional): you can mark a key for testing purposes if needed (see below more details).


## Test mode
The test mode is intended for development purposes only. 

It can be used to check if the request arrives correctly and is formatted correctly. The optimizer sends a response to the request without any cost on the user's quota. 

The test mode is a property of an API Key and it can be activated and deactivated at any time by the user. 

From the KEYS page you can see the created keys and their properties. There is a column Test mode to activate/deactivate the test mode for a key. 

**Test mode request**
On the REQUEST page the test mode requests are marked with the label "test mode". 
