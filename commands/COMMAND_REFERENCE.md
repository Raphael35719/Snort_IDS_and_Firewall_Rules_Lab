# Command Reference

Commands are based on the uploaded lab worksheet. Run only in the designated CyberOps Workstation/Mininet environment and follow your instructor's directions.

| Command | Purpose |
|---|---|
| `sudo ./lab.support.files/scripts/configure_as_dhcp.sh` | Configure the lab VM network using the provided DHCP script. |
| `ifconfig` | Inspect network interfaces and IP addresses. |
| `ping www.cisco.com` | Test connectivity if permitted; stop with `Ctrl+C`. |
| `sudo ./lab.support.files/scripts/cyberops_extended_topo_no_fw.py` | Start the provided Mininet topology. |
| `mininet> xterm R1` | Open a shell on router R1 from the Mininet prompt. |
| `mininet> xterm H5` / `mininet> xterm H10` | Open shells for the lab hosts. |
| `./lab.support.files/scripts/start_snort.sh` | Start Snort using the lab-provided script. |
| `./lab.support.files/scripts/mal_server_start.sh` | Start the simulated server supplied with the lab; isolated lab only. |
| `netstat -tunpa` | Inspect listening and active network sockets. |
| `tail -f /var/log/snort/alert` | Follow the Snort alert log; stop with `Ctrl+C`. |
| `tcpdump -i H5-eth0 -w nimda.download.pcap` | Capture packets on the lab interface to a PCAP file. Keep captures private unless sanitized and approved. |
| `ls -l nimda.download.pcap` | Verify the capture file exists and inspect its size. |
| `sudo iptables -L -v` | List firewall chains, rules, and packet/byte counters. |
| `sudo iptables -I FORWARD -p tcp -d 209.165.202.133 --dport 6666 -j DROP` | Insert the worksheet's example drop rule in the lab `FORWARD` chain. Do not run outside the authorized lab. |
| `quit` | Exit the Mininet CLI. |
| `sudo mn -c` | Clean up Mininet processes after quitting. |

## Shell notation
- `sudo` runs a command with elevated privileges; use it only when required and authorized.
- `-I` inserts a rule near the beginning of an iptables chain.
- `-p tcp` matches TCP packets.
- `-d` selects the destination address.
- `--dport` selects the destination port.
- `-j DROP` tells iptables to drop matching packets.
- `&` runs a command in the background in a shell.

## Safety note
The worksheet includes a command to request a file named `W32.Nimda.Amm.exe` from a simulated server. This workflow deliberately does not reproduce a malware-download command. Use only an instructor-controlled simulation in an isolated lab; never retrieve or run malware on a personal or production device.
