## Initial Foothold

### Footprinting
**Services to check**  
- **FTP, SMB, SMTP, IMAP, MSSQL**: Attempt anonymous/null authentication or credential reuse.  
- **WinRM**: Use `evil-winrm` for credential reuse.  
- **MSSQL**: Exploit with `mssqlclient.py`.  
- **SSH**: Check for weak or reused credentials.  
- **NFS/RSync**: Look for mountable and accessible shares.  

---

## Active Directory Exploitation

### Initial Enumeration
- **Enumerate Usernames**  
  - Use `rpcclient`, `enum4linux`, `kerbrute`, or `netexec` to find valid users.  
- **Valid Username**  
  - Perform password spraying using `kerbrute` or `netexec` with:
    - **null password**, **username as password**, **common passwords** (e.g., `Welcome1`, `Passw0rd`, `Winter2022!`).  
  - `netexec smb -u users.txt -p users.txt --continue-on-success`
- **Valid Credentials**  
  - Use `netexec --users --rid` to enumerate more usernames.  
  - Check for **Kerberoasting** / **ASREPRoasting**.  
  - Spray found passwords on domain users.  
- **Can't Acquire Any Passwords but Have Users?**  
  - Run `GetNPUsers.py` to ASREPRoast users without pre-authentication.  
    ```sh
    GetNPUsers.py domain.local/ -dc-ip $IP -usersfile users.txt
    ```

---

### Found Credentials or Hashes?
- **BloodHound / SharpHound**: Map AD environment, check for kerberoastable users, misconfigured ACLs. 
- **Password Spraying**: Spray passwords across `/24` network using `netexec`.  
- **Netexec**  
  - Re-enumerate shares, search for credentials in **SYSVOL, GPP, logon scripts**.  
  - Use LNK file attack on writable shares (`netexec -M slinky`, `LNKBomb`).  
- **PowerView**  
  - Enumerate your ACL privileges:  
    - Can you change a user's password?  
    - Can you add an SPN to a user?  
    - Can you add yourself to a group?  
    - Do you have DCSync rights?  
- **Shell Access**  
  - Re-check shell access via **SMB, WinRM, RDP** using:  
    - `evil-winrm`, `netexec`, `smbexec`, `wmiexec`, `psexec`, or `runas`.  

---

### Misconfigurations
- **Weak Passwords / Misconfigurations**  
  - Check if passwords are stored in **user descriptions**.  
  - Look for accounts with **PASSWD_NOTREQD**.  
  - Extract **GPP passwords** with:  
    ```sh
    netexec -M gpp_password
    ```

---

## Post Exploitation
- **Ping Sweep**  
  - Identify more hosts for lateral movement.  
  - Check for multiple network interfaces.  
- **Dump Credentials**  
  - Use `Mimikatz`, `laZagne`, or `netexec` to dump **LSASS, SAM, DPAPI, NTDS.dit**.  
- **Internal Password Spraying**  
  - Run `Invoke-DomainPasswordSpray` to test against domain users.  
- **Credential Hunting**  
  - Always run `tree` on compromised machines to find stored credentials.  
- **Inveigh**  
  - Capture NTLMv2 hashes after gaining admin privileges.  
---

## Exploits
- Attempt privilege escalation using:  
  - **NoPAC**  
  - **PrintNightmare**  
  - **PetitPotam**  

---

## Lateral Movement
- **ExtraSIDs Attack**  
  - Escalate privileges in a **Parent-Child domain** relationship.  
- **Domain Trusts Mapping**  
  - Use **BloodHound / PowerView** to analyze trust relationships.  
- **Cross-Forest Kerberoasting**  
  - Test for Kerberoastable users in **other domains**.  
  - Reuse credentials across **domain trusts**.  
- **Enumerate SPNs in Other Domains**  
  - Use **Rubeus** or `GetUserSPNs.py` for cross-domain Kerberoasting.  
