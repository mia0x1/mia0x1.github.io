---
published: true
description: A simple TCP port scanner written in Bash.
tags:
  - Linux
  - Security
  - Bash
title: A TCP Port Scanner Written in Bash
---

With a cup of coffee in hand, I settled in front of my laptop, opened my IDE, and wrote a simple TCP port scanner in Bash.
You can find it on [GitHub](https://github.com/mia0x1/portscan).
Of course you can use the script for troubleshooting purposes or learning **only in your own environment**, for example, to verify firewall changes. Feel free to test it and add more useful functions.

Per default, it scans the well-known port range from 1 to 1023.
One can specify their own port range by telling the script the first and last port. For example to scan for all ports from 21 to 100 you would use it like this:

```sh
./portscan.sh 10.0.0.1 21 100
```

The output looks something like this:
```
Scanning remote host 10.0.0.1 for open TCP ports in the range between 21 and 100
Port 22 is open
Port 80 is open
```

Below is the full script. You will probably find a more up-to-date version of the script on GitHub in the future. So don't assume that this blog post will be updated automatically.

```bash
#!/bin/bash

# User should be able to input portrange or single port - otherwise scan well known ports

ip_address=$1
# Per default we are using well known ports. These are overwritten when the user passes port numbers as parameters
port_first=1
port_last=1023

if [ -z "$ip_address" ]; then
    echo 'You need to run the script like this: ./portscan.sh $IP [$firstport] [$lastport]'
    exit 1
fi

# If 3 arguments are given (IP address, first port and last port, port variables will be replaced by the user input)

if [ $# -eq 3 ]; then
    port_first=$2
    port_last=$3
fi

# Scan for open ports

echo "Scanning remote host $ip_address for open TCP ports in the range between $port_first and $port_last"

for (( port=port_first; port<=port_last; port++ ))
do
    (echo >/dev/tcp/"$ip_address"/"$port") &>/dev/null && echo "Port $port is open"
    sleep 0.01
done
```