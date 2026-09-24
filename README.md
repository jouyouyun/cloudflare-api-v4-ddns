Cloudflare API v4 Dynamic DNS Update in Bash, without unnecessary requests
Now the script also supports v6(AAAA DDNS Recoards)

----

创建 Cloudflare API 令牌，请转到 https://dash.cloudflare.com/profile/api-tokens 并按照以下步骤操作：

1. 单击创建令牌
2. 为令牌提供一个名称，例如，`cloudflare-ddns`
3. 授予令牌以下权限：
    * 区域 - 区域 - 读取
    * 区域 - DNS - 编辑
4. 将区域资源设置为：
    * 包括 - 特定区域 - 选择你想设置的域名

----
![image.png](https://i.loli.net/2021/11/13/OMpjhUyubrwN6Lk.png)

```
bash ./cf-v4-ddns.sh -k cloudflare-api-token \
 -h host.example.com \     # fqdn of the record you want to update
 -z example.com \          # will show you all zones if forgot, but you need this
 -t A|AAAA                 # specify ipv4/ipv6, default: ipv4
```

`-k` 接收 API Token，不是 Global API Key。也可以通过环境变量 `CF_API_TOKEN` 传入；同时设置时，以 `-k` 为准。

```bash
read -rsp 'Cloudflare API Token: ' CF_API_TOKEN; printf '\n'
export CF_API_TOKEN
bash ./cf-v4-ddns.sh -z example.com -h host.example.com -f true
unset CF_API_TOKEN
```

不要使用 `bash -x` 运行包含真实 Token 的命令，以免认证头泄露到日志。
