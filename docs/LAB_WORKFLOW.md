# Snort IDS and Firewall Rules — Lab Workflow

## Purpose
Practice observing IDS alerts with Snort and responding by applying a firewall rule in an isolated CyberOps Workstation/Mininet environment.

## Requirements
- CyberOps Workstation virtual machine.
- Oracle VirtualBox and an internet connection for the setup described in the worksheet.
- The lab-provided support files and Mininet topology.
- Authorization to administer the virtual lab.

> **Safety:** The worksheet describes a simulated malware-hosting server and a file named `W32.Nimda.Amm.exe`. Do not retrieve or run real malware on your host or production systems. Use only the instructor-provided isolated lab. If it is unavailable, document the expected observation instead.

## Part 1 — Prepare the virtual environment
1. Open VirtualBox and check the CyberOps Workstation network setting. The worksheet recommends Bridged Adapter, or NAT where required by a Wi-Fi/proxy setup.
2. Start the VM and open a terminal.
3. Run the provided DHCP configuration script from the lab directory:
   ```bash
   sudo ./lab.support.files/scripts/configure_as_dhcp.sh
   ```
4. Verify the VM has a network address:
   ```bash
   ifconfig
   ```
   The worksheet also suggests `ping www.cisco.com` to test connectivity; stop with `Ctrl+C` and follow your instructor's network-use rules.

## Part 2 — Firewall and IDS logs

### Step 1 — Start Mininet and Snort
1. Start the provided topology:
   ```bash
   sudo ./lab.support.files/scripts/cyberops_extended_topo_no_fw.py
   ```
2. Wait for the `mininet>` prompt.
3. Open a terminal for R1:
   ```text
   mininet> xterm R1
   ```
4. In the R1 shell, start Snort:
   ```bash
   ./lab.support.files/scripts/start_snort.sh
   ```
   Leave this terminal running. The worksheet indicates Snort loads `/etc/snort/snort.conf`.
5. Open terminals for H5 and H10:
   ```text
   mininet> xterm H5
   mininet> xterm H10
   ```
6. In H10, start the lab's simulated server:
   ```bash
   ./lab.support.files/scripts/mal_server_start.sh
   ```
7. Check listening sockets:
   ```bash
   netstat -tunpa
   ```
   In the worksheet's example, the server listens on TCP port `6666`.
8. Open a second R1 terminal and monitor alerts:
   ```bash
   tail -f /var/log/snort/alert
   ```
   Stop monitoring with `Ctrl+C`.

### Step 2 — Observe the IDS alert
1. In the authorized lab only, follow your instructor's provided traffic-generation step to simulate the client connecting to the lab server. Do not download or execute malware outside the isolated lab.
2. Watch the alert log for a Snort alert. The worksheet's sample message is `Malicious Server Hit!`.
3. Record the actual alert timestamp, protocol, source/destination IPv4 addresses, source/destination ports, and message in `RESULTS.md`.
4. If no alert appears, document that and troubleshoot the lab rather than claiming success.

### Step 3 — Packet capture evidence
The worksheet uses `tcpdump` on H5's `H5-eth0` interface to write a packet capture. Use packet capture only in the authorized lab and capture only traffic needed for the exercise.

Example syntax:
```bash
tcpdump -i H5-eth0 -w nimda.download.pcap
```
The worksheet uses `&` to run capture in the background. If you use that approach, return it to the foreground with `fg` and stop with `Ctrl+C`. Verify the file exists and is non-empty:
```bash
ls -l nimda.download.pcap
```
Do not upload malware samples or captures containing credentials, private payloads, or sensitive data to a public repository. A sanitized summary or screenshot of file metadata is usually safer.

### Step 4 — Inspect firewall rules
1. Open another R1 terminal.
2. List current rules and counters:
   ```bash
   sudo iptables -L -v
   ```
3. Identify `INPUT`, `FORWARD`, and `OUTPUT`. The worksheet explains that traffic passing through R1 is handled by `FORWARD`.

### Step 5 — Add and verify the lab blocking rule
Only in the isolated lab, add the worksheet's example rule:
```bash
sudo iptables -I FORWARD -p tcp -d 209.165.202.133 --dport 6666 -j DROP
```

Meaning:
- `-I FORWARD` inserts a rule into the `FORWARD` chain.
- `-p tcp` matches TCP traffic.
- `-d 209.165.202.133` matches the worksheet's example destination IPv4 address.
- `--dport 6666` matches destination TCP port 6666.
- `-j DROP` drops matching packets.

Verify the rule:
```bash
sudo iptables -L -v
```
Record the actual output and packet counters. Do not run this example on a personal, school, workplace, or production network unless you are authorized and have confirmed the destination is correct.

### Step 6 — Verify the effect
In the lab, repeat the instructor-approved connectivity test to the simulated server. The worksheet expects the connection attempt to time out after the rule is applied. Record your actual outcome; it can vary by environment.

### Step 7 — Cleanup
1. Return to the terminal running Mininet and enter:
   ```text
   quit
   ```
2. After Mininet exits, clean up its processes:
   ```bash
   sudo mn -c
   ```
3. Confirm the lab has stopped.

## Completion checklist
- [ ] CyberOps Workstation received a network address.
- [ ] Mininet topology started.
- [ ] Snort started and alert log was monitored.
- [ ] Actual alert fields were recorded, or no alert was documented.
- [ ] Current iptables rules were inspected.
- [ ] Lab blocking rule was added and verified in the isolated environment.
- [ ] Follow-up test result was recorded.
- [ ] Mininet processes were cleaned up.
- [ ] Evidence was sanitized before sharing.
