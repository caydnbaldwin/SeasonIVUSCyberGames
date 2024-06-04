# Hunt
Level: Beginner

Description:
```
Agent, it looks like ARIA has spun up a simple website. Is there anything you can find to give more information about it's plans?

https://uscybercombine-s4-hunt.chals.io/

```

## Writeup
Web Exploitation challenge. The link takes you to a landing page for ARIA. I looked at the source code that showed `<!-- Don't forget to check in on the bots! --><!-- p1: SIVBGR{r1s3_ -->` commented out. The second comment was part one of the flag. The first comment was a reference to `/robots.txt` page, which showed one disallowed file `/secret-bot-spot` and part two of the flag `p2: 0f_th3_`. `/secret-bot-spot` didn't immediately reveal anything, however the bottom of the source code calls a script `/static/js/robot.js`. Part three of the flag is shown commented out at the bottom `// p3: r0b0ts!}` of the script.

**Flag** - `SIVBGR{r1s3_0f_th3_r0b0ts!}`
