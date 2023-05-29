---
title: "ExploringOTP"
date: 2020-06-29T09:18:25+05:30
draft: false
tags: ["otp", "python", "how-to", "notes","totp","hotp"]
---

![SecretShared](/img/otpWorks.jpg)

What is second factor authentication?  The code which we enter after entering username and password, how does this work ?

In this blog post will be exploring how one time password/passcode (OTP) works. Will start with what is included in standard on OTP and then implementing this using python code.

<!--more--> 

---

## RFC note
 Two major OTP implementations<!--more--> are defined in RFC. HOTP as described in [RFC4226](https://tools.ietf.org/html/rfc4226) and TOTP described in [RFC6238](https://tools.ietf.org/html/rfc6238). 
**HOTP** - HMAC based one time password
- Code is generated at server end only
- When prompted for OTP, the code is sent to user(SMS/Email). 
**TOTP** - Time based one time password
- Code is generated at user end and server.Time is used as counter here.  
- When prompted for OTP, the code generated at user end is verified against the code generated at server end. Avoids storing mobile numbers at server end to send SMS.  
---

## 10000 Feet View 
![OTP overview](/img/otp10000.jpg)
When someone signs up for OTP, a secret is shared with user, as a barcode or as a text. This text/barcode is added to a authenticator app(Authy, Google Authenticator etc). App generates a unique code which is in sync with code generated at server. When user enters the code, this is validated at server and if its a match, Access granted!!.

---

## Implementation Details
![SecretShared](/img/otpWorks.jpg)
Let's explore how HOTP and TOTP is implemented according to RFC's.
HOTP uses HMAC -SHA1 algorithm to generate random string. This random string is truncated so that it's suitable for use.

*How HOTP validates code*, given a secret
HOTP(K,C) = Truncate(HMAC-SHA-1(K,C))
**C** - 8bit counter
**K** - Shared secret between client and server
The HOTP value is truncated so that it can be used as one time password.
**OTP** = { HOTP modulo (10^ Digits)}
**Digits** - number of digits in OTP
[Check this Example code in RFC](https://tools.ietf.org/html/rfc4226#section-5.4)

*How TOTP validates code,* The main difference here, is that "Time" is used as counter. Unixtime is used as counter. Idea of using unix time is avoid errors due to timezone. *Unixtime* is the number of seconds elapsed since midnight UTC of January 1, 1970.
>**Example**: Unixtime for 26 July 2020 11:30:30 is 1595763030. Time counter in TOTP changes every 30 seconds. Value '30' is defined in RFC. This is a balance between usuability and security. So that user does not wait too long for a new code or attacker is given enough time to brute force codes.
 
* **Counter** = T = (Current Unixtime - T0 /X)  
* **T0** is the Unix time to start counting time steps (default value is 0, i.e., the Unix epoch) [RFC 6238](https://tools.ietf.org/html/rfc6238#section-4.1)  
* **X** represents the time step in seconds (default value X = 30 seconds)(RFC reference)[RFC 6238](https://tools.ietf.org/html/rfc6238#section-4.1). 
This ensures OTP changes every 30 seconds.  TOTP uses HOTP algorithm as basic building block,

>**TOTP = HOTP {K,T}**.  
**K** - secret key  
**T** - Counter  

![OTPFLOW](/img/otpFlow.jpg)

 To summarise,
>When a user is prompted to enter OTP, code from authenticator app is entered. This is sent to server. Remember this code is generated with current unixtime on user device and sent to server. Now at server end, the code is generated based on current unixtime at server and compared with code sent by client. There may be some time lag between the time when code is generated at client and time when code is generated at server. This should be within 30 seconds.

---

## Implementing OTP using python
There is a python module which implements OTP as per RFC.\
pyOTP is a python implementation of OTP according to [RFC4226](https://tools.ietf.org/html/rfc4226) and [RFC6238](https://tools.ietf.org/html/rfc6238). 

[PyOTP](https://github.com/pyauth/pyotp) 

### Demo on TOTP , 
{{< youtube vBntD8XMHkc >}}

\
Let's explore how we can use pyotp to implement this.
### Generate unique code
- **Generate unique code**, using pyotp we can generate random string and share this with user OR generate a barcode using [QR Code generator](https://github.com/neocotic/qrious) and share. Then user can scan or add Secret code to authenticator apps like Google Authenticator, Authy.

Below is a sample code which generates secret string, gives an output which can be used to generate barcode and shows how HOTP codes are generated with counter.
![code1](/img/otpSampleCode.png)

``` python
import pyotp

SecureString = pyotp.random_base32()
#Generate secure string to be shared with user

key = pyotp.TOTP(SecureString)
    
a=pyotp.totp.TOTP(SecureString).provisioning_uri(name='batman@google.com', issuer_name= 'NotesOTP')

#Convert unique string to barcode, output will be used in QRious to generate QR code.

print(" Secure Code is: ", SecureString)
print ("\n For QR Code use this -->: ", a)
print("\n TOTP is : ",key.now())

#Counter based OTP - HOTP
SecureString_H = pyotp.random_base32()
key_hotp = pyotp.HOTP(SecureString)

print("\n HOTP at 0 :", key_hotp.at(0))
print("\n HOTP at 1 :", key_hotp.at(1))
print("\n HOTP at 2 :", key_hotp.at(2))
```
*Output of this code*
![Output](/img/otpCodeOutput.png)

### ServerCode to Validate
- ServerCode to Validate( *this is not how server validates, but to make it simple a small python script will simulate code generated at server to see if both OTP's are in sync*), ideal flow would be like this, When client sends a TOTP to server, server retrieves secret for user and generates TOTP at server based on current unix time. This TOTP is compared with TOTP sent by client. As you can see, time difference plays a major role in sync 30seconds should compensate delays. Anything beyond this will start increasing attack surface for brute force attacks.

![OTP_code](/img/otpValidate.png)

``` python
import pyotp
import time

#Server code to validate
key = pyotp.TOTP('ENXXHOWWQ425QHJZ')
while(True):
    otp = key.now()
    localtime = time.localtime()
    result = time.strftime("%I:%M:%S %p", localtime)
    print(result, otp, end="", flush=True)
    print("\r", end="", flush=True)
    time.sleep(1)
```
Once we have these two scripts, lets generate QR code ,
Go ahead and scan this, 
![QRCode](/img/otpQR.png#center)


```python
--snip--
a=pyotp.totp.TOTP(SecureString).provisioning_uri(name='batman@google.com', issuer_name= 'NotesOTP')

#Output
>>> print (a)
>>> otpauth://totp/NotesOTP:batman%40google.com secret=ENXXHOWWQ425QHJZ&issuer=NotesOTP
```

Scan Code with an authenticator app on phone
![Scan Code](/img/otpScanCode.png)

Code Sync
![Sync_01](/img/otpCodeSync.png)

Second Code Sync
![sync_02](/img/otpCodeSync2.png)

Validate code in authenticator app using this script -> [Servercode to validate](#servercode-to-validate)
---

## References
- [How time based OTP works](https://www.freecodecamp.org/news/how-time-based-one-time-passwords-work-and-why-you-should-use-them-in-your-app-fdd2b9ed43c3/)
- [PyOTP](https://github.com/pyauth/pyotp)
- [Authenticator](https://pypi.org/project/authenticator/)
- [RFC 4226](https://tools.ietf.org/html/rfc4226)
- [RFC 6238](https://tools.ietf.org/html/rfc6238)
- [UnixTimeStamp](https://www.unixtimestamp.com/index.php)
- [QR Code generator](https://github.com/neocotic/qrious)




