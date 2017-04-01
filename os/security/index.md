---
title: RancherOS security
layout: os-default

---

## RancherOS security
---


<table width="100%">
<tr style="vertical-align: top;">
<td width="30%" style="border: none;">
<h4>Security policy</h4>
<p style="padding: 8px">Rancher Labs supports responsible disclosure, and endeavours to resolve all issues in a reasonable time frame. RancherOS is a minimal Linux distribution, built with entirely using open source components.</p>
</td>
<td width="30%" style="border: none;">
<h4>Reporting process</h4>
<p style="padding: 8px">Please submit possible security issues by emailing <a href="security@rancher.com">security@rancher.com</a></p>
</td>
<td width="30%" style="border: none;">
<h4>Announcments</h4>
<p style="padding: 8px">Subscribe to the <a href="https://forums.rancher.com/c/announcements">Rancher announcements forum</a> for release updates.</p>
</td>
</tr>
</table>

### RancherOS Vulnerabilities

| ID | Description | Date | Resolution |
|----|-------------|------|------------|
| [CVE-2017-6074](http://seclists.org/oss-sec/2017/q1/471) | Local privilege-escalation using a user after free issue in [Datagram Congestion Control Protocol (DCCP)](https://wiki.linuxfoundation.org/networking/dccp). DCCP is built into the RancherOS kernel as a dynamically loaded module, and isn't loaded by default. | 17 Feb 2017 | [RancherOS v0.8.1](https://github.com/rancher/os/releases/tag/v0.8.1) using a [patched 4.9.12 Linux kernel](https://github.com/rancher/os-kernel/releases/tag/v4.9.12-rancher) |


