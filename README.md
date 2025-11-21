Zero Trust Architecture (ZTA) Host Defense Implementation

I. Project Summary

This repository documents the implementation of a host-based Zero Trust Defense mechanism on a Linux server. The primary objective is to enforce the "Never Trust, Always Verify" principle directly at the host level. This defense system integrates the Uncomplicated Firewall (UFW) with Fail2Ban to create a dynamic, self-remediating security perimeter that automatically blocks unauthorized entities.

The goal is to move beyond conventional perimeter security by enforcing strict micro-segmentation and least-privilege access on the host. This measure effectively mitigates brute-force attacks and prevents unauthorized internal network movement.

II. Technical Implementation

The host defense system operates via a robust, closed-loop security mechanism. Policies are not static but are subject to immediate modification based on threat intelligence derived from server logs.

Component

Functional Role

ZTA Principle Applied

UFW

Host Firewall configured with a default DENY policy. Only necessary services (e.g., SSH, HTTP) are explicitly permitted from validated source addresses.

Least Privilege Access, Micro-segmentation

Fail2Ban

Log monitoring service that tracks authentication and access logs for repeated login failures, indicating brute-force attempts.

Continuous Verification, Device Authorization

Dynamic Action

Upon meeting a failure threshold, Fail2Ban instantaneously inserts a temporary DENY rule into UFW, directly targeting the source IP address of the aggressor.

Assumption of Breach, Automated Response

A. Configuration Files

config-files/user.rules: Contains the foundational static UFW directives: the main traffic rejection policy and specific ALLOW rules for designated trusted services and IP ranges.

config-files/jail.local: Defines the operational parameters for Fail2Ban, including the duration of bans (bantime), the maximum retry attempts (maxretry), and the list of administrative IP addresses to ignore (ignoreip).

III. Architectural Diagram

A high-level Architecture Diagram is provided in the documentation folder to visually represent the security enforcement process. This schematic details the automated feedback loop:

A hostile entity initiates a breach attempt.

The Fail2Ban service detects the signature of persistent brute-forcing from server logs.

The pre-configured Fail2Ban action is executed.

UFW instantaneously applies a new DENY rule targeting the attacker's IP.

Traffic from the prohibited source is immediately blocked at the host level.

IV. Enterprise Scalability (Ansible)

This host-based implementation can be seamlessly scaled across numerous servers using Configuration Management (CM) tools, specifically Ansible.

The use of Ansible facilitates:

Rule Automation: Manual configuration of UFW and Fail2Ban is replaced by centralized ZTA policy definition within a single Ansible playbook.

Uniform Deployment: Ansible ensures consistent propagation and enforcement of security rules across the entire server collective, guaranteeing uniform Micro-Segmentation and robust Defense-in-Depth.