# 用于dnspod的Ipv6 ddns脚本 – 沧水的博客
![](https://cangshui.net/wp-content/uploads/2021/03/20210324035457-1024x454.jpg)

目前 ipv6 国内宽带近乎普及，但是很多老脚本并不支持 ipv6 的 ddns，比如我家群晖自带的 ddns 以及华硕路由器中的 ddnspod 插件均不支持

那怎么办，只能自己想办法了，先是网上好不容易搜了个造好的轮子，设置好了居然一直获取 Ipv6 失败，只能自己改改看能不能跑起来，就为这个脚本添加了另一种 ip 获取方法和一些中文注解以及报错提示，应该是没啥问题了

```null
#!/usr/bin/bash    
dnspod_ddnsipv6_id="123456" 
dnspod_ddnsipv6_token="1a2b3c4d5e6f7g8h9i0" 
dnspod_ddnsipv6_ttl="600" 
dnspod_ddnsipv6_domain='baidu.com' 
dnspod_ddnsipv6_subdomain='pan' 
get_ipv6_mode='2' 
local_net="eth0" 














if [ "$dnspod_ddnsipv6_record" = "@" ]
then
  dnspod_ddnsipv6_name=$dnspod_ddnsipv6_domain
else
  dnspod_ddnsipv6_name=$dnspod_ddnsipv6_subdomain.$dnspod_ddnsipv6_domain
fi

die0 () {
    echo "IPv6地址提取错误,无ipv6地址或非公网IP（fe80开头的非公网IP）"
	exit
}

die1 () {  
	echo "IPv6地址提取错误,请使用ip addr命令查看自己的网卡中是否有IPv6公网（非fe80开头）地址，若网卡有IPv6地址却无法获取成功，可尝试在脚本中切换第二种模式获取"
    exit
}

die2 () {
    echo "尝试访问网页http://[2606:4700:4700::1111]/cdn-cgi/trace  查看返回的IPv6地址是否能够正常访问本机，无法访问网页则切换第一种模式获取"
	exit
}

if [[ "$get_ipv6_mode" == 1 ]]
    then
        echo "使用本地网卡获取IPv6"
		ipv6_list=`ip addr show $local_net | grep "inet6.*global" | awk '{print $2}' | awk -F"/" '{print $1}'` || die1
    else
        echo "使用网页接口获取IPv6"
		ipv6_list=$(curl -s -g http://[2606:4700:4700::1111]/cdn-cgi/trace | sed -n '3p' ) || die
        ipv6_list=${ipv6_list##*=}     
    fi






for ipv6 in ${ipv6_list[@]}
do
    if [[ "$ipv6" =~ ^fe80.* ]]
    then
        continue
    else
        echo 获取的IP为: $ipv6 >&1
        break
    fi
done

if [ "$ipv6" == "" ] || [[ "$ipv6" =~ ^fe80.* ]]
then
    die0
fi

dns_server_info=`nslookup -query=AAAA $dnspod_ddnsipv6_name 2>&1`

dns_server_ipv6=`echo "$dns_server_info" | grep 'address ' | awk '{print $NF}'`
if [ "$dns_server_ipv6" = "" ]
then
    dns_server_ipv6=`echo "$dns_server_info" | grep 'Address: ' | awk '{print $NF}'`
fi
    
if [ "$?" -eq "0" ]
then
    echo "你的DNS服务器IP: $dns_server_ipv6" >&1

    if [ "$ipv6" = "$dns_server_ipv6" ]
    then
        echo "该地址与DNS服务器相同。" >&1
    fi
    unset dnspod_ddnsipv6_record_id
else
    dnspod_ddnsipv6_record_id="1"   
fi

send_request() {
    local type="$1"
    local data="login_token=$dnspod_ddnsipv6_id,$dnspod_ddnsipv6_token&domain=$dnspod_ddnsipv6_domain&sub_domain=$dnspod_ddnsipv6_subdomain$2"
    return_info=`curl -X POST "https://dnsapi.cn/$type" -d "$data" 2> /dev/null`
}

query_recordid() {
    send_request "Record.List" ""
}

update_record() {
    send_request "Record.Modify" "&record_type=AAAA&record_line=默认&ttl=$dnspod_ddnsipv6_ttl&value=$ipv6&record_id=$dnspod_ddnsipv6_record_id"
}

add_record() {
    send_request "Record.Create" "&record_type=AAAA&record_line=默认&ttl=$dnspod_ddnsipv6_ttl&value=$ipv6"
}

if [ "$dnspod_ddnsipv6_record_id" = "" ]
then
    echo "解析记录已存在，尝试更新它" >&1
    query_recordid
    code=`echo $return_info  | awk -F \"code\":\" '{print $2}' | awk -F \",\"message\" '{print $1}'`
    echo "返回代码： $code" >&1
    if [ "$code" = "1" ]
    then
        dnspod_ddnsipv6_record_id=`echo $return_info | awk -F \"records\":.{\"id\":\" '{print $2}' | awk -F \",\"ttl\" '{print $1}'`
        update_record
        echo "更新解析成功" >&1
    else
        echo "错误代码返回，域名不存在，请尝试添加。" >&1
        add_record
        echo "添加成功" >&1
    fi
else
    echo "该域名不存在，请在dnspod控制台添加"
    add_record
    echo "添加成功" >&1
fi
```

也可以直接通过 github 下载：[https://raw.githubusercontent.com/CangShui/dnspod-ipv6-ddns/main/dnspod.sh](https://cangshui.net/-otherweb/go?url=aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL0NhbmdTaHVpL2Ruc3BvZC1pcHY2LWRkbnMvbWFpbi9kbnNwb2Quc2g=)

关于脚本的设置注释中已经解释的很详细了，一般来说设置个定时任务就行！

使用定时任务命令：

```null
 crontab -e
```

然后会出现一个文本编辑界面，如果你不会用你就直接照着我的来，填入下列字符

````null
*/5 * * * * bash /home/dnspod.sh > /home/dnspod.log 2>&1 &```

这句话的意思就是每五分钟运行一次/home目录下的dnspod.sh脚本，且将返回值记录到/home/dnspod.log这个文件中，你也可以不记录，去掉>后面这段就行，记得改成你自己的文件路径 
 [https://cangshui.net/4721.html](https://cangshui.net/4721.html)
````
