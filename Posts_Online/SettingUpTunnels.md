---
layout: default 
title: Posts/Tunnel
---

## Why to setup tunnel?
- Help maintain stable connection to remote


## How to setup tunnels?

Example: Connection to direct remote

`ssh -Y -L 4201:localhost4:22 ud4@or-slurm-login.ornl.gov`

run watch command on this terminal <br>
`watch -n 10 w`

Other terminals; connect to local port:
- if the user name of your local and remote are same:
  - `ssh -Y -p 4201 localhost`
- if the user name of your local and remote are different:
  - `ssh -Y -p 4201 ud4@localhost`
---

Example: Connection to bypass remote

`ssh -Y -L 4302:rebellion.ornl.gov:22 ud4@login1.ornl.gov`

It will prompt for passcode. 
if `watch` does not work, use this:
```
#!/bin/bash
while(date); do
uptime
sleep(9)
done
```




