# Control Panel
Level: Beginner

Description:
```

Agent, we've identified what appears to be ARIA's control panel. Luckily there's no authentication required to interact with it. Can you take down ARIA once and for all?

https://uscybercombine-s4-control-panel.chals.io/

[control-panel]
```

## Writeup
Web Exploitation challenge. The url leads to a webpage with a few different links, each one with written code or options to select. The downloadable folder reveals the following organization:
```
- control-panel
| - Dockerfile
| - run.py
| - setup.sh
| - challenge
  | - main.py
  | - templates
    | - index.html
| - config
  | - destroy_humans.sh
  | - destroyer.py
  | - supervisord.conf
```
The `main.py` is a Flask application that defines routes that execute system commands based on user input. This is a vulnerability because the application does not properly validate or sanitize the input passed to the `arg` parameter. The `destroy_humans.sh` script is invoked with an argument passed to it directly without proper validation, which exposes a vulnerability that allows the attacker to execute arbitrary commands. The `destroyer.py` script sets up an HTTP server with endpoints for status, destruction, and shutdown. This is where we find our end goal, to call the `/shutdown` endpoint which would reveal the flag. `supervisord.conf` configures the supervisord service to manage the Flask app and the HTTP server. A command injection needs to be crafted to select `destroy_humans` to trigger the execution of the `destroy_humans.sh` script, which would invoke `destroyer.py`, where the `/shutdown` endpoint will be triggered by the `arg` using the `curl` command.
```
https://uscybercombine-s4-control-panel.chals.io/?command=destroy_humans&arg=curl%20-s%20localhost:3000/shutdown
```
This malicious url returns a webpage with the flag.

**Flag** - `SIVBGR{g00dby3_ARI4}`
