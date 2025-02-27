## Initial Foothold

#### Footprinting
 **Services to check**
 
- FTP, SMB, SMTP, IMAP, MSSQL: Attempt anonymous authentication, null authentication, or   reuse credentials if found
    - WinRM: Utilize evil-winrm for credential reuse.
    - MSSQL: Exploit with mssqlclient.py.
    - SSH: Check for weak or reused credentials.
    - NFS/RSync: Look for mountable and accessible shares.

### Active Directory Exploitation
#### Initial Enumeration
- **Enumerate Usernames**  
    - Use tools like `rpcclient`, `enum4linux`, or attempt anonymous/null authentication to enumerate users. You can also try `kerbrute` or `netexec` to find valid usernames.
- **Valid Username**  
    - Perform password spraying using `kerbrute` or `netexec` with **null** (" "), **username as pass**, and **common** passwords {**Welcome1**,**Passw0rd**,**Winter2022!**}.
	- `Netexec` smb -u users.txt -p users.txt --continue-on-success
- **Valid Credentials**  
    - If you find valid credentials, use the `--users` and `--rid` flag on `netexec` to enumerate more usernames.  
    -  Check for Kerberoasting / ASREPRoasting
    - Perform local admin spraying with the found password or NTLM hash.
    - Spray already found passwords on domain users.

- **Can't Acquire Any Passwords but Have Users?**  
    Try ASREPRoasting using `GetNPUsers.py` to target users with `DONT_REQ_PREAUTH` set (syntax:  `GetNPUsers.py domain.local/ -dc-ip $IP -usersfile users.txt` ) 

#### Found Credentials or Hashes?
- **BloodHound / SharpHound**  
    - Run BloodHound to map Active Directory environment.
- **Password Spraying**
	- Perform Password Spraying on the whole network subnet once you got a password `/24` using netexec
- **Impacket secretsdump & GetUserSPNs.py**
	- Try running `secretsdump.py` blindly to see if the user can perform DCSync or you can also try running `GetUserSPNs.py` to see if there's any kerberoastable users
- **Netexec**  
    - ==**Re-enumerate shares**==, look for credentials on shared directories or `SYSVOL` for gpp and logon scripts.  
    - Re-enumerate valid users and groups.
    -  Use LNK file attack on writable shares with `Netexec -M slinky` and `LNKBomb`
- **PowerView**  
    - Enumerate user privileges and ACLs if Bloodhound is lacking.
    - Can we change a user's password?
    - Can we add an SPN to a user for kerberoasting?
    - Can we add ourselves to a group?
    - Can we do DCSync?
- **Shell Access**  
    **==Re-check shell access==** on **SMB, WinrRM,  RDP**  using `evil-winrm` `netexec` `smbexec` `wmiexec` `psexec` or maybe just running as another user via `runas` command.

#### Misconfigurations
- **Weak Passwords or Misconfigurations**  
    - Check if any users have passwords in their description.  
    - Look for users with weak or no passwords (`PASSWD_NOTREQD`).  
    - Try extracting Group Policy Preferences (GPP) passwords using `netexec -M gpp_password`
---

#### Post Exploitation
- **Ping sweep**
	- Perform a ping sweep to identify more hosts for lateral movement, make sure to also check if there's other network interfaces on the network.

- **Dump Credentials**
	- Use `Mimikatz`, `laZagne`,`Netexec` to dump LSASS, SAM, DPAPI, NTDS.dit and more.

- **Internal Password Spraying**
	- Once we have internal access on the AD environment we can also try `Invoke-DomainPasswordSpray` to spray for passwords.

- **Share Hunting**
	- We can also run `Snaffler` to enumerate for juicy shares that might contain sensitive credentials and lead us to lateral movement.

- **Targeted Kerberoasting**
	- Use `GetUserSPNs.py` or `targetedkerberoast.py` on Linux or create an SPN to attempt kerberoasting if you have sufficient privileges.

- **DCSync**
	- Once you have DCSync rights, use `Mimikatz` or `secretsdump` for DCSync to acquire all user credentials. Check for misconfigured users with DS-Replication ACLs.

- **Inveigh**
	- After gaining admin privs on a machine run Inveigh to potentially capture NTLMv2 hashes

#### Exploits
- Attempt privilege escalation using known exploits such as NoPAC, PrintNightmare, or PetitPotam.

#### Lateral Movement
- **ExtraSIDs Attack**
	- Use this method to escalate privileges from a Parent-Child domain relationship.
- **Domain Trusts Mapping**  
    - Map domain trusts with BloodHound or PowerView.  
    - Check if you can DCSync and move to a Golden Ticket attack.
- **Cross-Forest Kerberosting**  
    - Attempt to kerberoast users on other domains if you're unable to leverage your rights in the current domain.  
    - Reuse credentials for lateral movement to accounts in other domains.
- **Enumerate SPNs in Other Domains**  
    - Use Rubeus or GetUserSPNs to kerberoast users on other domains.
