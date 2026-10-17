# Snort IDS and Firewall Rules Lab

## Overview
This repository documents the defensive cybersecurity practical from the **“Lab - Snort and Firewall Rules”** worksheet. The lab uses the CyberOps Workstation VM and Mininet to observe Snort IDS alerts, inspect firewall rules, insert a lab-scoped `iptables` rule, verify its effect, and clean up.

## Objectives
- Prepare the CyberOps Workstation network environment.
- Start the provided Mininet topology and Snort IDS.
- Monitor Snort alerts and understand alert fields.
- Understand the role of a PCAP packet capture in investigation.
- Inspect `iptables` chains and add a rule to block specified TCP traffic in the isolated lab.
- Verify the rule and document actual results.
- Stop Mininet and clean up processes.

## Repository structure
- `docs/` — workflow, results, and reflection.
- `evidence/` — screenshots and sanitized evidence from your own run.
- `commands/` — commands used in the lab and explanations.

Each folder contains its own README explaining what belongs there.

## Safety and scope
Run the practical only in the provided CyberOps Workstation/Mininet lab or another isolated environment you are authorized to administer. The worksheet describes a simulated malware-hosting server and a file named `W32.Nimda.Amm.exe`. Do **not** download, execute, or redistribute real malware on a personal or production system. If the supplied lab environment is unavailable, document the expected observation rather than contacting an external host or retrieving a malware file.

The worksheet's example destination is `209.165.202.133` on TCP port `6666`. Treat it as a lab example, not a recommendation to probe or block unrelated networks. Confirm addresses and topology in your authorized lab before running commands.

## Suggested workflow
1. Read `docs/README.md` and `docs/LAB_WORKFLOW.md`.
2. Review `commands/README.md` and `commands/COMMAND_REFERENCE.md`.
3. Prepare the virtual environment using the worksheet.
4. Capture screenshots and sanitized output for `evidence/`.
5. Fill in `docs/RESULTS.md` with your actual observations. Do not claim success unless you observed it.
6. Push the project to your own GitHub repository.

## Push with Git Bash
Extract the ZIP, open Git Bash in the extracted `snort-firewall-rules-github-workflow` folder, then run:

```bash
git init
git add .
git commit -m "Add Snort and firewall rules lab workflow"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
git push -u origin main
```

Replace the remote URL with your own GitHub repository URL. If this folder is already connected to a GitHub repository, use:

```bash
git add .
git commit -m "Update Snort firewall lab documentation"
git push
```

## Before publishing
- Remove passwords, tokens, private details, and unrelated personal information from screenshots/logs.
- Do not commit malware samples or sensitive packet payloads.
- Publish only evidence you personally collected and are permitted to share.
