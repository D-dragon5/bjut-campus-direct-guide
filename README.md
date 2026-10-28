# 开着 Clash，也能正常访问校园网

**以北京工业大学（BJUT）为例：让校园流量走本地网络，其他流量继续按原来的代理规则处理。**

适用：Clash Verge Rev、FlClash，以及使用 Clash / Mihomo 规则的客户端。本文整理现成官方资料，补充 BJUT 案例；原教程见[资料索引](docs/references.md)。整理日期：2026-10-28。

## 先选你的情况

| 你的情况 | 从这里开始 |
|---|---|
| Windows + Clash Verge Rev，使用系统代理，TUN 关闭 | [方法一：在代理绕过末尾加校园域名](#方法一clash-verge-rev--windows-系统代理) |
| Clash Verge Rev 开启了 TUN，或想统一按规则处理 | [方法二：校园域名设为 DIRECT](#方法二clash-verge-rev--规则分流含-tun) |
| FlClash 桌面端或 Android | [FlClash：添加校园直连规则](docs/flclash.md) |
| 其他 Clash / Mihomo 客户端、手动 YAML 配置 | [通用规则及其他客户端](docs/common-rules.md) |
| 规则加了仍然打不开 | [排查表](docs/troubleshooting.md) |

## 访问内网的前提

**设备需要先具备校园资源的访问条件。** 在校内接入可访问对应资源的校园网络；人在校外时，按学校说明使用学校 WebVPN 或其他授权入口。

学校教务处公开列出了两种入口：[校内综合门户](https://my.bjut.edu.cn)与[校外 WebVPN](https://webvpn.bjut.edu.cn)。见[教务处通知的“选课方式”](https://undergrad.bjut.edu.cn/info/1213/3603.htm)；[2026 年校方通知](https://job.bjut.edu.cn/info/1048/4457.htm)也说明门户校外访问需经学校 VPN。

| 地址 | 本教程中的用途 |
|---|---|
| `https://my.bjut.edu.cn` | 校内综合门户示例；校外按学校入口访问 |
| `https://webvpn.bjut.edu.cn` | 学校校外访问入口；需本人具备相应账号权限 |
| `https://www.bjut.edu.cn` | 公开学校主页，可用来检查校园域名的分流情况 |

日常所说的“开着 VPN”，在本文指开启 Clash 类代理。学校 WebVPN 提供校园资源访问入口。把某个网站设成直连，只会改变请求走哪条路，不会增加内网路由或账号权限。

## 方法一：Clash Verge Rev + Windows 系统代理

适用条件：**系统代理已开启、TUN 关闭、系统代理使用默认模式（非 PAC）**。浏览器还应使用系统代理，未单独指定其他代理。

1. 打开 Clash Verge Rev → **设置 → 系统代理的设置按钮**，进入“系统代理设置”。不同版本可显示为齿轮或设置图标。
2. 先复制保存现有“代理绕过”内容，便于恢复。
3. 关闭 **“始终使用默认绕过”**。已有内容继续保留；若切换后自动填入默认条目，也保留它们。
4. 在“代理绕过”的文本末尾追加下面这一段，**包括开头的半角分号**：

   ```text
   ;bjut.edu.cn;*.bjut.edu.cn
   ```

5. 点击 **保存**。若界面是逐项编辑，就分别新增 `bjut.edu.cn` 和 `*.bjut.edu.cn` 两项，不输入分号。
6. 关闭校园网页的旧标签页，重新打开门户。仍沿用旧连接时，重启浏览器再试。

已有条目以分号结尾时，不重复加分号；完全空白的文本框填 `bjut.edu.cn;*.bjut.edu.cn`。不要把示例当成整份绕过列表替换进去，以免丢掉已有局域网条目。

这里填写**主机名或匹配项**，不填写 `https://`、路径、`DIRECT` 或 YAML。Windows 使用半角分号；macOS / Linux 不应直接照搬这段分隔符，优先使用下面的规则分流方法。

系统代理绕过是客户端的应用设置，独立于订阅内容；正常更新订阅不会改写这项设置。导入备份、重置客户端或其他软件接管系统代理时，需重新检查。

依据：[官方绕过说明](https://www.clashverge.dev/guide/bypass.html)、[系统代理设置源码](https://github.com/clash-verge-rev/clash-verge-rev/blob/dev/src/components/setting/mods/sysproxy-viewer.tsx)。[更详细的操作与恢复方式](docs/clash-verge.md)。

## 方法二：Clash Verge Rev + 规则分流（含 TUN）

这种方法让请求进入 Clash 后，由规则选择 **DIRECT（直连）**。对已被 TUN 接管的请求，也可以用相同的校园域名规则分流。

1. 把客户端的出站模式设为 **规则 / Rule**。
2. 打开 **订阅**，右键当前正在使用的订阅卡片 → **编辑规则**。
3. 在 **前置规则 / Prepend** 新增一条：

   | 字段 | 填写内容 |
   |---|---|
   | 类型 | `DOMAIN-SUFFIX` |
   | 内容 | `bjut.edu.cn` |
   | 策略 / 出站 | `DIRECT` |

4. 保存并确认配置应用成功；有报错时先处理报错，再访问校园网页。
5. 重新建立连接，在客户端的 **连接 / Connections** 页面检查该请求的出口是否为 `DIRECT`。

对应规则是：

```yaml
- DOMAIN-SUFFIX,bjut.edu.cn,DIRECT
```

它覆盖 `bjut.edu.cn` 本身以及 `my.bjut.edu.cn`、`webvpn.bjut.edu.cn` 等子域名。**在 `DOMAIN-SUFFIX` 中填 `bjut.edu.cn`，不要填 `*.bjut.edu.cn`。**

前置规则保存在该订阅的本地扩展中，官方说明其不会因正常订阅更新而失效。切换其他订阅后，要在新订阅中检查或添加相同规则。规则从上到下匹配，校园规则应放在可能提前匹配它的规则以及 `MATCH` 之前。

依据：[官方图文教程](https://www.clashverge.dev/guide/rules.html)、[Mihomo 路由规则](https://wiki.metacubex.one/config/rules/)。

## FlClash 怎么做？

当前 **v0.9.2** 可通过 **工具 → 进阶配置 → 附加规则** 新增 `DOMAIN-SUFFIX / bjut.edu.cn / DIRECT`，并使用规则出站模式。还需确认当前订阅采用会加载全局附加规则的标准扩展方式。

具体保存、订阅检查及版本依据见 [FlClash 操作页](docs/flclash.md)。较早教程中的“覆写”菜单与新版本有变化，按对应版本阅读。

## 三个最容易混淆的地方

| 项目 | 它控制什么 | 本例怎么填 |
|---|---|---|
| 系统代理绕过 | 遵循系统代理的应用是否把请求交给代理 | Windows 文本项：`bjut.edu.cn;*.bjut.edu.cn` |
| 内核规则分流 | 已进入 Clash 的请求选择哪个出口 | `DOMAIN-SUFFIX,bjut.edu.cn,DIRECT` |
| TUN 流量排除 | 指定网段是否被 TUN 路由接管 | 属于另一个设置；只在明确需要排除已知网段时配置 |

开启 TUN 后，单加系统代理绕过不能保证这些请求也避开 TUN；请使用校园直连规则，并检查实际连接。`DIRECT` 通常仍由内核处理连接，和“完全不经过客户端”是两种实现。

如果你还想让全部 `.cn` 域名直连：Windows 绕过项可新增 `*.cn`；规则可新增 `DOMAIN-SUFFIX,cn,DIRECT`。这会扩大直连范围。它覆盖 `bjut.edu.cn`，**不覆盖 `cn.overleaf.com`、`kns.cnki.net` 或国内网站使用的其他 `.com` / `.net` 域名**。本教程默认只加校园域名。

## 怎样确认生效？

1. 开着代理，重新打开你原本有权限访问的校园资源，检查页面和实际功能是否正常。
2. 使用规则分流时，在连接页搜索请求的实际主机名，确认最终出口是 `DIRECT`。
3. 使用纯系统代理绕过且 TUN 关闭时，请求通常不会出现在 Clash 连接页；“看不到连接”本身不能证明网站已经访问成功。
4. 更新一次订阅后，重新检查规则/绕过项和一次新连接，确认自己的版本确实保留设置。

HTTP `200` 只表示某次请求收到了成功响应，不能单独证明已经完成登录、具有资源权限或走了直连出口。

## 可以直接分享的话

> 开着 Clash 时校园网打不开，可以把校园域名设为直连。BJUT 的例子：Clash Verge 在 Windows 系统代理绕过里保留原内容，再追加 `;bjut.edu.cn;*.bjut.edu.cn`；开启 TUN 或使用 FlClash 时，用规则模式添加 `DOMAIN-SUFFIX,bjut.edu.cn,DIRECT`。校外访问内网仍需学校 WebVPN 等授权入口。完整步骤见本仓库。

## 资料、示例与验证范围

- [现成教程与官方资料索引](docs/references.md)
- [通用规则、可复制配置片段与其他客户端](docs/common-rules.md)
- [打不开时的排查表及恢复方法](docs/troubleshooting.md)
- [校园规则片段](examples/campus-rules.yaml) / [Windows 绕过追加片段](examples/verge-bypass-windows.txt)

本文核对了公开校方说明、客户端文档及标注版本的源码；示例文件做了语法和匹配范围检查。未在所有客户端、手机和校园网络环境中逐一实机测试，也未执行校园业务登录。具体界面和网络效果应按上面的步骤在自己的环境验证。

本仓库为个人整理的交流教程。原创文字和示例按 [MIT License](LICENSE) 提供；链接到的教程、软件及校方资料仍遵循其各自条款。
