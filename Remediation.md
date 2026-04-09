# Remediation

This file outlines the actions that should be taken to fix or mitigate the vulnerabilities identified during the exercise.

## Service Exposure

Only required services should be exposed on the network. Unnecessary services increase the attack surface and make enumeration easier for an attacker. If FTP is not essential, it should be disabled. If it must remain enabled, access should be restricted and monitored.

## File Upload Security

Uploaded files should never be placed in locations where they can be executed by the web application or server. Upload directories should use strict permissions, and the application should validate file types and treat uploaded content as untrusted.

## Web Application Hardening

The application should be reviewed to ensure that user-controlled input cannot be used to trigger command execution. Any functionality that processes files or system commands should be restricted, sanitized, and isolated.

## Sudo Configuration

The `sudoers` configuration should be reviewed immediately. Users should not be allowed to run interpreters such as Python with elevated privileges unless there is a very specific and controlled business need. Least-privilege access should be enforced for all accounts.

## Monitoring and Auditing

Administrative actions, file uploads, authentication events, and privilege escalation attempts should be logged and reviewed regularly. Ongoing auditing helps identify misconfigurations before they can be abused.