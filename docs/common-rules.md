# 通用规则与其他 Clash 客户端

[返回首页](../README.md)

本页针对需要内核分流的情形；桌面浏览器使用系统代理且不需要 TUN 时，先看[首页的系统代理绕过](../README.md#方法一clash-verge-rev--windows-系统代理)。Clash / Mihomo 内核的路由规则可表达相同的校园直连意图；不同客户端保存本地自定义规则的菜单和方式不同。优先使用客户端提供的前置规则、附加规则或持久化扩展功能。

## 最小校园规则

在已有 `rules:` 列表的前部加入这一项，保留其余规则：

```yaml
rules:
  - DOMAIN-SUFFIX,bjut.edu.cn,DIRECT
  # 下面继续保留你原来的规则和最终 MATCH
```

这只是结构示意。可直接复制的[示例文件](../examples/campus-rules.yaml)同样是配置片段，**不能单独替代包含节点等内容的完整订阅**。如果编辑器已经有 `rules:`，只增加列表项，不再创建第二个同名顶层键。

`DOMAIN-SUFFIX,bjut.edu.cn` 按域名边界匹配根域名和子域名，不会把 `bjut.edu.cn.example.com` 也当成学校域名。规则按从上到下的顺序匹配，新增项放在可能提前匹配它的规则及最终 `MATCH` 之前，客户端模式选“规则”。

## 可选：直接访问私有 IP

校园资源如果通过私有 IP 字面地址访问，可按需要增加以下标准私有 IPv4 网段。它们涵盖家庭、校园、企业等私网，**不代表 BJUT 专属网段**；不要仅为一个地址就盲目扩大范围。

```yaml
- IP-CIDR,10.0.0.0/8,DIRECT,no-resolve
- IP-CIDR,172.16.0.0/12,DIRECT,no-resolve
- IP-CIDR,192.168.0.0/16,DIRECT,no-resolve
```

`no-resolve` 避免仅为这条 IP 规则触发额外域名解析；它不会关闭整个请求的 DNS。已知单个目标时，可使用该目标的 `/32` 精确规则；不要将登录截图或个人设备的内网地址直接发到公共仓库。

这些规则同样要求本机原有网络能到达目标。校外没有学校通道时，把私有 IP 设为 `DIRECT` 仍无法进入校园网。

## `.cn` 和“国内网站”有何区别？

| 你想匹配什么 | 规则 |
|---|---|
| 学校根域名及全部子域名 | `DOMAIN-SUFFIX,bjut.edu.cn,DIRECT` |
| 只匹配一个完整主机名 | `DOMAIN,my.bjut.edu.cn,DIRECT` |
| 全部以 `.cn` 结尾的域名 | `DOMAIN-SUFFIX,cn,DIRECT` |
| `cn.overleaf.com` 这个主机 | `DOMAIN,cn.overleaf.com,DIRECT` |

`.cn` 后缀规则不等于国内服务全集。主机名中的 `cn` 前缀不改变 `.com` 后缀。`GEOSITE` / `GEOIP` 使用域名或 IP 数据库，范围和依赖也不同；本教程不要求新增数据库或替换现有国内分流体系。

## 其他客户端怎么用？

1. 找到当前配置的本地自定义规则入口，模式选择规则。
2. 添加上面的校园域名直连项，优先放到原订阅规则之前。
3. 若客户端只能直接编辑文件，复制一份本地配置进行管理；更新节点时合并自己的规则。不要期待远程订阅保留你对下载文件的手动修改。
4. 重载配置，确认无错误，检查新连接的实际出口；再更新一次订阅验证持久性。

传统 Clash for Windows / Clash for Android 等客户端可从[社区客户端教程导航](https://github.com/clashbk/clash/wiki/)查对应界面。社区页有旧版内容和推广，规则含义以[内核官方文档](https://wiki.metacubex.one/config/rules/)为准。

官方来源：[路由规则](https://wiki.metacubex.one/config/rules/)、[GitHub 文档源文件](https://github.com/MetaCubeX/Meta-Docs/blob/main/docs/config/rules/index.md)、[出站模式](https://wiki.metacubex.one/config/general/)。
