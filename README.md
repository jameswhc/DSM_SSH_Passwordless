Synology DSM ssh passwordless
#
[操作本機]
#
>在本機執行 ssh-keygen -t rsa，產生 id_rsa、id_rsa.pub 
>(檔案會存放在 <本機\使用者\帳號\.ssh\> 下)
    
>然後想辧法把 id_rsa.pub上傳至**想控制帳號**的.ssh目錄中
```
    以網頁開啟DSM，以**想控制的帳號**登入，	
    開啟File Station	
    在home目錄下建立 .ssh 目錄	
    將id_rsa.pub 從本地檔案總管<拉>到 .ssh目錄
```    
[操作遠端]
#
>ssh 到伺服器
```
    ssh <管理者帳號> @ <host_IP>
    <管理者密碼>
```
>切換到root模式
```	
    sudo -i
    <管理者密碼>
```
>切換到**想控制帳號**目錄下
	
    `cd /volume1/homes/想控制帳號/.ssh`

>跟原來的authorized_keys串接起來
	
    `cat id_rsa.pub >> authorized_keys`
  
>或者新增一個`authorized_keys`然後再串接
```
    touch authorized_keys
    cat id_rsa.pub >> authorized_keys
```
>改變檔案、資料夾的權限設定
```
    chmod 644 authorized_keys
    cd ..
    chmod 755 .ssh
    cd ..
    chmod 755 <想控制帳號>    !!!!沒看錯! 家目錄也要設
```
>變更SSH Deamon的設定

    `vim /etc/ssh/sshd_config`

>取消下面4項註解
```
    (vim 基本操作->按<i>開始編輯，按<ESC>結束編輯，按<:>進入指令模式，指令模式下按<wq>存檔並離開)
	
    RSAAuthentication yes
    PubkeyAuthentication yes
    AuthorizedKeysFile .ssh/authorized_keys
    ChallengeResponseAuthentication no
```
>重新載入 SSHD
	
    `synoservicectl --reload sshd`
  
完成!可以去試看看囉
#
