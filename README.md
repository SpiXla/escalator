# Escalator

This repository contains the documentation for the Escalator privilege escalation project. The goal of the project is to document the process used to gain elevated access on the provided virtual machine, explain the security impact, and present the work in a professional format suitable for submission or audit.

## Project Goal

The objective of the challenge is to:

- identify the target VM on the network
- enumerate exposed services
- gain an initial foothold
- escalate privileges to `root`
- retrieve the flag from `/root/root.txt`

## Documentation

- [Walkthrough](Walkthrough.md): Step-by-step process of exploiting the vulnerability.
- [Remediation](Remediation.md): Suggestions to fix or mitigate the vulnerability.
- [Vulnerability Report Email](Vulnerability-Report-Email.md): Drafted vulnerability report email.
- [Ethical Hacking Report](Ethical-Hacking-Report.md): Discussion of ethical responsibilities in security testing.

## Walkthrough

1. Configure the VM and Kali Linux with a Host-Only network, then discover the target with `arp-scan --localnet`. The target address identified during the exercise was `192.168.128.3`.
2. Scan the target with `nmap` and identify the exposed FTP, SSH, and HTTP services.
3. Enumerate the web server and discover the `/files` directory.
4. Log in to FTP as `anonymous`, confirm upload access, and upload the included `exploit.php` webshell to the web-accessible files directory.
5. Start `nc -lvnp 9001` on the testing machine and use the webshell to execute a URL-encoded reverse-shell command. This provides a `www-data` shell.
6. Inspect `/` and `.runme.sh`, recover the `shrek` credential material, and use the recovered password to authenticate over SSH.
7. Run `sudo -l` as `shrek`. The misconfiguration allows `/usr/bin/python3.5` to run as root without a password.
8. Spawn a root shell with:

	```bash
	sudo /usr/bin/python3.5 -c 'import pty; pty.spawn("/bin/bash")'
	```

9. Confirm the root context with `whoami` and read `/root/root.txt`.

Screenshots documenting each stage are in the [screenshots](screenshots/) directory.

## Remediation

- Disable anonymous FTP and remove FTP if it is not required.
- Restrict FTP uploads and prevent uploaded files from being executed by the web server.
- Validate upload types and store untrusted files outside the web document root.
- Remove command execution from web-accessible user input and apply strong input validation.
- Remove the unrestricted Python entry from `sudoers`; grant only narrowly scoped administrative commands.
- Rotate exposed credentials and remove secrets from world-readable scripts.
- Review authentication, upload, process, and sudo logs for indicators of compromise.
- Segment management services and restrict access with firewall rules.

## Vulnerability Report Email

**To:** security@01talent.com
**Subject:** Security Vulnerability Report: Privilege Escalation in Escalator VM

Dear Security Team,

I am writing to report a security vulnerability identified during an authorized educational penetration test against the Escalator VM environment.

**Summary**

A low-privileged `shrek` account can execute `/usr/bin/python3.5` as `root` without a password because of an overly permissive `sudoers` rule. The initial user context can be reached through the VM's exposed and misconfigured services.

**Steps to Reproduce**

1. Discover the VM at `192.168.128.3` using an authorized Host-Only network scan.
2. Enumerate the target and identify HTTP, FTP, and SSH services.
3. Log in to FTP as `anonymous` and upload `exploit.php` to the web-accessible `/files` directory.
4. Use the webshell to obtain a `www-data` shell.
5. Inspect the exposed `.runme.sh` file, recover the `shrek` credentials, and log in through SSH.
6. Run `sudo -l` as `shrek` and observe the `NOPASSWD: /usr/bin/python3.5` rule.
7. Run `sudo /usr/bin/python3.5 -c 'import pty; pty.spawn("/bin/bash")'`.
8. Verify root access with `whoami` and read `/root/root.txt`.

**Impact**

An attacker who reaches the affected user context can obtain complete root control, read sensitive data, alter or destroy system files, establish persistence, and use the system to attack other hosts.

**Proof of Root Access**

The root flag was recovered from `/root/root.txt`:

`01Talent@nokOpA3eToFrU8r5sW1dipe2aky`

![Root flag evidence](screenshots/14-root-txt.png)

Best regards,
Ayman AIT BIHI
Talent, Zone01Oujda

## Ethical Hacking Report

### Authorization

Testing must begin only after obtaining explicit permission from the system owner. The scope, dates, permitted techniques, target addresses, data-handling rules, and emergency contacts should be agreed in writing.

### Legal and Ethical Boundaries

Testing must remain within the authorized scope and use the least disruptive technique that proves the finding. Testers must avoid unrelated systems, unnecessary data access, service disruption, persistence, and destructive actions. Unauthorized testing can be illegal even when a vulnerability is publicly known.

### Responsible Disclosure

Findings should be reported privately to the appropriate stakeholders with clear impact, evidence, reproduction steps, and remediation guidance. Sensitive credentials, flags, and exploit details should be shared only with authorized recipients and stored securely. The tester should allow the owner reasonable time to remediate and should avoid public disclosure that increases risk.

## Verification Checklist

The repository contains the documentation and screenshots needed for review. These items require live verification during the stakeholder demonstration:

- VirtualBox or UTM is installed and the VM launches successfully.
- The VM uses Host-Only or Bridged networking, not NAT or Shared Network mode.
- The target is reachable from the authorized testing machine.
- The documented commands reproduce the user shell, root shell, and root flag.

## Stakeholder Discussion

- **How was the escalation identified?** Service enumeration exposed anonymous FTP and a web-accessible upload path; later `sudo -l` exposed unrestricted root execution of Python.
- **What is the impact?** The chain ends in complete root compromise and loss of confidentiality, integrity, and availability.
- **How should it be fixed?** Disable unnecessary services, secure uploads, rotate exposed credentials, and remove the unrestricted interpreter from `sudoers`.
- **How was testing kept ethical?** Testing was limited to the provided challenge VM, used only to demonstrate the authorized finding, and documented for remediation.
- **How can it be detected?** Monitor anonymous FTP logins, uploads, webshell-like requests, unusual interpreter execution, SSH logins, and sudo events.

## Evidence

The screenshots directory contains the proof collected during the exercise, including:

- host discovery
- network scanning
- web and FTP enumeration
- command execution
- user access
- privilege escalation
- root flag retrieval
