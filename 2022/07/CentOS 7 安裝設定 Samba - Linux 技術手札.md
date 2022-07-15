# CentOS 7 安裝設定 Samba - Linux 技術手札
Samba 可以讓 Linux 的檔案及印表機以 “網路上的芳鄰” 分享給 Windows 電腦, 以下是在 RHEL 及 CentOS 7 安裝 Samba 的步驟:

用 YUM 安裝 Samba 及其相關套件:

`# yum install samba samba-client samba-common -y`

安裝好 Samba 後, 開啟 Samba 的設定檔 /etc/samba/smb.conf, 加入以下內容:

    [global]
    workgroup = MYGROUP
    server string = Samba Server %v
    netbios name = centos_7
    security = user
    map to guest = bad user
    dns proxy = no
     
    [Secure]
    path = /home/share_dir
    valid users = @smbgrp
    guest ok = no
    writable = yes
    browsable = yes

儲存檔案後啟動 Samba, 及設定開機自動啟動:

```shell
# mkdir -p /samba/anonymous  
# systemctl enable smb  
# systemctl enable nmb  
# systemctl restart smb  
# systemctl restart nmb
```

執行以下指令開啟 Samba 對外開放及其權限:

    # firewall-cmd –permanent –zone=public –add-service=samba
    # firewall-cmd –reload
    # mkdir /home/share_dir
    # chown -R smb_user:smbgrp /home/share_dir
    # chmod -R 0770 /home/share_dir
    # chcon -t samba_share_t /home/share_dir

建立連接到 Samba 的帳號, 下面會建立 smb_user 帳號, 然後用 smbpasswd 設定密碼:

    # useradd smb_user
    # groupadd smbgrp
    # usermod -a -G smbgrp smb_user
    # smbpasswd -a smb_user

CentOS 7 的設定完成後, 在 Windows 裡面去到 “執行”, 輸入 “\\centos_7” (這個名稱是在 smb.conf 設定的 netbios name), 如果要進入 “share_dir” 目錄, 輸入上面用 smbpasswd 設定的密碼就可以連接了。 
 [https://www.ltsplus.com/linux/centos-7-install-samba](https://www.ltsplus.com/linux/centos-7-install-samba)
