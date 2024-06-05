# You Have Mail
Level: Beginner

Description:
```
This challenge is composed of an email, more specifically a .eml file. The email introduces the theme for the forensics group, which is a whistleblower announcing that alien life exists on Earth, and the government knows about it.

[URGENT: Proof of UFO!!! Read in a secure location]
```

## Writeup
Digital Forensics challenge. The download is an email message. Upon opening it up, there is a password locked `evidence.zip` file. The email gives us the password as `53 65 63 75 72 65 5f 43 6f 64 65 3a 4f 72 64 65 72 5f 36 36`. It also says `Utilize a common number system to decrypt that password.` Throw the password into `From Hex` recipe using CyberChef and you get `Secure_Code:Order_66`. `evidence.txt` will be extracted which contains the flag.

**Flag** - `SIVBGR{th3_ev1d3nc3_1s_h3r3}`
