# Linux SSH Service Verification Lab

Completed on 5 September 2026.

## Overview

A small Linux/networking self-study lab using two VirtualBox VMs: Kali as the scanning machine and Ubuntu as the target. I checked connectivity, observed the effect of installing an SSH server, then stopped and disabled the service and checked the result from Kali.

## Objective

Practise Linux service management and use a second machine to verify how a service change affects the ports visible over the network.

## Lab environment

| VM | Role | Adapter 1 | Adapter 2 host-only IP |
| --- | --- | --- | --- |
| Kali Linux | Connectivity checks and scanning | NAT for internet access | 192.168.56.102 |
| Ubuntu Linux | Target and SSH service management | NAT for internet access | 192.168.56.101 |

Both VMs used the same VirtualBox Host-Only Adapter for Adapter 2. Tools used were `ip`, `ping`, Nmap, `apt` and `systemctl`.

## Network layout

```text
                        Internet
                       /        \
              VirtualBox NAT  VirtualBox NAT
                (Adapter 1)    (Adapter 1)
                     |              |
                  Kali VM       Ubuntu VM
              192.168.56.102   192.168.56.101
                (Adapter 2)    (Adapter 2)
                     |              |
                     +--------------+
                    Shared host-only network
```

The addresses shown belong to Adapter 2. Adapter 1 provided each VM with its own NAT connection; traffic between the lab VMs used the shared host-only network. The subnet mask was not recorded here.

## Steps performed

1. Created/used Kali and Ubuntu VMs in VirtualBox and checked their addresses with `ip a`. Initially, both used NAT and showed `10.0.2.15`; these separate NAT connections did not provide the shared lab network needed for direct communication.
2. Kept Adapter 1 as NAT for internet access and added Adapter 2 using the same Host-Only Adapter on both VMs. Ubuntu had `192.168.56.101` and Kali had `192.168.56.102`.
3. From Kali, ran `ping -c 4 192.168.56.101`. All four packets were received, with 0% packet loss.
4. Ran `nmap -sV 192.168.56.101` from Kali. The host was up, but all 1,000 default TCP ports scanned were closed.
5. On Ubuntu, ran `systemctl status ssh --no-pager`. It reported `Unit ssh.service could not be found.`
6. Ran `sudo apt update` and `sudo apt install openssh-server -y` on Ubuntu. A subsequent service status check showed `Active: active (running)` and that the server was listening on port 22.
7. Repeated the Nmap scan from Kali. TCP port 22 was open and identified as SSH/OpenSSH.
8. Because SSH was no longer required for the lab, ran `sudo systemctl stop ssh` and `sudo systemctl disable ssh` on Ubuntu. Checked the service status again and confirmed it was no longer running.
9. Repeated the same Nmap scan from Kali. The final scan confirmed that port 22 was no longer open.

## Key commands

On Kali:

```bash
ping -c 4 192.168.56.101
nmap -sV 192.168.56.101
```

On Ubuntu, to inspect and install the service:

```bash
systemctl status ssh --no-pager
sudo apt update
sudo apt install openssh-server -y
systemctl status ssh --no-pager
```

On Ubuntu, after verifying SSH from Kali:

```bash
sudo systemctl stop ssh
sudo systemctl disable ssh
systemctl status ssh --no-pager
```

See [the command notes](notes/commands.md) for all commands used and their explanations, including `ip a`.

## Results

These are summaries of my recorded observations, not full terminal transcripts.

| Check | Observed result |
| --- | --- |
| Connectivity from Kali to Ubuntu | 4 packets transmitted, 4 received, 0% packet loss |
| Initial Nmap scan | Host up; all 1,000 default TCP ports scanned were closed |
| Initial Ubuntu service check | `ssh.service` could not be found |
| After installing OpenSSH server | SSH service active and running; listening on port 22 |
| Nmap scan with SSH running | TCP port 22 open; SSH/OpenSSH identified |
| After stopping and disabling SSH | Service no longer running; final scan showed port 22 was no longer open |

## What I learned

- Separate NAT connections and a shared host-only network serve different purposes. Adding the host-only adapters gave the VMs a network for direct lab communication while keeping NAT for internet access.
- An open port is not automatically a vulnerability. I need to understand which service is using it and whether that service is required.
- Stopping a service and disabling it are separate actions: stopping affects its current execution, while disabling removes its configured automatic startup through systemd enablement.
- Checking service status locally and scanning from another machine gave me two ways to verify the change.

The main lesson was to make a controlled change and independently verify the result from another machine.

## Limitations / scope

- This was a Linux/networking self-study lab focused on connectivity, service management, scanning and verification. It was not penetration testing or a cybersecurity vulnerability assessment, and no exploitation was performed.
- Scans targeted only the Ubuntu VM's host-only address. The default scan covered 1,000 TCP ports, not every TCP port or UDP ports.
- Service detection identified SSH/OpenSSH; no exact version is recorded here, and no vulnerability conclusion is drawn from that identification.
- The final scan established that port 22 was no longer open at that time. No reboot verification or SSH login test is documented.
- No screenshots or raw scan logs are currently included. See [the screenshot notes](screenshots/README.md) for suggested evidence to add if available.
