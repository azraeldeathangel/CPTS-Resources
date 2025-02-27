### **Important**  
- **Keep Internal and External Walkthroughs Separate** – Document internal and external compromises separately.  
- **Tell a Story (Use Third-Person)** – Explain why findings matter, not just the steps taken. Focus on impact.  (e.g the tester exploited X Y) 
- **Provide Clear Evidence (Redact Properly)**:  
  - Skip unnecessary details like tool versions and bash prompts.  
  - Use **large screenshots** for clarity, especially on SysReptor.  
  - Redact hashes as `XXX` and passwords as `<REDACTED>`.  
- **Highlight Key Information**:  
  - Use **bold** for important details like usernames, ACLs, and groups.  
  - **Color Code:** Commands in **blue**, critical output (e.g., plaintext credentials) in **red** kindly view Reporting_Highlighting.md  
- **Prefer `curl` When Possible** – It keeps evidence cleaner and easier to read, especially for command injection.  
- **Label Figures Clearly** – Number all screenshots (e.g., **Figure 1: Extracting Credentials from Registry Hives**).  
- **Use Bullet Points Instead of Numbers** – Makes reports easier to read.  
- **Simplify Command Output** – Keep only essential parts, removing unnecessary bash prompts (except `$`).  
- **Explain Why an Endpoint is Vulnerable** – Don’t just list vulnerabilities; clarify why they are exploitable (e.g., specific parameters like `id`).  

### **Minor**  
- **Keep Formatting Consistent** – Standardize capitalization and punctuation.  
- **Ensure Clear Writing** – Keep sentences structured and easy to understand.  

### **Tips & Tricks**
- **[nmap2md.py](https://github.com/vdjagilev/nmap2md)** - For Nmap outputs, you can use nmap2md.py, a Python tool that converts Nmap XML output to Markdown, so you won’t have to manually format it.  
