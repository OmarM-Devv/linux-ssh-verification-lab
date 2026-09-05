# Screenshot evidence

These three images are cropped copies of genuine screenshots attached to the original lab record, "02 Linux Lab", for the lab completed on 5 September 2026. They were prepared for this repository on 6 September 2026. The original chat attachments were not modified.

| File | What it shows | Cropping |
| --- | --- | --- |
| [01-connectivity-ping.png](01-connectivity-ping.png) | Kali's `ping -c 4 192.168.56.101`, four replies and 0% packet loss | Excludes the desktop, identifying prompt and unrelated interface output. |
| [02-initial-nmap-scan.png](02-initial-nmap-scan.png) | The initial `nmap -sV 192.168.56.101` scan: host up and all 1,000 scanned TCP ports closed | Excludes the window title, identifying prompts, unrelated commands and MAC address line. |
| [03-ssh-service-running.png](03-ssh-service-running.png) | Ubuntu's SSH service status showing `active (running)` | Excludes the identifying prompt and log lines containing the machine name. |

Only cropping was applied. No terminal text was generated, replaced or reconstructed. The lab IP addresses remain visible to connect the evidence to the documented setup.

## Evidence limits

The Nmap scan with SSH open and the final scan after stopping and disabling SSH were confirmed in messages, but no screenshots of those checks were attached to the original lab record. These images do not claim to show those later results.

If evidence from a later repeat is added, label it with the actual capture date and identify it as a repeat.
