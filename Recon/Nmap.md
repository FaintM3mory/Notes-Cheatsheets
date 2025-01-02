# Nmap Cheatsheet

### Base Nmap Syntax
```
nmap [Scan Type(s)] [Options] [Target(s)]
```

### Scan Types

| **Scan Type** | **Command** | **Description** |
| -------------- | -------------- | -------------- |
| TCP Connect | `-sT` | Complete TCP 3-way handshake; less stealthy. |
| SYN Scan | `-sS` | Half-open scan; stealthier. |
| UDP Scan | `-sU` | Scans for open UDP ports. |
| Ping Scan | `-sn` | Detect live hosts without port scanning. |
| ACK Scan | `-sA` | Maps out firewall rules. |
| Window Scan | `-sW` | Uses TCP window size to determine open ports. |
| Maimon Scan | `-sM` | Detects firewalls; sends FIN/ACK probe. |
| FIN Scan | `-sF` | Sends a FIN packet to detect open ports. |
| Null Scan | `-sN` | Sends no TCP flags; used to bypass some firewalls. |
| Xmas Scan | `-sX` | Sends FIN, PSH, URG flags; bypasses some firewalls. |
| IP Protocol Scan | `-sO` | Scans for supported IP protocols. |
| Idle Scan | `-sI <Zombie>` | Uses a zombie host to cloak the scan. |

### Common Options

| **Option** | **Command** | **Description** |
| -------------- | -------------- | -------------- |
| Target IP/Range | `<IP>/<CIDR>` | Specify single or multiple targets |
| Host Discovery | `-Pn` | Skip host discovery; scan all specified IPs. |
| OS Detection | `-O` | Perform OS detection. |
| Service Version Detection | `-sV` | Detect versions of services running on ports. |
| Aggressive Scan | `-A` | OS Detection, version detection, script scanning, and traceroute. |
| Scan Timing | `-T[0-5]` | Set timing template (0=paranoid, 5=aggressive). |
| Output File | `-oN/-oX/-oG/-oA` | Save output (Normal/XML/Grepable/AII). |

### Port Specification

| **Option** | **Command** | **Description** |
| -------------- | -------------- | -------------- |
| Specific Ports | `-p <ports>` | Scan specific ports (e.g., `-p 22,80,443`). |
| Port Range | `-p <range>` | Scan port ranges (e.g., `-p 1-65535`). |
| Top Ports | `--top-ports <n>` | Scan the top `n` most common ports. |
| All Ports | `-p-` | Scan all 65 535 ports. |

### Script Scanning

| **Option** | **Command** | **Description** |
| -------------- | -------------- | -------------- |
| Default Scripts | `-sC` | Use default scripts. |
| Specify Scripts | `--script=<name>` | Run specific script(s). |
| Script Categories | `--script=<category>` | Use categories like `auth`, `default`, `exploit`, etc. |
| Arguments for Scripts | `--script-args=<args>` | Pass arguments to scripts. |

### Advanced Scans

| **Option** | **Command** | **Description** |
| -------------- | -------------- | -------------- |
| Fragmentation | `-f` | Send fragmented packets; evades some firewalls. |
| Spoof IP Address | `-S <IP>` | Use a fake source IP address. |
| Decoy Scanning | `-D <IP1,IP2,...>` | Perform scan using decoy IPs. |
| MTU | `--mtu <value>` | Specify custom MTU for packets. |
| Randomize Targets | `--randomize-hosts` | Randomize the scan order of target hosts. |


### Scanning Examples

```shell
# Fast Full Scan
nmap -sS -sV -p- -T4 &TARGET 

# Live Host Discovery
nmap -sn &TARGET 

# Enumerating SMB (Shares/Vulns/Users)
nmap -p445 --script=smb-enum-shares,smb-enum-users,smb-vuln-* -T4 $TARGET

# Quick Focused Web Server Scan
nmap -sS -p80,443 --script=http-enum,http-vuln-* -T4 $TARGET 

# FTP Anonymous Login Check & Directory Listing
nmap -p21 --script=ftp-anon,ftp-ls -T4 $TARGET 

# Default Credentials on Web Interfaces
nmap -p80 --script=http-default-accounts $TARGET 

# DNS Zone Transfer
nmap --script=dns-zone-transfer -p53 $TARGET 

# DNS Service Discovery
nmap -p53 --script=dns-service-discovery $TARGET 

# Comprehensive Vulnerability Scan
nmap -sS -sV --script=vulscan,vulners -T4 $TARGET 

# Decoys & Fragmented packets for Evasion
nmap -sS -sV -p22,80,443 -D RND:10 -f -T3 $TARGET 

# Zombie Idle Scan 
nmap -sI $ZOMBIE -p80,443 $TARGET

# Specific Target Protocol & Timing
nmap -p22,80,443 -sS -sV --script=http-enum --max-retries 2 --scan-delay 100ms $TARGET
```

