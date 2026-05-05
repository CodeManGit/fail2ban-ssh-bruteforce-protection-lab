# Fail2Ban SSH Brute-Force Protection Lab

## Overview

This lab demonstrates how to install, configure, and test Fail2Ban on Ubuntu 24 to protect SSH from brute-force login attempts. The lab shows how Fail2Ban monitors SSH authentication logs, detects repeated failed login attempts, bans suspicious IP addresses, and allows manual unbanning.

## Lab Objective

The goal of this was to:

- Install and configure Fail2Ban
- Protect SSH from brute-force attacks
- Simulate failed SSH login attempts
- Observe IP banning behavior
- Review Fail2Ban logs
- Manually unban an IP address

## Tools and Environment

- Ubuntu 24
- Fail2Ban
- OpenSSH Server
- Linux terminal
- sudo/root access

## Key Skills Demonstrated

- Linux service management using systemctl
- SSH security hardening
- Log monitoring and analysis
- Brute-force attack mitigation
- Fail2Ban jail configuration
- Basic intrusion prevention concepts
- Troubleshooting security tools

## Installation Steps

Fail2Ban was installed using:

sudo apt update
sudo apt install fail2ban -y

The service was enabled and started with:
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
sudo systemctl status fail2ban

##Configuration

The default Fail2Ban jail configuration was copied to a local configuration file:
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local 

The SSH jail was configured with the following settings:
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = %(sshd_log)s
bantime = 3600
findtime = 600
maxretry = 3
ignoreself = false

Configuration Explanation
enabled = true turns on SSH protection
maxretry = 3 bans an IP after 3 failed login attempts
findtime = 600 means attempts must occur within 10 minutes
bantime = 3600 bans the IP for 1 hour
logpath = %(sshd_log)s tells Fail2Ban where to look for logs
ignoreself = false allows testing on localhost

##Testing the Lab

To simulate a brute-force attack, SSH into the system and enter the wrong password multiple times:
ssh username@127.0.0.1

After several failed attempts, check the SSH jail status:
sudo fail2ban-client status sshd
Fail2Ban will show failed attempts and any banned IP addresses.

##Viewing Logs

sudo cat /var/log/fail2ban.log

SSH authentication logs:
cat /var/log/auth.log
These logs confirm failed login attempts and banning activity.

##Unbanning an IP
To remove a banned IP:
sudo fail2ban-client set sshd unbanip 127.0.0.1

Verify:
sudo fail2ban-client status sshd

##What I Learned
Through this lab, I learned how Fail2Ban helps protect Linux systems from brute-force attacks by monitoring logs and automatically banning suspicious IP addresses. I also learned how to configure SSH jail settings, review logs, troubleshoot Fail2Ban issues, and manually unban IP addresses.

##Limitations
Fail2Ban is effective but not a complete security solution. It depends on log files, so if logs are not working properly, it may not detect attacks. Attackers can bypass it by using slow login attempts or multiple IP addresses. It should be combined with other controls like SSH keys, firewalls, and rate limiting.

##Conclusion
This lab demonstrated how Fail2Ban improves SSH security by detecting repeated failed login attempts and banning the source IP. It provides an effective layer of defense against brute-force attacks when combined with other security practices.


