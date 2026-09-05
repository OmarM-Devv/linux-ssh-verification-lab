# Commands used

These commands were used in the lab completed on 5 September 2026. Results below summarise recorded observations rather than reproducing complete terminal output.

| Command | Machine | Purpose and observation |
| --- | --- | --- |
| `ip a` | Kali / Ubuntu | Display network interfaces and IP addresses to identify the VM addresses. The host-only addresses were `192.168.56.102` for Kali and `192.168.56.101` for Ubuntu. |
| `ping -c 4 192.168.56.101` | Kali | Send four ICMP echo requests to Ubuntu to check reachability. Result: 4 transmitted, 4 received, 0% packet loss. |
| `nmap -sV 192.168.56.101` | Kali | Scan the default 1,000 TCP ports and attempt service/version detection on open ports. Repeated before SSH installation, with SSH running, and after stopping and disabling it. |
| `systemctl status ssh --no-pager` | Ubuntu | Inspect the SSH service status without an interactive pager. Used before installation, after installation and after stopping and disabling the service. |
| `sudo apt update` | Ubuntu | Refresh package indexes before installing the SSH server. This command does not itself upgrade installed packages. |
| `sudo apt install openssh-server -y` | Ubuntu | Install the OpenSSH server package, automatically answering yes to package-manager confirmation prompts. |
| `sudo systemctl stop ssh` | Ubuntu | Stop the currently running SSH service. |
| `sudo systemctl disable ssh` | Ubuntu | Remove the SSH service's configured automatic startup through systemd enablement. Disabling alone does not stop a running service, so it was used after `stop`. |

## Verification sequence

1. Inspect addresses with `ip a`, then check connectivity from Kali with `ping`.
2. Run the initial Nmap scan from Kali: host up, all 1,000 default TCP ports closed.
3. Check SSH on Ubuntu: `Unit ssh.service could not be found.`
4. Update package indexes, install OpenSSH server, then check its status: active and running, listening on port 22.
5. Repeat the Nmap scan from Kali: TCP port 22 open, identified as SSH/OpenSSH.
6. Stop and disable SSH on Ubuntu, then check that the service is no longer running.
7. Repeat the Nmap scan from Kali: port 22 no longer open.

No additional command output, exact OpenSSH version, or reboot test is claimed.
