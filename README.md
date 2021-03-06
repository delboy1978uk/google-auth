# google-auth

[![Latest Stable Version](https://poser.pugx.org/delboy1978uk/google-auth/v/stable)](https://packagist.org/packages/delboy1978uk/google-auth) [![Total Downloads](https://poser.pugx.org/delboy1978uk/google-auth/downloads)](https://packagist.org/packages/delboy1978uk/google-auth) [![Latest Unstable Version](https://poser.pugx.org/delboy1978uk/google-auth/v/unstable)](https://packagist.org/packages/delboy1978uk/google-auth) [![License](https://poser.pugx.org/delboy1978uk/google-auth/license)](https://packagist.org/packages/delboy1978uk/google-auth)<br />
[![Build Status](https://travis-ci.org/delboy1978uk/google-auth.png?branch=master)](https://travis-ci.org/delboy1978uk/google-auth) [![Code Coverage](https://scrutinizer-ci.com/g/delboy1978uk/google-auth/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/delboy1978uk/google-auth/?branch=master) [![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/delboy1978uk/google-auth/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/delboy1978uk/google-auth/?branch=master)<br />

This PHP class can be used to interact with the Google Authenticator mobile app for 2-factor-authentication. This class
can generate secrets, generate codes, validate codes and present a QR-Code for scanning the secret. It implements TOTP 
according to [RFC6238](https://tools.ietf.org/html/rfc6238)

For a secure installation you have to make sure that used codes cannot be reused (replay-attack). You also need to
limit the number of verifications, to fight against brute-force attacks. For example you could limit the amount of
verifications to 10 tries within 10 minutes for one IP address (or IPv6 block). It depends on your environment.

Forked from PHPGangsta's lib - see copyright notice.

## installation
Installation is via composer
```
composer require delboy1978uk/google-auth
```

## usage
See following example:

```php
<?php

use Del\Auth\GoogleAuthenticator;

$ga = new GoogleAuthenticator();
$secret = $ga->createSecret();
echo "Secret is: ".$secret."\n\n";

$qrCodeUrl = $ga->getQRCodeGoogleUrl('Blog', $secret);
echo "Google Charts URL for the QR-Code: ".$qrCodeUrl."\n\n";

$oneCode = $ga->getCode($secret);
echo "Checking Code '$oneCode' and Secret '$secret':\n";

$checkResult = $ga->verifyCode($secret, $oneCode, 2);    // 2 = 2*30sec clock tolerance
```
`$checkResult` is a boolean true/false
Running the script provides the following output:
```
Secret is: OQB6ZZGYHCPSX4AK

Google Charts URL for the QR-Code: https://www.google.com/chart?chs=200x200&chld=M|0&cht=qr&chl=otpauth://totp/infoATphpgangsta.de%3Fsecret%3DOQB6ZZGYHCPSX4AK

Checking Code '848634' and Secret 'OQB6ZZGYHCPSX4AK':
OK
```
