### **Setting Up Syntax Highlighting in SysReptor**  

#### **Edit SysReptor’s CSS File**  
Modify the CSS file to define colors for commands and output:  

```css
.output {
    color: rgb(236, 17, 17);  /* Red for important outputs */
}

.command {
    color: rgb(8, 43, 240);    /* Blue for commands */
}
```

#### **Formatting Commands and Outputs**  
SysReptor requires a delimiter to differentiate between commands and outputs. Use the appropriate format based on whether your command includes pipes (`|`) or not.  

##### **Commands Without Pipes**  
Use `|` as the delimiter:  
```css
non highlight-manual="|" # Set delimiter
|<em><span class="command">|COMMAND HERE|</span></em>|  # Highlighted command
|<em><span class="output">|IMPORTANT OUTPUT HERE|</span></em>|  # Highlighted output
```

##### **Commands With Pipes (`|`)**  
Use `^` as the delimiter:  
```css
non highlight-manual="^"
^<em><span class="command">^COMMAND HERE^</span></em>^
^<em><span class="output">^IMPORTANT OUTPUT HERE^</span></em>^
```

#### **Example Usage**  
The following example applies the correct classes:  
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

### **When to Use `.output` Class?**  
Use `.output` only for **important outputs** such as:  
✔️ Hashes  
✔️ Passwords  
✔️ Indicators of successful exploitation (e.g., confirming user identity)  
