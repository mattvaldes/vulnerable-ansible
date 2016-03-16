# vulnerable-ansible

1. Add these files to your ansible directory.
2. Update the *ansible/hosts* file. (Playbooks are configured for Debian hosts)
2. Run the broken playbook
` $ ansible-playbook broken.yml`
3. Note the services running on the target host. Be aware, anonymous FTP upload is enabled!
```
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:6379            0.0.0.0:*               LISTEN      1007/redis-server
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      964/nginx       
tcp        0      0 0.0.0.0:21              0.0.0.0:*               LISTEN      1046/vsftpd     
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      2593/sshd       
tcp        0      0 192.168.1.104:22       10.0.0.X:60258    ESTABLISHED 1094/0          
tcp6       0      0 :::22                   :::*                    LISTEN      2593/sshd       
```
4. Run vulnerability scan against the host
```
* Anonymous FTP is enabled.
* nginx webserver is displaying an old version in the banner
* Redis server is publicly available.
```
5. Run the fixed playbook
` $ ansible-playbook fixed.yml`
6. Note the services running on target host. Anonymous FTP is no longer enabled.
```
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:6379          0.0.0.0:*               LISTEN      14932/redis-server
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      14889/nginx     
tcp        0      0 0.0.0.0:21              0.0.0.0:*               LISTEN      14971/vsftpd    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      2600/sshd       
tcp        0      0 192.168.1.104:33627    10.0.0.X:80       TIME_WAIT   -               
tcp        0      0 192.168.1.104:22       10.0.0.X:53549    ESTABLISHED 13260/0         
tcp6       0      0 :::22                   :::*                    LISTEN      2600/sshd      
```
7. Re-run vulnerability scan.
```
* Anonymous FTP is disabled.
* nginx webserver is displaying a current version in the banner
* Redis server is no longer publicly available.
```
