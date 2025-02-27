### General rules
- **Separate Internal and External Walkthroughs**: Maintain separate documentation for internal and external compromise walkthroughs.
- **Tell a Story (Always Use Third-Person Perspective)**: Explain the significance of your findings, not just the steps taken. Focus on the impact.
- **Provide Clear Evidence (Redact Properly)**:
    - Avoid unnecessary details like tool versions and bash prompts.
    - Use **large screenshots** for better clarity, especially on SysReptor.
    - Redact hashes using `XXX` and passwords using `<PASSWORD REDACTED>`.
- **Highlight & Bold Relevant Information**:
    - Use **bold** for important details like usernames, ACLs, and groups.
    - **Color Code:** Highlight commands in **blue** and critical output (e.g., discovered plaintext credentials) in **red**.
- **Use `curl` Whenever Possible**: `curl` produces cleaner and more readable evidence, especially for vulnerabilities like command injection.
- **Label Figures Properly**: Number all screenshots (e.g., **Figure 1: Extracting Credentials from Registry Hives**).
- **Use Bullet Points Instead of Numbers**: Improves readability and keeps reports neater.
- **Remove Bash Prompts (Except `$`)**: Commands and outputs are the focus—omit unnecessary prompts like `kali@kali:`.
- **Explain Why an Endpoint is Vulnerable**: Don't just list vulnerable endpoints—clarify what makes them exploitable (e.g., `id` specific parameters).
- **Maintain Consistency**: Standardize capitalization and punctuation.
- **Ensure Proper Formatting**: Keep sentences clear and structured for better readability.

### ChatGPT Prompts
#### Start
Good day ChatGPT, you're familiar with Sysreptor right?
SysReptor is a fully customizable pentest reporting platform designed for penetration testers, red teamers, and other cybersecurity professionals. Simplify, customize, and automate your pentest reports with ease.

#### Root cause / Description
I found [Vulnerability, Misconfiguration], What is the general root cause (description of this vulnerability, paragraph format please also dont start with 'The root cause' since my reporting would look redundant) make it clear and professional but dont make it overly formal

#### Impact 
What are the impacts of vulnerability/misconfiguration in paragraph format make it clear and professional but not overly formal.

#### Remediation
How can I remediate this vulnerability/misconfiguration please use bulletpoints and highlight in a format like this make it clear and professional but not overly formal.
- **Remediation name**: description

### Blue/Red Report highlighting
If you're going to look at HackTheBox Report draft you'll see the command in **blue** color and important outputs like hash, passwords in **red** color, so how do we do that in sysreptor?

First off edit edit your report CSS file and put this
**CSS file**
```css
.output {
    color: rgb(236, 17, 17);  
}

.command {
    color: rgb(8, 43, 240);    
}
```

**Commands with no pipes**
```css
non highlight-manual="|" # this is the delimiter
|<em><span class="command">|COMMAND HERE|</span></em>| # Commands
|<em><span class="output">|IMPORTANT OUTPUT HERE|</span></em>|  # Output
```

**Commands with pipes**
```css
non highlight-manual="^"
^<em><span class="command">^ ^</span></em>^
^<em><span class="output">^ ^</span></em>^
```

This is the sample usage of the code snippets that I provided
```non highlight-manual="^"
┌──(kali㉿kali)-[~]
└─$ ^<em><span class="command">^sqlmap -r req.txt^</span></em>^

Database: status
Table: users
[2 entries]
+----+-----------------------------------+----------+
| id | password                          | username |
+----+-----------------------------------+----------+
| 1  | 4528342e54d6f8f8cf15bf6e3c31bf1f6 | Admin    |
^<em><span class="output">^| 2  | 1fbea4df249xxxxxxxxxxda387eb297cf | Flag     |^</span></em>^
+----+-----------------------------------+----------+
```


