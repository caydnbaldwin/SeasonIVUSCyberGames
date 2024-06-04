# Secret
Level: Beginner

Description:
```
We managed to get an informant to obtain a document which we believe might contain information confirming alien existence, but it is too censored.

[Secret.pdf]
```

## Writeup
Digital Forensics challenge. `Secret.pdf` shows a top secret report with lots of redacted text, including a section at the bottom that says `Flag:` followed by a redaction. I ran a pdftotext command in my terminal that output `Secret.txt`, which showed the flag in plaintext.

**Flag** - `SIVBGR{C0nta1n_Th3_Al13ns}`
