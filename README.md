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

## Evidence

The screenshots directory contains the proof collected during the exercise, including:

- host discovery
- network scanning
- web and FTP enumeration
- command execution
- user access
- privilege escalation
- root flag retrieval
