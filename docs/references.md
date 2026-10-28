# 现成教程和来源索引

[返回首页](../README.md)

本仓库在现成文档上增加 BJUT 案例、客户端入口对照和排查顺序。正文及示例独立编写；以下链接保留原作者和来源，不整篇搬运上游教程，也不包含其软件安装包或节点订阅。

## 客户端与内核官方资料

| 资料 | 看哪里 |
|---|---|
| [Clash Verge Rev：代理绕过](https://www.clashverge.dev/guide/bypass.html) / [GitHub 原文](https://github.com/clash-verge-rev/clash-verge-rev.github.io/blob/main/docs/guide/bypass.md) | 系统代理默认模式、PAC、TUN 排除、自定义路由规则；网页包含操作演示 |
| [Clash Verge Rev：自定义规则](https://www.clashverge.dev/guide/rules.html) / [GitHub 原文](https://github.com/clash-verge-rev/clash-verge-rev.github.io/blob/main/docs/guide/rules.md) | 订阅卡片“编辑规则”、前置规则、正常更新后的保留方式 |
| [Clash Verge Rev：扩展配置与脚本](https://www.clashverge.dev/guide/extend.html) | 配置合并和脚本执行顺序，防止无意覆盖整份规则 |
| [Mihomo：路由规则](https://wiki.metacubex.one/config/rules/) / [GitHub 原文](https://github.com/MetaCubeX/Meta-Docs/blob/main/docs/config/rules/index.md) | DOMAIN、DOMAIN-SUFFIX、IP-CIDR、no-resolve 和优先级 |
| [Mihomo：TUN](https://wiki.metacubex.one/config/inbound/tun/) / [DNS](https://wiki.metacubex.one/config/dns/) | 校园 VPN、路由和解析发生冲突时的进一步参考 |
| [FlClash 官方仓库](https://github.com/chen08209/FlClash) / [发布记录](https://chen08209.github.io/FlClash/) | 确认软件来源、版本变化；本仓库 FlClash 入口以 v0.9.2 源码核对 |

## 已有社区教程

| 资料 | 适用范围 |
|---|---|
| [Clash 多客户端 Wiki](https://github.com/clashbk/clash/wiki/) | 集中查找不同客户端的基础界面；部分页面包含旧版内容和推广 |
| [FlClash 基础教程](https://github.com/clashbk/clash/wiki/flclash) | 导入、开启代理和出站模式；不是当前版本全部菜单的依据 |
| [FlClash 仓库 Issue #1510：动态化配置与覆写分组规则](https://github.com/chen08209/FlClash/issues/1510) | 社区作者在官方仓库分享的截图与脚本教程，适合了解进阶分流；不等于维护者官方教程 |

桌面系统代理可用绕过项；需要内核分流时，用附加/前置规则即可，无需导入完整的第三方策略组脚本。FlClash v0.9 系列调整过配置扩展方式，旧教程的菜单需对照版本。

## BJUT 校方公开说明

- [教务处：新修课程选课通知（2025 年秋季学期课程）](https://undergrad.bjut.edu.cn/info/1213/3603.htm)：在“选课方式”中明确列出校内 `my.bjut.edu.cn` 和校外 `webvpn.bjut.edu.cn`。
- [学校：2026 届夏季毕/结业生离校工作通知](https://job.bjut.edu.cn/info/1048/4457.htm)：再次说明门户校外访问需经学校 VPN。
- [图书馆：电子资源访问系统](https://lib.bjut.edu.cn/wx_xwfw/dzzyfwxt.htm)：查学校图书馆资源的校内/校外入口。

上述历史通知用于核对公开域名和访问方式，不用于推断当前业务办理时间。资源入口和认证要求以学校当前公告为准。
