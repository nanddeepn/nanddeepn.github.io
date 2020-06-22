---
title: "Configure Multi-Factor Authentication (MFA) for Office 365"
date: "2019-06-19"
---

## Overview

Authentication to Office 365 is driven by Azure Active Directory (shortly known as Azure AD). It manages identities and authentication for Office 365. Each user can access Office 365 resources using the credentials (a combination of username and password). Multi-factor authentication adds a layer of security on top of it.

In this article, we will explore the concept of Multi-factor authentication (MFA) and how to enable it in Office 365.

 

## Issues with Traditional Password Approach

The major shortcoming of using username and password is that it can be compromised easily or with some efforts because of several reasons.

1. Users tend to use the same password across multiple applications.
2. Store all passwords​ at one single location.
3. Easy to remember passwords makes it easy to guess.

With various tricks, attackers can easily target these scenarios to crack the password.

 

## Multi-factor authentication (MFA)

Multi-factor authentication by the name itself denotes more than one authentication factor for successful authentication or combination of multiple authenticators which provides multiple factors. It can be a combination of any of below:

1. Software (Password, PIN)
2. Hardware (Token, Key)
3. User identity (Biometric data)

Multi-factor authentication adds an extra layer of protection on top of username and password.

 

## Set up Multi-factor Authentication

1. Open Office 365 Admin Center ([https://admin.microsoft.com](https://admin.microsoft.com/)).
2. From the left menu, click Users > Active users.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-556.png)

1. Select the users to enable MFA.
2. Click Enable.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-557.png)

1. Click “enable multi-factor auth”.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-558.png)

1. Multi-factor auth is now enabled for the selected accounts.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-559.png)

1. Click close.

 

## Multi-Factor Auth Status

MFA status of users is one of below values.

- Disabled: MFA is not enabled for the user.
- Enabled: The user is enrolled for MFA but has not completed the registration process.
- Enforced: The user may not have completed registration.

 

## Bulk Update the users for MFA

The users can be updated in bulk to enable or disable MFA. This can be handled using a CSV file.

1. Login as a global administrator.
2. On Multi-factor authentication page, click bulk update.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-560.png)

1. Click “Download a sample file” to download sample CSV.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-561.png)

1. Modify this file to include users to enable or disable MFA.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-562.png)

1. From “BROWSE FOR FILE…” dialogue, upload the file with records to enable or disable MFA.
2. Click next arrow.
3. Once the file is verified, click next to update the accounts.
4. Once finished, click Done.

Please note new users cannot be added using this approach.

## Test the MFA

1. In the browser, open Office 365 portal ([http://portal.office.com](http://portal.office.com/))
2. Provide the sign-in credentials
3. The sign-in window will ask for more information.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image.jpeg)

1. Click Next.
2. Select the method to contact (either call or text message) to authenticate.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-563.png)

1. In case you have opted to receive the code by text message, enter the received code.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-564.png)

1. Click Verify.
2. The next screen will provide the app password. In some apps, like Outlook, Apple Mail, and Microsoft Office, you can't use a phone to secure your account. To use these apps, you'll need to create a new "app password" to use in place of your work or school account password.

![](https://nanddeepnachanblogs.com/wp-content/uploads/2020/03/word-image-565.png)

1. Click Done.

The user will be logged in to Office 365 portal using MFA.

 

## Summary

Static password can be compromised by an attacker. Multi-factor authentication adds an extra layer of protection on top of username and password. Office 365 used Azure AD for authentication. MFA can be easily setup per user or as a bulk. Only Global administrator can enable or disable MFA.

 

This content was originally posted [here](https://www.c-sharpcorner.com/article/configure-multi-factor-authentication-for-office-365/).
